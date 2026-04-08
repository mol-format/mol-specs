# Markdown Object Language (MOL) Specification

STATUS: DRAFT / IN PROGRESS
VERSION: 0.1.0

## Overview

Markdown Object Language (MOL) is a nested object notation built on top of markdown-like text.
It is intended for human-editable structured data such as records, schemas, configuration files, and other markdown-adjacent object documents.

MOL is deliberately flexible. Multiple markdown-shaped forms can describe the same object tree.
This flexibility is intended to make MOL practical for both humans and LLMs.




## Intended Use and Limits

MOL is a good fit when authors want to stay close to normal markdown while storing structured data.

Common uses include:

- Records
- Schemas
- Configuration files
- LLM-facing structured documents

MOL is intentionally text-first and loose. This has tradeoffs:

- It is less suitable for deeply nested structures.
- Primitive types are not fully defined by the markup itself.
- Higher-level deserializers are expected to apply schema, typing, and native object/array mapping rules.




## Data Model

A MOL document represents a recursively nested object.

Each object contains an ordered list of entries:

- Each entry has a key.
- A key may map to either a scalar value or another object.
- Keys are not required to be unique.
- Repeated sibling keys represent repeated entries and may be deserialized as arrays.

Examples:

- `Property:` repeated multiple times means `Property` is an array-like key.
- `Properties:` followed by distinct child keys means `Properties` is an object-like key.

Parsers should preserve source order. Higher-level deserializers may choose how to map repeated keys to arrays or records.




## Structural Forms

The following forms are structurally equivalent when they describe the same tree:

- Headings
- Plain `Key: Value` entries
- Plain `Key:` or `Key` object entries
- Markdown list items using `-`, `*`, `+`, or `1.`

### Root Heading Level

Headings are structural. 

The first structural heading level used in a document becomes heading level 1 for that document's structure.

Examples:

- If the first structural heading is `#`, then `#` is level 1, `##` is level 2, and so on.
- If the first structural heading is `##`, then `##` is level 1, `###` is level 2, and so on.

Multiple root headings at that same normalized level are valid:

```md
# Level 1 A
Id: 10

# Level 1 B
Id: 11
```

These are sibling entries at the document root, and define the resulting root object to be an array.

By convention, authors may use the first heading as a human-facing document title or description.

### Key-Value Entry

A single-line property is written as:

```md
Key: Value
```

This creates a scalar entry named `Key` with the text value `Value`.

### Key-Only Entry

An entry with deferred content may be written as either:

```md
Key:
```

or

```md
Key
```

This creates an entry named `Key`.

Its content is determined by what follows:

- If nested structural entries follow, `Key` is an object entry.
- If a text body follows, `Key` is a scalar text entry.

### Heading Entry

Headings create named object scopes:

```md
## Properties
### Length
Type: number
```

This is equivalent to:

```md
Properties:
    Length:
        Type: number
```

Headings may also take a text body instead of child entries.

Heading depth is determined solely by the number of ``#`` markers, normalized to the document's shallowest
  structural heading level. Indentation does not affect heading depth.

### List Entry

List markers are syntactic sugar and do not change the data model:

```md
- Id: 10
* Hash: abc
1. Enabled: true
```

All of the following list markers are valid:

- `-`
- `*`
- `+`
- `1.`, `2.`, etc.

List syntax alone does not imply array semantics.
Array-like behavior comes from repeated sibling entries or from higher-level deserializer conventions.

## Nesting Rules

### Indentation-Based Nesting

For plain entries and list entries, nesting is determined by indentation:

- Greater indentation means child content.
- Returning to a previous indentation closes the nested scope.
- Parsers should accept tabs or consistent spaces.
- Writers should prefer tab characters for emitted indentation.

Indented content may be pasted from elsewhere without being reformatted, as long as its relative indentation remains intact.

### Heading-Based Nesting

For headings, nesting is determined by normalized heading level:

