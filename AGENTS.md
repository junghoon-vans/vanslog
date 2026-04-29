# AGENTS.md

This document applies to this repository and all subdirectories.

## Project Overview
- This repository hosts a **Hugo**-based technical blog.
- The active theme is `papermod`.
- The default content language is Korean (`ko`).
- Core site settings are split across `config/_default/*.yaml`.

## Key Directory Structure
- `content/`: Blog post source files (Markdown), organized by topic/category paths.
- `config/_default/`: Site configuration (main config, params, menu, language settings).
- `layouts/`: Custom templates, partials, and markup renderers.
- `assets/`: CSS/JS assets processed by Hugo pipelines.
- `static/`: Static files served as-is (images, favicons, verification files).
- `archetypes/`: Default templates used when creating new posts.

## Working Principles
1. **Keep changes minimal**: avoid unrelated formatting or broad refactors.
2. **Preserve content consistency**:
   - Follow existing path conventions (e.g., `content/posts/<domain>/<topic>.md`).
   - Match existing frontmatter keys and style used in nearby posts.
3. **Be careful with global settings**:
   - Changes in `config/_default/config.yaml` can affect the whole site.
4. **Protect static path stability**:
   - If you rename/move static assets, update all references in content and layouts.

## Recommended Validation Flow
- For content-only changes:
  - Verify Markdown heading hierarchy and code block rendering.
  - Check image/link paths for typos.
- For config/layout/assets changes:
  - Run a local Hugo build or preview and inspect for visual/render regressions.

## Local Verification Commands
- Dev preview: `hugo server -D`
- Production build: `hugo`

> Note: Hugo may be unavailable in some execution environments. If a command fails due to missing tooling, report it explicitly as an environment limitation.

## Commit Guidance
- Follow **Conventional Commits** format: `type(scope): summary`.
- Common types: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`.
- Use commit messages that clearly describe intent and scope.
- Keep one logical change per commit whenever possible.
- Even for docs-only edits, explain *what* changed and *why* in the commit message.
