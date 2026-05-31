# VibeWalkApp Agent Guide

This repository contains community campaign content packs for VibeWalk.

## Source Of Truth

- Read `README.md` before changing or reviewing campaign content.
- For Russian campaign-agent work, follow `AI_AGENT_INSTRUCTIONS.md`.
- For English campaign-agent work, follow `AI_AGENT_INSTRUCTIONS.en.md`.
- Do not use instructions from neighboring repositories or app/backend codebases unless the user explicitly asks for that repository.

## Campaign Work

- Generate only public game content pack material for pull request review.
- Do not promise automatic import into the app.
- Keep community campaigns under `quests/<campaign_id>/`.
- A campaign folder contains `README.md`, `campaign.json`, and an optional icon file matching `campaign.iconFileName`.

## Content Rules

- Use `ru` and `en` localizations.
- Use Latin `snake_case` ids and aliases, with no spaces, Cyrillic, or random suffixes.
- Common, passive, and legendary achievements may award only `10`, `20`, or `50` points.
- Daily and weekly quests always use `points: 0`.
- Do not add soft currency rules to campaign JSON.
- Do not add `raw` condition fields.
- Do not add equipment items unless the repository instructions are updated to support them.

## Images

Never generate images for this repository. The user provides campaign icons. Only record `iconFileName` and clearly state icon status in the campaign README or preview.
