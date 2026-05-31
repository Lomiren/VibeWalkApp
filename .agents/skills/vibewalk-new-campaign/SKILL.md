---
name: vibewalk-new-campaign
description: Start a fresh VibeWalk community campaign workflow in Lomiren/VibeWalkApp. Use when the user says "Начинаем новую кампанию", "Начинаем новую компанию", asks to start a new VibeWalk campaign, or invokes $vibewalk-new-campaign. Creates a clean campaign branch when possible, then interviews the user using the repository README.md and AI_AGENT_INSTRUCTIONS.md without generating images.
---

# VibeWalk New Campaign

## Start Workflow

1. Confirm the current repository is `Lomiren/VibeWalkApp` or contains both `README.md` and `AI_AGENT_INSTRUCTIONS.md`.
2. Read `README.md` and the appropriate agent instruction file:
   - Russian chat: `AI_AGENT_INSTRUCTIONS.md`
   - English chat: `AI_AGENT_INSTRUCTIONS.en.md`
3. Run `git status --short --untracked-files=all`.
4. If the worktree has any uncommitted or untracked changes, stop before branching. Report the dirty paths and ask the user to commit, stash, or otherwise save them.
5. If the worktree is clean, create a branch:
   - If the initial user message includes a clear campaign title or id, derive a Latin `snake_case` slug and run `git switch -c campaign/<slug>`.
   - Otherwise run `git switch -c campaign/new-campaign-YYYYMMDD-HHMM`, using local time.
6. Start the campaign interview from zero.

## Interview Rules

Follow `AI_AGENT_INSTRUCTIONS.md` exactly for Russian sessions and `AI_AGENT_INSTRUCTIONS.en.md` for English sessions. Ask first; do not output final JSON until the user confirms key choices or asks to use your suggestions.

Collect, in order:

1. Campaign name.
2. Author name and one short author description for `ru` and `en`.
3. Campaign group: `global_cities`, `global_games`, `global_books`, or a new group.
4. Writing style.
5. Required common achievements.
6. Optional common achievements.
7. Daily quests.
8. Weekly quests.
9. Passive hidden achievements, if needed.
10. Campaign legendary achievement.
11. Global legendary achievements, if needed.
12. Player titles and unlock rules.
13. Campaign icon filename, or a clear note that no icon is ready.

## Content Rules

- Generate only VibeWalk community campaign content.
- Do not promise automatic import into the app.
- Keep output compatible with `README.md` and `AI_AGENT_INSTRUCTIONS.md`.
- Final campaign output belongs in `quests/<campaign_id>/README.md` and `quests/<campaign_id>/campaign.json`.
- Use one `packType: "campaign"` JSON file.
- Include `ru` and `en` localizations.
- Include campaign author fields: `authorName` and `authorDescription` in both locales.
- Use Latin `snake_case` ids and aliases.
- Use camelCase condition keys.
- Do not add a `raw` condition field.
- Do not add soft currency rules.
- Do not add equipment items unless this repository's instructions are updated to support them.
- Common, passive, and legendary achievements may award only `10`, `20`, or `50` points.
- Daily and weekly quests always use `points: 0`.

## Images

Never generate images. Do not call image generation tools or create placeholder image files. The user supplies campaign icons. Only record `campaign.iconFileName` and state whether the icon file is ready or missing.
