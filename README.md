# Agent Rules

Guides, templates, and best practices for creating OpenClaw skills.

---

## What's Inside

### 📖 SKILL-CREATOR-GUIDE.md

Comprehensive guide covering:
- Skill structure and anatomy
- YAML frontmatter best practices
- Progressive disclosure pattern
- Gating and requirements
- Publishing to ClawHub

### 📁 templates/

Ready-to-use templates for common skill types:

| Template | Use Case |
|----------|----------|
| `CLI-SKILL-TEMPLATE.md` | CLI tool integrations |
| `API-SKILL-TEMPLATE.md` | REST API clients |
| `SCRIPT-SKILL-TEMPLATE.md` | Script-based skills |

### 📁 examples/

Well-structured example skills:

| Example | Description |
|---------|-------------|
| `deployment-skill/` | Multi-step deployment workflow |
| `database-skill/` | Database query patterns |

---

## Quick Start

1. **Copy a template**:
   ```bash
   cp templates/API-SKILL-TEMPLATE.md my-skill/SKILL.md
   ```

2. **Fill in the placeholders** (search for `{{PLACEHOLDER}}`)

3. **Test locally** by placing in `~/.openclaw/skills/my-skill/`

4. **Publish to ClawHub**:
   ```bash
   clawhub publish ./my-skill
   ```

---

## Key Principles

### 1. Context Window is Sacred

The description is loaded in EVERY session. Be concise.

```yaml
# ❌ Bad
description: PDF tools

# ✅ Good
description: Create, edit PDFs. Use when: (1) Merging PDFs, (2) Extracting text
```

### 2. Progressive Disclosure

Keep SKILL.md under 500 lines. Move details to `references/`.

```markdown
# In SKILL.md
## Advanced Topics

See [references/advanced.md](references/advanced.md)
```

### 3. Examples > Explanations

```markdown
# ❌ Bad
The API accepts JSON with the following structure...

# ✅ Good
```json
{ "name": "My Project", "type": "webapp" }
```
```

---

## Resources

- **OpenClaw Docs**: https://docs.openclaw.ai/tools/skills
- **ClawHub**: https://clawhub.com
- **AgentSkills Spec**: https://agentskills.io
- **Awesome OpenClaw Skills**: https://github.com/VoltAgent/awesome-openclaw-skills

---

## Contributing

PRs welcome! Add new templates or improve existing guides.

---

MIT License
