# Markdown Object Language (MOL)

**A simple and easy to use object notation language based on markdown.** 

Easy to use for humans and machines (LLMs).

Status: ``DRAFT / IN PROGRESS``

In the new world of LLMs and agents, markdown has become a goto format for describing things. Sometimes it's nice to stay in the format for storing structured data, configurations and more.

MOL is also a bit easier and friendlier to edit than other formats such as JSON. And there is more than one way to express the same object. This makes MOL looser and less prone to parsing errors, especially when coming from a LLM model.

Quick Installs:
- JS/TS: ```npm install mol-format```
- Rust: ```cargo install serde_mol```
- .net: ```dotnet add package Mol.Format```


## Example

JSON (626 chars, ~215 tokens):

```json
{
    "id": 10,
    "username": "janedoe",
    "fullname": "Jane Doe",
    "birthDate": "1989-01-31T23:00:00.000Z",
    "password": {
        "hash": "aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A",
        "algorithm": "argon2id",
        "version": 19
    },
    "addresses": [
        {
          "name": "business",
          "street": "123 Main St.",
          "city": "Zurich",
          "postalCode": "8001",
          "country": "CH"
        },
        {
          "name": "home",
          "street": "123 Suburbia",
          "city": "Zurich",
          "postalCode": "8045",
          "country": "CH"
        }
    ]
}
```

MOL (319 chars, ~130 tokens):

```md
# User 10

Id: 10
Username: janedoe
Full name: Jane Doe

## Password

Hash: aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A
Algorithm: argon2id
Version: 19

## Addresses

### Business

Street: 123 Main St.
City: Zurich
Postal Code: 8001
Country: CH

### Home

Street: 123 Suburbia
City: Zurich
Postal Code: 8045
Country: CH

```

or

```md
# User 10
Id: 10
Username: janedoe
Full name: Jane Doe
Password:
    Hash: aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A
    Algorithm: argon2id
    Version: 19
Addresses:
    Business:
        Street: 123 Main St.
        City: Zurich
        Postal Code: 8001
        Country: CH
    Home:
        Street: 123 Suburbia
        City: Zurich
        Postal Code: 8045
        Country: CH
```


<details>
<summary>more alternates</summary>

or

```md
# User 10
// This is a comment
- Id: 10
- Username: janedoe
- Full name: Jane Doe
- Password:
    - Hash: aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A
    - Algorithm: argon2id
    - Version: 19
- Addresses:
    1. Business:
        - Street: 123 Main St.
        - City: Zurich
        - Postal Code: 8001
        - Country: CH
    2. Home:
        - Street: 123 Suburbia
        - City: Zurich
        - Postal Code: 8045
        - Country: CH
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

### Typescript

- Official Package: ```npm install mol-format```
- Official Repo: [https://github.com/mol-format/mol-ts](https://github.com/mol-format/mol-ts)

### JavaScript

- Official Package: ```npm install mol-format```
- Official Repo: [https://github.com/mol-format/mol-ts](https://github.com/mol-format/mol-ts)

### Rust

- Official Package: ```cargo install serde_mol```
- Official Repo: [https://github.com/mol-format/mol-rs](https://github.com/mol-format/mol-rs)

### .NET

- Official Package: ```dotnet add package Mol.Format```
- Official Repo: [https://github.com/mol-format/mol-net](https://github.com/mol-format/mol-net)

### C++

- Official Package: TODO
- Official Repo: [https://github.com/mol-format/mol-cpp](https://github.com/mol-format/mol-cpp)

### Other

Enter this prompt into your favorite coding tool: 

```Implement a markdown object file reader/writer for MOL Format as specified by https://github.com/mol-format/mol-specs/blob/main/SPECS.md```



## Comments

Comments are line-sytle ``// comment `` or block-style  ``/* comment */``. Why? Many languages use this format and it doesn't conflict with MD.



## LLM-Compatibility

Markdown Object Language is highly compatible with large language models. Because they are trained on a lot of markdown data, for them its basic instinct to write MDC. 

For example, you can ask a LLM (without any knowledge about MOL), to convert MOL to JSON:


<details>
    
<summary>Example of LLM built-in capability</summary>

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

```js
{
  "id": 10,
  "hash": "9b87da04349185080af34e04895d4d66",
  "properties": {
    "length": {
      "type": "number",
      "enabled": false,
      "value": -1
    },
    "description": {
      "type": "string",
      "enabled": true,
      "value": "example"
    }
  }
}
```

</details>



## FAQ

### Why use MOL?

- Because it can be easier for humans and machines to write and edit, especially if already making extensive use of markdown.

### Why not use TOML or YAML instead?

- Yeah, those are great alternatives. Use them.

### When not to use MOL

- If you have deeply nested data
- If value types must be fully defined in the file (not in the deserialization)

### Can I use MOL if I don't need to interact with LLMs?

- Yes absolutely. It's fast and easy to edit for humans.


## Specs

[SPECS.md](https://github.com/mol-format/mol-specs/blob/main/SPECS.md)




## Examples

<details>
    
<summary>Example of Skill.mol</summary>

```md
# CSV Insights Skill

Name: csv-insights
Description: Summarize a CSV, compute basic stats, and produce a markdown report + a plot image. Use when the user asks to analyze, summarize, or visualize data from a .csv file.

## Overview
This skill provides automated analysis for CSV data files using pandas and matplotlib.

## Instructions
1. Analyze Data: Load the CSV file and check for missing values or data type inconsistencies.
2. Generate Stats: Calculate mean, median, and standard deviation for all numerical columns.
3. Create Visualization: Generate a bar chart or line plot based on the most relevant numerical trends.
4. Format Report: Output a structured Markdown report including the "Missing Values" section and a summary of key findings.

## Examples
- Input: "Analyze sales_data.csv"
- Output: A summary table of total sales per region and a PNG chart showing monthly trends.

## Constraints
- Do not modify the original CSV file.
- Limit visualizations to a maximum of two plots per report.
```

>>>

```js
{
  "name": "csv-insights",
  "description": "Summarize a CSV, compute basic stats, and produce a markdown report + a plot image. Use when the user asks to analyze, summarize, or visualize data from a .csv file.",
  "overview": "This skill provides automated analysis for CSV data files using pandas and matplotlib.",
  "instructions": [
    "Analyze Data: Load the CSV file and check for missing values or data type inconsistencies.",
    "Generate Stats: Calculate mean, median, and standard deviation for all numerical columns.",
    "Create Visualization: Generate a bar chart or line plot based on the most relevant numerical trends.",
    "Format Report: Output a structured Markdown report including the \"Missing Values\" section and a summary of key findings."
  ],
  "examples": [
    {
      "input": "Analyze sales_data.csv",
      "output": "A summary table of total sales per region and a PNG chart showing monthly trends."
    }
  ],
  "constraints": [
    "Do not modify the original CSV file.",
    "Limit visualizations to a maximum of two plots per report."
  ]
}
```

</details>
