# Agent Skills Repository

This repository contains reusable Agent Skills that extend AI agent capabilities with specialized knowledge and workflows. Skills follow the [Agent Skills format](http://agentskills.io/) specification, making them compatible with skills-compatible agent products.

## What are Agent Skills?

Agent Skills are folders containing instructions, scripts, and resources that agents can discover and use to perform tasks more accurately and efficiently. They package procedural knowledge and domain expertise into portable, version-controlled packages.

**Key benefits:**
- **Domain expertise**: Package specialized knowledge into reusable instructions
- **New capabilities**: Give agents new abilities (e.g., creating presentations, analyzing datasets)
- **Repeatable workflows**: Turn multi-step tasks into consistent and auditable workflows
- **Interoperability**: Reuse the same skill across different skills-compatible agent products

## Repository Structure

This repository uses a **flat structure** - all skills are stored at the root level:

```
skills/
├── upgrade-php/
│   └── SKILL.md
├── woodworking/
│   ├── SKILL.md
│   └── references/
├── data-analysis/
│   ├── SKILL.md
│   └── scripts/
└── ...
```

### Naming Conventions

Skill names should feel **natural and descriptive**, like describing a real-world skill:

✅ **Good examples:**
- `upgrade-php` - Clear, action-oriented
- `woodworking` - Domain-specific expertise
- `code-review` - Describes the activity
- `data-analysis` - Clear purpose
- `create-presentations` - Action-focused

❌ **Avoid:**
- `helper` - Too vague
- `utils` - Not descriptive
- `tool` - Generic
- `skill-1` - Not meaningful

**Format requirements:**
- Lowercase letters, numbers, and hyphens only (`a-z`, `0-9`, `-`)
- 1-64 characters
- Must not start or end with a hyphen
- Must not contain consecutive hyphens (`--`)
- Must match the directory name

## Creating a New Skill

### 1. Directory Structure

Create a directory with your skill name and include at minimum a `SKILL.md` file:

```
your-skill-name/
├── SKILL.md              # Required - main instructions
├── scripts/              # Optional - executable code
│   └── helper.py
├── references/           # Optional - detailed documentation
│   └── REFERENCE.md
└── assets/              # Optional - templates, resources
    └── template.md
```

### 2. SKILL.md Format

Every skill must have a `SKILL.md` file with YAML frontmatter and Markdown content:

```markdown
---
name: your-skill-name
description: A clear description of what this skill does and when to use it. Include specific keywords that help agents identify relevant tasks.
license: MIT
---

# Your Skill Name

## When to Use

Describe when this skill should be activated...

## Instructions

Step-by-step guidance for the agent...

## Examples

Concrete examples of using this skill...
```

### 3. Required Frontmatter Fields

#### `name` (required)
- Max 64 characters
- Lowercase letters, numbers, and hyphens only
- Must match the directory name
- Must not start/end with hyphen or contain consecutive hyphens

#### `description` (required)
- Max 1024 characters
- **Critical for skill discovery** - agents use this to decide when to apply the skill
- Should describe both **WHAT** the skill does and **WHEN** to use it
- Include specific keywords and trigger terms
- Write in **third person** (the description is injected into the system prompt)

**Good description examples:**
```yaml
description: Manages incremental PHP version upgrades (e.g. 8.0->8.1->8.2) with phased development, library compatibility checks, infrastructure updates. Use when upgrading PHP versions, checking library compatibility against a specific PHP version, or planning infrastructure upgrades.

description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.

description: Review code for quality, security, and best practices following team standards. Use when reviewing pull requests, code changes, or when the user asks for a code review.
```

**Poor description examples:**
```yaml
description: Helps with PDFs.  # Too vague, no trigger terms

description: I can help you upgrade PHP.  # First person, not specific

description: Utility functions.  # Not descriptive
```

### 4. Optional Frontmatter Fields

#### `license`
- License name or reference to a bundled license file
- Example: `license: MIT` or `license: Apache-2.0`

#### `compatibility`
- Max 500 characters
- Indicates environment requirements (intended product, system packages, network access, etc.)
- Only include if your skill has specific requirements
- Example: `compatibility: Requires git, docker, jq, and access to the internet`

#### `metadata`
- Arbitrary key-value mapping for additional metadata
- Example:
  ```yaml
  metadata:
    author: example-org
    version: "1.0"
  ```

### 5. Writing Effective Skills

#### Core Principles

1. **Concise is Key**
   - The agent is already very smart - only add context it doesn't have
   - Challenge each piece of information: "Does the agent really need this?"
   - Keep `SKILL.md` under 500 lines

2. **Progressive Disclosure**
   - Put essential information in `SKILL.md`
   - Move detailed reference material to separate files in `references/`
   - Keep file references **one level deep** from `SKILL.md`

3. **Specific and Actionable**
   - Provide concrete examples, not abstract concepts
   - Include step-by-step workflows when appropriate
   - Use templates and checklists for repeatable tasks

4. **Consistent Terminology**
   - Choose one term and use it throughout
   - Avoid mixing synonyms (e.g., don't mix "URL", "route", "path")

#### Common Patterns

**Template Pattern:**
```markdown
## Report Structure

Use this template:

```markdown
# [Analysis Title]

## Executive Summary
[One-paragraph overview]

## Key Findings
- Finding 1
- Finding 2
```
```

**Examples Pattern:**
```markdown
## Commit Message Format

**Example 1:**
Input: Added user authentication
Output:
```
feat(auth): implement JWT-based authentication
```

**Example 2:**
Input: Fixed date bug
Output:
```
fix(reports): correct date formatting
```
```

**Workflow Pattern:**
```markdown
## Migration Workflow

1. **Phase 1: Assessment**
   - Analyze current state
   - Identify dependencies
   
2. **Phase 2: Planning**
   - Create migration plan
   - Document risks
```

### 6. Optional Directories

#### `scripts/`
Contains executable code that agents can run:
- Should be self-contained or clearly document dependencies
- Include helpful error messages
- Handle edge cases gracefully
- Common languages: Python, Bash, JavaScript

#### `references/`
Contains additional documentation loaded on demand:
- `REFERENCE.md` - Detailed technical reference
- Domain-specific files (`finance.md`, `legal.md`, etc.)
- Keep files focused and small

#### `assets/`
Contains static resources:
- Templates (document templates, configuration templates)
- Images (diagrams, examples)
- Data files (lookup tables, schemas)

## Contributing Skills

### Before Contributing

1. **Check existing skills** - Avoid duplicates or very similar skills
2. **Validate your skill** - Use the [skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref) reference library to validate:
   ```bash
   skills-ref validate ./your-skill-name
   ```
3. **Test your skill** - Ensure it works with skills-compatible agents
4. **Follow the format** - Adhere to the Agent Skills specification

### Contribution Checklist

- [ ] Skill follows the Agent Skills format specification
- [ ] `name` field matches directory name and follows naming conventions
- [ ] `description` is specific, includes trigger terms, and written in third person
- [ ] `SKILL.md` is under 500 lines
- [ ] File references are one level deep
- [ ] Examples are concrete and actionable
- [ ] Consistent terminology throughout
- [ ] No time-sensitive information
- [ ] Scripts (if any) are documented and handle errors gracefully
- [ ] License is specified if applicable

### Submission Process

1. Create your skill directory with `SKILL.md`
2. Add any optional directories (`scripts/`, `references/`, `assets/`) as needed
3. Validate your skill using the skills-ref library
4. Submit a pull request with:
   - Clear description of what the skill does
   - Why it's useful
   - Any dependencies or requirements

## Best Practices

### Do's ✅

- **Be specific** in descriptions and instructions
- **Include trigger terms** that help agents discover your skill
- **Use progressive disclosure** - keep main file concise, details in references
- **Provide examples** - concrete examples are more valuable than abstract descriptions
- **Test thoroughly** - ensure your skill works as intended
- **Document dependencies** - if scripts require specific packages or tools

### Don'ts ❌

- **Don't be verbose** - the agent is smart, only add what's needed
- **Don't use first person** in descriptions ("I can help..." → "Helps with...")
- **Don't include time-sensitive info** - use versioning or "current method" sections instead
- **Don't create deeply nested references** - keep it one level deep
- **Don't use vague names** - `helper`, `utils`, `tool` are not helpful
- **Don't mix terminology** - pick one term and stick with it
- **Don't use Windows-style paths** - use forward slashes (`scripts/helper.py`)

## Resources

- [Agent Skills Specification](http://agentskills.io/specification) - Complete format specification
- [What are Skills?](http://agentskills.io/what-are-skills) - Introduction to Agent Skills
- [Example Skills](https://github.com/anthropics/skills) - Browse example skills on GitHub
- [Skills Reference Library](https://github.com/agentskills/agentskills/tree/main/skills-ref) - Validate skills and generate prompt XML

## License

Skills in this repository are licensed under the MIT License unless otherwise specified in the skill's frontmatter.

---

**Questions or suggestions?** Open an issue or submit a pull request!
