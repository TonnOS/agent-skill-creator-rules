---
name: {{SKILL-NAME}}
description: |
  {{FUNCTIONALITY}} using bundled scripts. Use when:
  (1) {{USE_CASE_1}}
  (2) {{USE_CASE_2}}
metadata:
 {
 "openclaw": {
   "requires": { "bins": ["python3"] }
 }
 }
---

# {{SKILL-NAME}}

{{BRIEF_OVERVIEW}}

## Available Scripts

| Script | Purpose |
|--------|---------|
| `scripts/{{SCRIPT_1}}.py` | {{SCRIPT_1_DESC}} |
| `scripts/{{SCRIPT_2}}.py` | {{SCRIPT_2_DESC}} |

## Usage

### {{OPERATION_1}}

```bash
python3 {baseDir}/scripts/{{SCRIPT_1}}.py {{ARGS}}
```

### {{OPERATION_2}}

```bash
python3 {baseDir}/scripts/{{SCRIPT_2}}.py {{ARGS}}
```

## Options

| Flag | Description |
|------|-------------|
| `-o, --output` | Output file path |
| `-v, --verbose` | Enable verbose logging |
| `-h, --help` | Show help message |

## Examples

```bash
# {{EXAMPLE_DESC}}
python3 {baseDir}/scripts/{{SCRIPT}}.py {{EXAMPLE_ARGS}}
```
