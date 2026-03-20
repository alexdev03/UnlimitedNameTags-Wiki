# Wiki style guide

Short rules for editing **UnlimitedNameTags** documentation.

1. **Language:** English for all user-facing wiki pages.
2. **Title:** One top-level `# Title` per page; use `##` / `###` below. Avoid skipping levels (e.g. do not jump from `##` to `####` without `###`).
3. **Separators:** Use `---` on its own line between major sections; avoid decorative `***`.
4. **Commands and permissions:** Use backticks for commands (`/unt reload`) and permissions (`unt.reload`). Prefer tables for permission summaries.
5. **YAML keys:** Match the plugin and ConfigLib defaults (**camelCase** for `settings.yml` / `advanced.yml` unless documented otherwise).
6. **Accuracy:** When describing behaviour, cross-check the **UnlimitedNametags** Java source; if something is disabled in code (e.g. a hook), say so.
