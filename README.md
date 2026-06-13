# Claude Skills — Reference Files

Reference files for Claude skills used by Liuda (Lead UX Designer, DataArt).

These files are fetched live by Claude during conversations — 
so edits here take effect immediately without reinstalling the skill.

## Structure

Each skill has its own folder with a `references/` subdirectory.
The skill logic (SKILL.md) lives inside the installed `.skill` file in Claude.
Only reference files that change frequently live here.

## Skills

| Skill | Description |
|-------|-------------|
| `figma-variables` | Figma Variables architecture, audit, dev handoff |

## How to update

1. Edit the relevant `.md` file directly in GitHub UI
2. Commit to `main`
3. Claude will fetch the updated version on next conversation

## Verification dates

Each fact in reference files is marked with a verification date and source.
When Claude finds newer information, it will suggest a specific edit to make here.
