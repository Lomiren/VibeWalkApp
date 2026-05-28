# User Campaigns

This folder is where contributors add new VibeWalk campaign proposals.

Each campaign must live in its own `snake_case` folder:

```text
quests/
  campaign_id/
    README.md
    campaign.json
    campaign_icon.png
```

Required files:

- `README.md` - human-readable campaign description, author, quest and achievement list, conditions, points, titles, and icon status.
- `campaign.json` - one `packType: "campaign"` content pack for game export.
- campaign icon - PNG or WebP file matching `campaign.iconFileName` in `campaign.json`, if the icon is ready.

Use the root `README.md`, `README.en.md`, and `AI_AGENT_INSTRUCTIONS*.md` files for the full authoring guide.
