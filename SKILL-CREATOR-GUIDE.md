# OpenClaw Skill Creator Guide

A comprehensive guide for creating effective OpenClaw skills following the AgentSkills specification.

---

## Table of Contents

1. [What Are Skills?](#what-are-skills)
2. [Skill Structure](#skill-structure)
3. [YAML Frontmatter](#yaml-frontmatter)
4. [Skill Body Best Practices](#skill-body-best-practices)
5. [Bundled Resources](#bundled-resources)
6. [Progressive Disclosure Pattern](#progressive-disclosure-pattern)
7. [Gating & Requirements](#gating--requirements)
8. [Publishing to ClawHub](#publishing-to-clawhub)
9. [Templates](#templates)
10. [Examples](#examples)

---

## What Are Skills?

Skills are modular packages that extend OpenClaw's capabilities by providing:
- **Specialized workflows** - Multi-step procedures for specific domains
- **Tool integrations** - Instructions for working with APIs, CLIs, file formats
- **Domain expertise** - Company-specific knowledge, schemas, business logic
- **Bundled resources** - Scripts, references, and assets for complex tasks

### Key Principle

**Context window is a public good.** Skills share context with everything else. Default assumption: the AI is already smart. Only add context it doesn't already have.

---

## Skill Structure

```
skill-name/
├── SKILL.md (required)
│   ├── YAML frontmatter (required)
│   └── Markdown body (required)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation loaded as needed
    └── assets/           - Files used in output (templates, etc.)
```

### Naming Conventions

- Lowercase letters, digits, and hyphens only
- Prefer short, verb-led phrases: `pdf-editor`, `git-helper`, `api-client`
- Namespace by tool when helpful: `gh-address-comments`, `linear-create-issue`
- Keep under 64 characters

---

## YAML Frontmatter

The frontmatter is **critical** — it determines when the skill triggers.

### Minimal Example

```yaml
---
name: my-skill
description: Brief description of what this skill does and when to use it.
---
```

### Complete Example

```yaml
---
name: pdf-editor
description: |
  Create, edit, and manipulate PDF documents with support for merging, 
  splitting, rotation, text extraction, and form filling. Use when working 
  with PDF files for: (1) Merging multiple PDFs, (2) Splitting pages, 
  (3) Extracting text, (4) Rotating pages, (5) Filling forms.
metadata:
 {
 "openclaw":
 {
   "emoji": "📄",
   "requires": { "bins": ["pdftk"], "env": ["PDF_API_KEY"] },
   "primaryEnv": "PDF_API_KEY",
   "install": [
     {
       "id": "brew",
       "kind": "brew",
       "formula": "pdftk",
       "bins": ["pdftk"],
       "label": "Install PDFTK (brew)"
     }
   ]
 }
 }
```

### Frontmatter Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | ✅ | Skill identifier (lowercase, hyphens) |
| `description` | ✅ | Primary trigger mechanism. Include what + when |
| `homepage` | Optional | URL shown in Skills UI |
| `user-invocable` | Optional | Default: `true`. Exposes as slash command |
| `metadata` | Optional | JSON object with gating/install info |

### Description Best Practices

The description is **loaded in every session**. It must answer: "When should I use this skill?"

```yaml
# ❌ Bad - too vague
description: PDF tools

# ✅ Good - specific and actionable
description: |
  Create, edit, and manipulate PDF documents. Use when:
  (1) Merging or splitting PDFs
  (2) Extracting text from PDFs
  (3) Rotating or reordering pages
  (4) Filling PDF forms
```

---

## Skill Body Best Practices

### 1. Be Concise

```markdown
# ❌ Bad - verbose
This skill allows you to create and edit PDF documents. PDF stands for 
Portable Document Format and is widely used for...

# ✅ Good - direct
## Quick Start

Merge PDFs:
```bash
pdftk file1.pdf file2.pdf cat output merged.pdf
```

Extract text:
```bash
pdftotext input.pdf output.txt
```
```

### 2. Prefer Examples Over Explanations

```markdown
# ❌ Bad
The API accepts a JSON payload with the following fields...

# ✅ Good
```json
{
  "name": "My Project",
  "type": "webapp",
  "config": { "port": 3000 }
}
```
```

### 3. Set Appropriate Degrees of Freedom

| Freedom Level | When to Use | Example |
|---------------|-------------|---------|
| **High** (text instructions) | Multiple valid approaches | "Choose appropriate caching strategy" |
| **Medium** (pseudocode) | Preferred pattern exists | "Use the retry pattern from examples" |
| **Low** (specific scripts) | Fragile operations | "Run `scripts/deploy.sh` exactly" |

---

## Bundled Resources

### scripts/

Executable code for deterministic or repetitive tasks.

```markdown
# In SKILL.md
## Scripts

- `scripts/rotate_pdf.py` - Rotate PDF pages
- `scripts/merge_pdfs.py` - Merge multiple PDFs

Run with: `python3 {baseDir}/scripts/<script>.py`
```

### references/

Documentation loaded as needed.

```markdown
# In SKILL.md
## Advanced Topics

- **Form filling**: See [references/forms.md](references/forms.md)
- **API reference**: See [references/api.md](references/api.md)
```

### assets/

Files used in output (templates, images, fonts).

```markdown
# In SKILL.md
## Templates

Copy the starter template:
```bash
cp -r {baseDir}/assets/template ./my-project
```
```

---

## Progressive Disclosure Pattern

Skills use three loading levels to manage context:

1. **Metadata** (~100 words) — Always in context
2. **SKILL.md body** (<5k words) — When skill triggers
3. **Bundled resources** — As needed

### Pattern: Split by Domain

```
bigquery-skill/
├── SKILL.md (navigation only)
└── references/
    ├── finance.md (revenue metrics)
    ├── sales.md (pipeline data)
    └── product.md (usage analytics)
```

### Pattern: Split by Framework

```
cloud-deploy/
├── SKILL.md (provider selection)
└── references/
    ├── aws.md (AWS patterns)
    ├── gcp.md (GCP patterns)
    └── azure.md (Azure patterns)
```

---

## Gating & Requirements

### Binary Requirements

```yaml
metadata:
 {
 "openclaw": {
   "requires": { "bins": ["gh", "jq"] }
 }
 }
```

### Environment Variables

```yaml
metadata:
 {
 "openclaw": {
   "requires": { "env": ["OPENAI_API_KEY"] },
   "primaryEnv": "OPENAI_API_KEY"
 }
 }
```

### Config Requirements

```yaml
metadata:
 {
 "openclaw": {
   "requires": { "config": ["browser.enabled"] }
 }
 }
```

### Platform-Specific

```yaml
metadata:
 {
 "openclaw": {
   "os": ["darwin", "linux"]
 }
 }
```

### Any Binary (OR logic)

```yaml
metadata:
 {
 "openclaw": {
   "requires": { "anyBins": ["uv", "pip"] }
 }
 }
```

---

## Publishing to ClawHub

### Prerequisites

1. Skill must have valid `SKILL.md` with frontmatter
2. Test locally first
3. Have a ClawHub account

### Publishing

```bash
# Using ClawHub CLI
clawhub publish ./my-skill

# Or sync all skills
clawhub sync --all
```

### After Publishing

Your skill appears on https://clawhub.com and can be installed:

```bash
clawhub install my-skill
```

---

## Templates

### Basic CLI Skill

```yaml
---
name: cli-tool
description: Use CLI-Tool for X operations. Use when: (1) Doing X, (2) Configuring Y.
---

# CLI-Tool Skill

## Quick Start

```bash
cli-tool command --option value
```

## Common Operations

### Operation 1

```bash
cli-tool op1 --flag
```

### Operation 2

```bash
cli-tool op2 --config config.json
```
```

### API Integration Skill

```yaml
---
name: api-client
description: |
  Interact with ExampleAPI for data retrieval and updates. Use when:
  (1) Fetching data from API
  (2) Creating/updating records
  (3) Managing API configuration
metadata:
 {
 "openclaw": {
   "requires": { "env": ["EXAMPLE_API_KEY"] },
   "primaryEnv": "EXAMPLE_API_KEY"
 }
 }
---

# ExampleAPI Client

## Authentication

API key is automatically loaded from `EXAMPLE_API_KEY` env var.

## Endpoints

### GET /items

```bash
curl -H "Authorization: Bearer $EXAMPLE_API_KEY" \
  https://api.example.com/items
```

### POST /items

```bash
curl -X POST \
  -H "Authorization: Bearer $EXAMPLE_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"name": "New Item"}' \
  https://api.example.com/items
```

## Error Handling

See [references/errors.md](references/errors.md) for error codes.
```

### Script-Based Skill

```yaml
---
name: pdf-processor
description: |
  PDF processing operations including merge, split, rotate, and extract.
  Use when: (1) Merging PDFs, (2) Splitting documents, (3) Extracting pages.
metadata:
 {
 "openclaw": {
   "requires": { "bins": ["python3"] }
 }
 }
---

# PDF Processor

## Available Scripts

| Script | Purpose |
|--------|---------|
| `merge.py` | Combine multiple PDFs |
| `split.py` | Split PDF by pages |
| `rotate.py` | Rotate PDF pages |

## Usage

```bash
# Merge PDFs
python3 {baseDir}/scripts/merge.py file1.pdf file2.pdf -o merged.pdf

# Split PDF
python3 {baseDir}/scripts/split.py input.pdf --pages 1-5

# Rotate PDF
python3 {baseDir}/scripts/rotate.py input.pdf --angle 90
```
```

---

## Examples

### Well-Structured Skills from ClawHub

1. **skill-creator** - Guide for creating skills
2. **github** - GitHub CLI integration
3. **tmux** - Terminal session management
4. **coolify** - Coolify PaaS API integration

### Common Patterns

#### Pattern 1: Workflow Guide

```markdown
# Deployment Workflow

1. **Prepare**: Run tests with `npm test`
2. **Build**: Run `npm run build`
3. **Deploy**: See [references/deploy.md](references/deploy.md)
```

#### Pattern 2: Configuration Reference

```markdown
# Configuration

Required env vars:
- `API_KEY` - Authentication token
- `API_URL` - Base URL (optional, defaults to https://api.example.com)

See [references/config.md](references/config.md) for all options.
```

#### Pattern 3: Error Recovery

```markdown
# Common Errors

## Rate Limited

```bash
# Wait and retry
sleep 60 && retry-command
```

## Auth Failed

Check `API_KEY` is set correctly:
```bash
echo $API_KEY | head -c 8
```
```

---

## Checklist Before Publishing

- [ ] `name` follows naming conventions (lowercase, hyphens)
- [ ] `description` includes both what and when to use
- [ ] Body is concise (<500 lines recommended)
- [ ] Scripts are tested and working
- [ ] References are organized and referenced from SKILL.md
- [ ] No extraneous files (README.md, CHANGELOG.md, etc.)
- [ ] Metadata gates are appropriate for requirements

---

## Resources

- **OpenClaw Docs**: https://docs.openclaw.ai/tools/skills
- **ClawHub**: https://clawhub.com
- **AgentSkills Spec**: https://agentskills.io
- **Awesome OpenClaw Skills**: https://github.com/VoltAgent/awesome-openclaw-skills
