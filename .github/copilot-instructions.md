# Copilot Instructions

## Repository Purpose

This is a **meta-repository** of reusable AI instructions, skills, agents, plugins, and collections for GitHub Copilot and AI coding tools. It is NOT a typical application codebase — there is no runtime code to build or test. The primary artifact types are Markdown-based instruction files and YAML configuration files.

## Repository Structure

| Directory | Purpose |
|---|---|
| `instructions/` | `.instructions.md` files with coding standards, applied via `applyTo` front-matter glob |
| `skills/` | Self-contained `SKILL.md` packages with bundled resources (scripts, templates, references) |
| `agents/` | `.agent.md` files defining custom AI personas |
| `plugins/` | `plugin.yaml` bundles grouping related skills for installation |
| `collections/` | YAML files grouping instructions/prompts for APM consumption |
| `website/` | Astro static documentation site |
| `scripts/` | Maintenance scripts (e.g., `validate-collection-links.sh`) |

## Content Authoring Rules

### Instructions (`instructions/*.instructions.md`)
- Must include YAML front-matter with `applyTo` (file glob) and `description` fields.
- Use imperative language: "Do", "Use", "Avoid".
- Structure: Title → Purpose → Rules/Guidelines → Best Practices → Examples → References (optional).
- See [instructions/meta-instructions.instructions.md](instructions/meta-instructions.instructions.md) for the authoritative template.

### Skills (`skills/<name>/SKILL.md`)
- Front-matter requires `name` (lowercase kebab-case, no underscores/spaces) and `description` (must start with "Use when").
- Include "When to Use" and "When NOT to Use" sections to prevent over-application.
- Prefer concise examples over verbose prose — the context window is shared with conversation history.
- See [skills/skill-creator/SKILL.md](skills/skill-creator/SKILL.md) for the creating/updating guide.
- See [skills/migrating-prompts-to-skills/SKILL.md](skills/migrating-prompts-to-skills/SKILL.md) when converting `.prompt.md` or `.instructions.md` to skill format.

### Agents (`agents/*.agent.md`)
- Front-matter: `name`, `description`, `tags`.
- Must define explicit scope limitations (e.g., the Architect agent restricts itself to `.md` files only).

### Collections (`collections/*.yml`)
- Reference paths as they appear in the consuming project (typically under `.github/instructions/`).
- Run `scripts/validate-collection-links.sh` to verify all referenced paths exist before committing.

### Plugins (`plugins/<name>/plugin.yaml`)
- Lists `skills` by name; skills must exist in `skills/` or the plugin's own `skills/` subfolder.

## Language & Commit Policy

- All content must be written in **English**.
- All commits must be **signed** and follow **Conventional Commits**: `<type>[scope]: <description>` (max 72 chars on first line).
  - Types: `feat`, `fix`, `docs`, `refactor`, `chore`, `ci`
  - Example: `feat(skills): add rg-code-search skill`

## Website Development

Commands run from the `website/` directory:

```sh
npm install       # Install dependencies
npm run dev       # Start dev server at localhost:4321
npm run build     # Build to ./dist/
```

## C# Code Generation (when producing output for consumers of this repo)

When generating C# code guided by instructions in this repo, consult and apply in order:

1. **DDD** — [instructions/domain-driven-design.instructions.md](instructions/domain-driven-design.instructions.md): no base class inheritance, factory methods (`Order.Create(...)`), strongly typed IDs, business-intent method names (never CRUD verbs like Get/Set/Update/Delete).
2. **Clean Architecture** — [skills/clean-architecture-dotnet/SKILL.md](skills/clean-architecture-dotnet/SKILL.md): four layers (Domain → Application → Infrastructure → API), CQRS without MediatR, handler discovery by convention.
3. **Testing** — [instructions/unit-and-integration-tests.instructions.md](instructions/unit-and-integration-tests.instructions.md): TDD-first; run `dotnet test` before committing (do not ask, just run it).
4. Always start your response by listing which instruction files guided the implementation.

Path tokens in rules: replace `[project]` and `[feature]` with actual names (e.g., `Ordering`, `PlaceOrder`).
