# Awesome Skills

A collection of OpenCode Agent Skills for Terraform Azure RM and related workflows.

## Overview

This repository contains reusable OpenCode Agent Skills that enhance OpenCode's capabilities for infrastructure-as-code workflows, particularly focused on Azure Resource Manager (AzureRM) and Terraform.

## Skills

### azurerm-acctest

Run terraform-provider-azurerm acceptance tests locally with mitmproxy traffic capture.

**Usage:**
```bash
/skill azurerm-acctest
```

This skill provides guidance on:
- Sourcing required environment variables
- Setting up mitmproxy for traffic capture
- Running targeted acceptance tests
- Preserving test output for debugging

## Installation

### For Global Access

Copy skills to your global OpenCode skills directory:

```bash
cp -r .opencode/skills/* ~/.config/opencode/skills/
```

### For Project-Local Access

Commit this repository to your project and OpenCode will discover skills automatically:

```bash
git add .opencode/
git commit -m "Add OpenCode skills"
```

## Publishing with `gh skills`

To publish these skills using the `gh skills` CLI:

```bash
gh skills publish
```

This repository is configured to be discoverable by GitHub's skills ecosystem.

## Structure

```
.
├── .opencode/
│   └── skills/
│       └── azurerm-acctest/
│           └── SKILL.md
├── opencode.json
└── README.md
```

## Skill Requirements

All skills in this repository comply with the [OpenCode Agent Skills Specification](https://opencode.ai/docs/skills/):

- ✅ Placed in `.opencode/skills/<name>/SKILL.md`
- ✅ Contain valid YAML frontmatter with `name` and `description`
- ✅ Names are lowercase alphanumeric with hyphens
- ✅ Descriptions are 1-1024 characters
- ✅ Match directory names exactly

## Contributing

To add a new skill:

1. Create a new directory: `.opencode/skills/<skill-name>/`
2. Create `SKILL.md` with required frontmatter:
   ```yaml
   ---
   name: skill-name
   description: Brief description (max 1024 chars)
   license: MIT
   compatibility: opencode
   ---
   ```
3. Add your skill content below the frontmatter
4. Ensure skill name matches the directory name

## License

These skills are licensed under the MIT License. See individual skill files for specific licensing information.

## Support

For issues or questions about OpenCode:
- GitHub Issues: https://github.com/anomalyco/opencode/issues
- Discord: https://opencode.ai/discord
- Documentation: https://opencode.ai/docs/skills/