- The shallowest structural heading level in the document is level 1.
- The next deeper heading level is its child level.
- Headings at the same normalized depth are siblings.

Examples:

- If the root heading level is `#`, then `##` is a child of `#`.
- If the root heading level is `##`, then `###` is a child of `##`.

Indentation does not change heading depth. This allows heading blocks to be indented for readability or pasted into larger structures without changing their semantic level.

### Mixed Forms

Heading-based and indentation-based forms may be mixed in the same document.

For example, a heading may contain plain entries, list entries, or additional headings.




## Scalar Values

MOL itself is text-first and does not impose a strict primitive type system.

At the core syntax layer, scalar values are preserved as strings.

Values such as `10`, `false`, `number`, and `example` are written as text. Higher-level deserializers may coerce unquoted scalar strings to numbers, booleans, enums, or other domain-specific types.

Scalar values may be expressed either inline or as a text body.

Inline scalar example:

```md
Id: 10
```

Text-body scalar example:

```md
## Id

10
```

### Inline Quoted Values

An inline scalar whose entire value is wrapped in matching single quotes or matching double quotes is an explicit string literal.

Supported forms:

```md
Year: "2025"
Flag: 'false'
```

Quoted inline scalar values:

- Must occupy the full inline value after `Key:`
- Are preserved as strings by the parser
- Must not be auto-coerced by the parser to numbers, booleans, or other native types

Only full-value quoting has special meaning. Partial quoting inside an otherwise unquoted inline value is not defined as a distinct scalar form by the core syntax.

Basic escapes are supported inside quoted inline values:

- `\\`
- `\"`
- `\'`
- `\n`
- `\r`
- `\t`

Example:

```md
Message: "line 1\nline 2"
Path: 'C:\\temp\\cache'
Quote: "\"hello\""
```

### Date and Datetime Conventions

ISO 8601 date and datetime values are an officially supported scalar convention.

Examples:

```md
Date: 2026-04-02
Timestamp: 2026-04-02T18:30:45Z
Timestamp With Offset: 2026-04-02T20:30:45+02:00
```

ISO 8601 date and datetime values are preserved as strings by the parser and must not be auto-coerced to native date objects at the syntax layer.

Higher-level deserializers may interpret these string values according to schema or application rules.

### Text-Body Values

A text body is a scalar value formed by plain text lines that appear beneath a heading entry or a key-only entry.

Examples:

```md
Description:
    This is a single-line value.
```

```md
## Description

This is line one.
This is line two.
```

```md
## Notes

First paragraph.

Second paragraph.
```

Text-body rules:

- A text body begins when the first non-blank, non-comment child line of a heading entry or key-only entry is plain text rather than another structural entry.
- A text body may contain multiple lines.
- Blank lines inside the text body are preserved as part of the value.
- A text body ends when the parser encounters the next sibling or ancestor structural entry, or the end of the document.
- A heading entry or key-only entry cannot contain both a text body and child structural entries at the same scope.

For indentation-based entries, the text body is formed from the indented child lines belonging to that entry.

For heading entries, the text body is formed from the subsequent plain text lines until another heading at the same or shallower normalized level, another enclosing structural boundary, or end of document.

### Escaping Text Bodies

If a text-body line would otherwise be parsed as MOL structure, authors should escape it using normal markdown escaping or wrap the value in a fenced code block.

Examples:

```md
## Example

\# not a heading
\- not a list item
Key\: not a key-value entry
```

````md
## Script

```txt
# literal heading text
Name: literal text
- literal list text
```
````

When a fenced code block is used as a text body, its contents are taken literally as the scalar value body until the closing fence.




## Arrays and Duplicate Keys

Sibling keys are allowed to repeat.

Repeated keys represent repeated entries:

```md
Property:
    Name: length
Property:
    Name: description
```

This is the canonical array-like pattern in MOL.

Distinct sibling keys represent a keyed object:

```md
Properties:
    Length:
    Description:
```

