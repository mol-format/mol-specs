# Markdown Object Language (.mol)

A simple and easy to use object notation language based on markdown.

Easy to use for humans and machines (LLMs).

Status: DRAFT / IN PROGRESS

## Example

JSON:

```json
{
    "id": 10,
    "hash": "9b87da04349185080af34e04895d4d66",
    "properties": [
        {
            "name": "length",
            "type": "number",
            "enabled": false,
            "value": -1
        },
        {
            "name": "description",
            "type": "string",
            "enabled": true,
            "value": "example"
        }
    ],
}
```

MOL:

```md
# Example Record 10

Id: 10
Hash: 9b87da04349185080af34e04895d4d66

## Properties

### Length

Type: number
Enabled: false
Value: -1

### Description

Type: string
Enabled: true
Value: example
```

or

```md
# Example Record 10

Id: 10
Hash: 9b87da04349185080af34e04895d4d66
Properties:
    Length:
        Type: number
        Enabled: false
        Value: -1
    Description
        Type: string
        Enabled: true
        Value: example
```


<details>
<summary>more alternates</summary>

or

```md
# Example Record 10
// This is a comment

- Id: 10
- Hash: 9b87da04349185080af34e04895d4d66
- Properties:
    - Length:
        - Type: number
        - Enabled: false
        - Value: -1
    - Description
        - Type: string
        - Enabled: true
        - Value: example
```

or

```md
# Example Record 10

## Id

10

## Hash

9b87da04349185080af34e04895d4d66

## Properties

### Length

Type: number
Enabled: false
Value: -1

### Description

Type: string
Enabled: true
Value: example
```

or

```md
# Example Record 10

Id: 10
Hash: 9b87da04349185080af34e04895d4d66

## Properties

Length.Type: number
Length.Enabled: false
Length.Value: -1

Description.Type: string
Description.Enabled: true
Description.Value: example

```

or

```md
# Example Record 10

Id: 10
Hash: 9b87da04349185080af34e04895d4d66

Properties.Length.Type: number
Properties.Length.Enabled: false
Properties.Length.Value: -1

Properties.Description.Type: string
Properties.Description.Enabled: true
Properties.Description.Value: example

```

</details>

## Benefits

- Human and LLM friendly
- Easy and flexible editing
- Fast and easy parsing
- Looser format, types defined by user (deserializer), not markup

## Not good for...

- Deeply nested structures

## Implementations

### C++

TODO

### JavaScript

TODO

### .NET

TODO

### Typescript

TODO

### Rust

TODO

### Other

Enter this prompt into your favorite coding tool: 

```Implement a markdown configuration file reader/writer specified by https://github.com/markdown-config/markdown-config/SPECS.md```

## Comments

Comments are line-sytle ``// comment `` or block-style  ``/* comment */``. Why? Many languages use this format and it doesn't conflict with MD.

## LLM-Compatibility

Markdown Object Language is highly compatible with large language models. Because they are trained on a lot of markdown data, for them its basic instinct to write MDC. 

For example, you can ask a LLM (without any knowledge about MOL), to convert MOL to JSON:


<details>
    
<summary>examples of LLM capability</summary>

```txt
convert this into JSON format:

# Example Record 10
// This is a comment

Id: 10
Hash: 9b87da04349185080af34e04895d4d66

## Properties

### Length

Type: number
Enabled: false
Value: -1

### Description

Type: string
Enabled: true
Value: example
```

>>>

```txt
{
  "Id": 10,
  "Hash": "9b87da04349185080af34e04895d4d66",
  "Properties": {
    "Length": {
      "Type": "number",
      "Enabled": false,
      "Value": -1
    },
    "Description": {
      "Type": "string",
      "Enabled": true,
      "Value": "example"
    }
  }
}
```

</details>

## See Also

[SPECS.md](SPECS.md)
