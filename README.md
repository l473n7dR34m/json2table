# json2table

`json2table` is a lightweight CLI tool for converting JSON arrays of objects into readable tables.  
It is designed for inspecting API responses, logs, and structured data directly in the terminal, or exporting as Markdown or CSV for documentation.

The default output is a clean, plain aligned table optimised for terminal readability.

---

## Features

- Reads JSON from a file or stdin
- Outputs:
  - Plain aligned table (default)
  - Markdown table (`--md`)
  - CSV (`--csv`)
- Flatten nested objects into `dot.notation` columns (`--flatten`)
- Select and order columns (`--fields`)
- Sort rows (`--sort`, with `--desc`)
- Limit row count (`--limit`)
- Zero dependencies (pure Python standard library)

---

## Installation

```bash
git clone https://github.com/<your-user>/json2table.git
cd json2table
chmod +x json2table.py
```

Optional: add to PATH:

```bash
sudo cp json2table.py /usr/local/bin/json2table
```

---

## Usage

### Basic usage
```bash
json2table data.json
```

### Read from stdin
```bash
cat data.json | json2table
```

### Flatten nested objects
```bash
json2table data.json --flatten
```

### Select fields
```bash
json2table data.json --fields id,name,status
```

### Sort and limit
```bash
json2table data.json --sort price --desc --limit 5
```

### Markdown output
```bash
json2table data.json --md
```

### CSV output
```bash
json2table data.json --csv > out.csv
```

---

## Input Format

Top-level JSON must be:

A list of objects:

```json
[
  { "id": 101, "name": "Widget Alpha", "price": 29.99 },
  { "id": 102, "name": "Widget Beta", "price": 34.50 }
]
```

or a single object:

```json
{ "id": 101, "name": "Widget Alpha", "price": 29.99 }
```

---

## Examples

### Example input (`test.json`)

```json
[
  {
    "id": 101,
    "name": "Widget Alpha",
    "price": 29.99,
    "status": "active",
    "categories": ["hardware", "tools"],
    "inventory": {
      "location": "Warehouse A",
      "stock": 120
    }
  },
  {
    "id": 103,
    "name": "Gadget Pro",
    "price": 89.00,
    "status": "inactive",
    "categories": ["electronics", "gadgets"],
    "inventory": {
      "location": "Warehouse C",
      "stock": 0
    }
  }
]
```

### Default output

```text
id  | name         | price | status   | categories                 | inventory                                
----+--------------+-------+----------+----------------------------+------------------------------------------
101 | Widget Alpha | 29.99 | active   | ["hardware", "tools"]      | {"location": "Warehouse A", "stock": 120}
103 | Gadget Pro   | 89.0  | inactive | ["electronics", "gadgets"] | {"location": "Warehouse C", "stock": 0}  
```

### Flattened output

```text
id  | name         | price | status   | categories                 | inventory.location | inventory.stock
----+--------------+-------+----------+----------------------------+--------------------+----------------
101 | Widget Alpha | 29.99 | active   | ["hardware", "tools"]      | Warehouse A        | 120            
103 | Gadget Pro   | 89.0  | inactive | ["electronics", "gadgets"] | Warehouse C        | 0              
```

### Markdown output

```md
| id | name | price | status | categories | inventory |
| --- | --- | --- | --- | --- | --- |
| 101 | Widget Alpha | 29.99 | active | ["hardware", "tools"] | {"location": "Warehouse A", "stock": 120} |
| 103 | Gadget Pro | 89.0 | inactive | ["electronics", "gadgets"] | {"location": "Warehouse C", "stock": 0} |
```

---

## macOS Clipboard Workflow (optional)

Convert copied JSON → Markdown → back into clipboard:

```bash
pbpaste | json2table --md | pbcopy
```

Add an alias:

```bash
echo "alias j2t='pbpaste | json2table --md | pbcopy'" >> ~/.zshrc
source ~/.zshrc
```

---

## License

MIT License.