Whether repeated keys are exposed as arrays, multisets, or ordered duplicate fields is an implementation choice above the syntax layer, but source order should be preserved.

Sequence-style sections such as `Instructions` or `Constraints` may also be deserialized as native arrays when the target schema expects ordered items.
That mapping is a deserializer convention above the MOL syntax layer.




## Comments and Whitespace

Blank lines may be used freely to improve readability.

The following comment forms are extensions to markdown and are ignored by MOL parsers:

- Line comments: `// comment`
- Block comments: `/* comment */`

Comments may appear anywhere a blank line could appear.

Thematic breaks (---) may be used anywhere where a blank line could appear.





## Markdown Compatibility

### Markdown Backwards Compatibility

MOL intentionally reuses common markdown constructs:

- Headings: `# Heading`
- Unordered lists: `- Item`, `* Item`, `+ Item`
- Ordered lists: `1. Item`

### MOL Extensions

MOL extends markdown with:

- Object semantics for headings, list items, and `Key: Value` lines
- `//` line comments
- `/* ... */` block comments

## Minimal Parsing Guidance

A conforming parser should:

- Ignore comments and blank lines
- Recognize the document's root heading level
- Recognize heading scopes
- Recognize plain and list-based key/value entries
- Recognize key-only entries
- Recognize full-value single-quoted and double-quoted inline string literals
- Decode the basic quoted inline escapes `\\`, `\"`, `\'`, `\n`, `\r`, and `\t`
- Recognize text-body scalar values, including multiline text
- Preserve scalar values as strings at the syntax layer, including ISO 8601 date and datetime forms
- Preserve entry order
- Allow duplicate sibling keys
- Support indentation-based nesting for non-heading entries




## Conventions

### Root Heading

The root heading does not need to start at `#`. A document may begin structurally at `#`, `##`, or any deeper heading level.

The first heading depth used for structure becomes normalized level 1 for that document.

Multiple root headings at that normalized level are allowed.

### Preferred Shorthand

Use repeated keys for array-like data and headings for major sections.

Tabs are the preferred indentation style for emitted files, though parsers should be liberal in what they accept.

### File Endings

Official file extension for MOL files: `.mol`

#### Alternate Naming (TBD):

Official file extension for objects: `.mdo` or `.molo`
Official file extension for configurations: `.mdc` or `.molc` (could be useful to easily spot config files, avoids things like tsconfig.json - would be typescript.molc)




## Implementations

- Language specific implementations of MOL should fully comply with the official specifications in all matters.
- Every implementation should be efficient in memory and processing speed, as well as code complexity.
- The codebase should be lean and clean, and easy to understand, maintain and verify.
- Implementation distributables should be dependency free (so long it makes sense).

### Key-Name Normalization

MOL does not define key-name normalization as part of the core format.
Transformations such as camelCase, PascalCase, snake_case, or kebab-case are implementation-specific.

If an implementation exposes named normalization modes, the following terms should be used with these meanings:

- `identity`
  Preserve the key as written, without changing separators or letter casing.
  Example: `Full name` -> `Full name`

- `camelCase`
  Split the key into words, remove separators, lowercase the first word, and capitalize subsequent words.
  Example: `Full name` -> `fullName`

- `PascalCase`
  Split the key into words, remove separators, and capitalize each word.
  Example: `Full name` -> `FullName`

- `snake_case`
  Split the key into words, lowercase them, and join them with underscore characters.
  Example: `Full name` -> `full_name`

- `kebab-case`
  Split the key into words, lowercase them, and join them with hyphen characters.
  Example: `Full name` -> `full-name`

Implementations should document their exact handling of edge cases such as:

- Acronyms and initialisms
- Repeated separators
- Leading digits
- Non-ASCII letters
- Punctuation other than space, underscore, or hyphen

## Tests

Each implementation must be able to pass all the tests provided by the mol-specs tests (in ./tests).
See ./tests/README.md for more information on tests.
