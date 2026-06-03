# User Campaigns

This folder is where contributors add new VibeWalk campaign proposals.

Each campaign must live in its own `snake_case` folder:

```text
quests/
  campaign_id/
    README.md
    campaign.json
    campaign_icon.png
    optional_item_image.png
```

Required files:

- `README.md` - human-readable campaign description, author, quest and achievement list, conditions, points, titles, equipment sets/items, and asset status.
- `campaign.json` - one `packType: "campaign"` content pack for game export. It can include campaign content, player titles, `equipmentCollections`, and `equipmentItems`.
- campaign icon - PNG or WebP file matching `campaign.iconFileName` in `campaign.json`, if the icon is ready.
- equipment item images - PNG or WebP files matching `equipmentItems[].imageFileName`, if the pack declares items.

Use the root `README.md`, `README.en.md`, and `AI_AGENT_INSTRUCTIONS*.md` files for the full authoring guide.
