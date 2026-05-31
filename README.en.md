# User Quests for VibeWalk

[Русская версия](README.md)

VibeWalk turns ordinary walking into RPG progress. A player chooses an active campaign, and walking data such as steps, distance, active calories, and climbs gradually completes campaign goals. A campaign can be a city route, a book or game world, a personal challenge, or another theme. It can include main achievements, daily/weekly quests, passive hidden discoveries, a legendary completion, player titles, and equipment items.

This folder is for authors of user-submitted campaigns. It describes the public content pack format for review and pull requests. After a PR, a maintainer reviews the content and imports it into the app.

## What You Can Submit

- `campaign` - one campaign: city, book, game, route, challenge, or another theme.
- `commonAchievements` - one-time main campaign achievements that give points.
- `isRequiredForLegendary: true` - a required main achievement for campaign legendary completion.
- `isRequiredForLegendary: false` - an optional main achievement. It gives rewards but does not block legendary completion.
- `dailyQuests` - repeatable daily quests. They always use `points: 0`.
- `weeklyQuests` - repeatable weekly quests. They always use `points: 0`.
- `passiveHiddenAchievements` - hidden passive achievements unlocked by behavior or app events.
- `legendaryAchievement` - a rare achievement for major campaign completion.
- `globalLegendaryAchievements` - legendary achievements for overall player progress, not tied to one campaign.
- `playerTitles` - proposed character titles. An achievement can reference a title through `rewardPlayerTitle`.
- `equipmentItems` - equipment items awarded by achievements or quests through `rewardEquipmentItem`.

## Using the AI Agent

1. Open a new chat with an AI agent.
2. Attach or paste `AI_AGENT_INSTRUCTIONS.en.md` as the persistent instruction.
3. In the first message, give the campaign theme and ask the agent to interview you.
4. Answer the questions: title, author, group, writing style, required and optional goals, daily/weekly quests, passive/legendary ideas, player titles, and equipment items.
5. Review the preview tables: titles, conditions, points, required flags, reward titles, and reward items.
6. Ask the agent for the final JSON content pack and campaign `README.md` text. A campaign is always one JSON file: main achievements, daily/weekly quests, passive hidden achievements, legendary achievements, player titles, and equipment items are kept together.
7. Create a pull request with your campaign folder inside `quests/`.

Example first prompt:

```text
I want to create a VibeWalk campaign called "Night City". Interview me using the instructions and help me design required and optional achievements, daily/weekly quests, one legendary achievement, one title, and a few equipment rewards.
```

## Pull Request Structure

User-submitted campaigns live in `quests/`. Create a folder with the same `snake_case` name as `campaign.id`.

```text
quests/
  samarkand/
    README.md
    campaign.json
    samarkand_icon.png
    samarkand_silk_hat.png
```

Campaign folder contents:

- `campaign.json` - one `packType: "campaign"` JSON file. It contains main achievements, daily/weekly quests, passive hidden achievements, campaign legendary achievement, optional global legendary achievements, player titles, equipment items, and campaign author metadata.
- `README.md` - human-readable campaign description: what the campaign is, who wrote it, what quests and achievements exist, what each condition means, how many points are awarded, which titles and items are unlocked.
- Campaign icon - PNG or WebP file whose name matches `campaign.iconFileName`. `256x256` or larger is normal for import; `512x512` is not required.
- Equipment images - PNG or WebP files next to `campaign.json`. Each file name must match `equipmentItems[].imageFileName`.

The campaign `README.md` should include:

- campaign title;
- author and short author description;
- simple campaign idea;
- required main achievements with conditions and points;
- optional main achievements with conditions and points;
- daily and weekly quests with conditions;
- passive hidden achievements, if present;
- legendary achievement and its condition;
- global legendary achievements, if present;
- player titles and what unlocks them;
- equipment items, their slots, rarity, and which achievement/quest awards them;
- campaign icon and item image status.

## Content Rules

- All `id` values and aliases use English `snake_case`, with no spaces or random suffixes.
- All content must have `ru` and `en` localizations.
- Main, passive, and legendary achievements use `5` to `50` points inclusive. Values must be divisible by `5`.
- Daily and weekly quests always use `points: 0`.
- Soft currency is not described in JSON. The game calculates it outside the content pack.
- Conditions use camelCase keys.
- A user campaign is one `packType: "campaign"` JSON file. If the campaign has passive achievements, put them in `campaign.passiveHiddenAchievements`.
- Global legendary achievements go in top-level `globalLegendaryAchievements` in the same `campaign.json`.
- Campaign author is required: `authorName` and `authorDescription` in both `ru` and `en`.
- Equipment items go in top-level `equipmentItems`, not inside `campaign`.
- An achievement or daily/weekly quest can award an item with `rewardEquipmentItem`. The value must reference an `equipmentItems[].id` from this pack or an existing active item.
- Do not embed images in JSON. Use `iconFileName` and `equipmentItems[].imageFileName`, then attach files separately.
- PNG/WebP `256x256` is a normal size for campaign icons and item images. Max file size is 2 MB. `imageFileName` must be a file name only, with no directories.

Base campaign groups:

| group | Meaning |
| --- | --- |
| `global_cities` | Cities |
| `global_games` | Games |
| `global_books` | Books |

If you need a new group, mention it in the PR idea. Before import, an admin must create and publish that global group separately with `kind = campaign_group`; the importer does not create new groups automatically.

## Equipment Items

Equipment items are server content. They are not added to the player inventory immediately after import. A player receives an item only when they unlock an achievement or quest with `rewardEquipmentItem`.

Supported `slot` values:

| slot | Meaning |
| --- | --- |
| `hat` | Hat |
| `shirt` | Shirt |
| `pants` | Pants |
| `boots` | Boots |
| `gadget` | Gadget |
| `pendant` | Pendant |
| `jacket` | Jacket/cloak |
| `pet` | Pet |

Supported `rarity` values:

| rarity | Meaning |
| --- | --- |
| `common` | Common gray item |
| `rare` | Rare green item |
| `legendary` | Legendary orange item |
| `epic` | Epic purple item |

Item fields:

| Field | Required | Description |
| --- | --- | --- |
| `id` | Yes | `snake_case` item alias. |
| `slot` | Yes | One supported slot. |
| `rarity` | Yes | One supported rarity. |
| `imageFileName` | Yes | PNG/WebP file next to `campaign.json`, with no directories in the name. |
| `softPrice` | No | Coin price if the item should be available in the store. Usually omitted for reward-only items. |
| `isStoreVisible` | No | `true` if the item should appear in the store. Usually absent or `false` for reward-only items. |
| `localizations.ru/en.name` | Yes | Item name. |
| `localizations.ru/en.description` | No | Item description. |

Example reward:

```json
{
  "id": "samarkand_first_steps",
  "points": 10,
  "rewardEquipmentItem": "samarkand_silk_hat",
  "conditions": {
    "totalStepsInCategory": 1000
  }
}
```

## Supported Conditions

Conditions live in `conditions` and use camelCase. Multiple conditions in one achievement are AND: all must be completed. For new campaigns, start with simple step, distance, calorie, daily, and weekly goals. Advanced conditions are supported, but call them out in the PR so a maintainer can review the idea.

Do not add a `raw` field. It is not part of the user content pack.

| Condition | Meaning | Usually used for | Example |
| --- | --- | --- | --- |
| `totalStepsInCategory` | Accumulate N steps inside this campaign. | Main achievements | `{"totalStepsInCategory": 100000}` |
| `totalDistanceMetersInCategory` | Accumulate N meters inside a campaign. Add `categoryId` if it targets a specific campaign. | Main, passive, legendary | `{"totalDistanceMetersInCategory": 42500, "categoryId": "samarkand"}` |
| `totalDistanceMeters` | Accumulate N meters lifetime across all campaigns. | Global legendary | `{"totalDistanceMeters": 42500}` |
| `totalActiveCaloriesInCategory` | Accumulate N active calories inside this campaign. | Main achievements | `{"totalActiveCaloriesInCategory": 10000}` |
| `totalStairsEquivalentInCategory` | Accumulate N floors or stair-equivalent units inside this campaign. | Main achievements | `{"totalStairsEquivalentInCategory": 842}` |
| `stepsInDay` | Reach N steps during the current local day in the active campaign. | Daily quests | `{"stepsInDay": 7000}` |
| `activeCaloriesInDay` | Reach N active calories during the current day. Often combined with `stepsInDay`. | Daily quests | `{"stepsInDay": 8000, "activeCaloriesInDay": 300}` |
| `stepsInWeek` | Reach N steps during the current week in the active campaign. | Weekly quests | `{"stepsInWeek": 50000}` |
| `activeCaloriesInWeek` | Reach N active calories during the current week. | Weekly quests | `{"stepsInWeek": 60000, "activeCaloriesInWeek": 1500}` |
| `daysInWeekWithAtLeastSteps` | During a week, have at least `steps` steps on `days` different days. | Weekly quests | `{"daysInWeekWithAtLeastSteps": {"days": 5, "steps": 5000}}` |
| `event` | User or app event. Supported values are listed below. | Passive achievements | `{"event": "firstCategorySelected"}` |
| `times` | How many times an event must happen. Currently useful for `categoryChanged`. | Passive achievements | `{"event": "categoryChanged", "times": 1}` |
| `categoryId` | Bind an event or distance to a campaign by `campaign.id`. | Passive, legendary | `{"event": "categorySelected", "categoryId": "samarkand"}` |
| `stepsInDayLessThan` | A completed day had fewer than N steps. | Passive achievements | `{"stepsInDayLessThan": 2000}` |
| `stepsInDayBetween` | A completed day's steps were within `min`-`max`. | Passive achievements | `{"stepsInDayBetween": {"min": 1000, "max": 3000}}` |
| `activeCaloriesInDayLessThan` | A completed day had fewer than N active calories. | Passive achievements | `{"activeCaloriesInDayLessThan": 100}` |
| `stepsInDayAtLeast` | A day had at least N steps. | Passive achievements | `{"stepsInDayAtLeast": 5000}` |
| `dayOfWeek` | Restrict a condition to `monday` ... `sunday`. | Passive achievements | `{"dayOfWeek": "monday", "stepsInDayLessThan": 3000}` |
| `softCurrencyEarnedAtLeast` | Earn at least N soft currency lifetime. Spending does not reduce this counter. | Passive achievements | `{"softCurrencyEarnedAtLeast": 50}` |
| `campaignUnlocksAtLeast` | Unlock at least N paid campaigns. Free campaigns do not count. | Passive achievements | `{"campaignUnlocksAtLeast": 3}` |
| `timeWindowSteps` | In a short recent period, find days or weekends where steps inside a time window match a range. | Passive achievements | `{"timeWindowSteps": {"days": ["monday"], "startTime": "06:00", "endTime": "12:00", "minSteps": 3000, "occurrences": 1}}` |
| `weeklyStepsRange` | In a short recent period, find weeks where total steps match a range. | Passive achievements | `{"weeklyStepsRange": {"minSteps": 30000, "occurrences": 1}}` |
| `allCommonAchievementsCompletedInCategory` | Complete all required main achievements in a campaign. Optional achievements do not block it. | Legendary achievements | `{"allCommonAchievementsCompletedInCategory": "samarkand"}` |
| `allLegendaryAchievementsCompleted` | Complete a list of other legendary achievements. | Legendary achievements | `{"allLegendaryAchievementsCompleted": ["legendary_samarkand_completed"]}` |
| `metric` + `target` | Legacy/global lifetime condition. Supported metrics: `steps`, `distance_meters`, `flights`, `active_energy_kcal`. Use only for global legendary and call it out in the PR. | Global legendary | `{"metric": "steps", "target": 1000000}` |

Supported `event` values: `firstAppLaunch`, `healthAccessGranted`, `firstStepsDataReceived`, `firstCategorySelected`, `categoryChanged`, `categorySelected`.

`timeWindowSteps` supports `days`, `startTime`, `endTime`, `minSteps`, `maxSteps`, `occurrences`, `consecutive`, `period`. `period` can be `day` or `weekend`. At least one of `minSteps` or `maxSteps` is required.

`weeklyStepsRange` supports `minSteps`, `maxSteps`, `occurrences`, `consecutive`. At least one of `minSteps` or `maxSteps` is required.

## Campaign Questionnaire

Before final JSON, collect:

1. Campaign title.
2. Author and one-sentence author description in `ru` and `en`.
3. Campaign group: `global_cities`, `global_games`, `global_books`, or a new group.
4. Writing style.
5. Required main achievements.
6. Optional main achievements.
7. Daily quests.
8. Weekly quests.
9. Passive hidden achievements, if needed.
10. Campaign legendary achievement.
11. Player titles and what unlocks them.
12. Equipment items: slot, rarity, image file, and reward source.
13. Campaign icon and item image file names.

## Campaign Content Pack

```json
{
  "contentPackVersion": "1.0",
  "packType": "campaign",
  "campaign": {
    "id": "samarkand",
    "group": "global_cities",
    "iconFileName": "samarkand_icon.png",
    "localizations": {
      "ru": {
        "title": "Самарканд",
        "description": "Городская кампания про площади, базары и древние дороги.",
        "authorName": "Имя автора",
        "authorDescription": "Автор маршрута и идеи кампании."
      },
      "en": {
        "title": "Samarkand",
        "description": "A city campaign about squares, bazaars, and ancient roads.",
        "authorName": "Author Name",
        "authorDescription": "Author of the route and campaign idea."
      }
    },
    "playerTitles": [
      {
        "id": "samarkand_pathfinder",
        "localizations": {
          "ru": {
            "name": "Проводник Самарканда",
            "description": "Закрыл обязательные цели кампании."
          },
          "en": {
            "name": "Samarkand Pathfinder",
            "description": "Completed the required campaign goals."
          }
        }
      }
    ],
    "commonAchievements": [
      {
        "id": "samarkand_first_steps",
        "points": 10,
        "isRequiredForLegendary": true,
        "rewardEquipmentItem": "samarkand_silk_hat",
        "conditions": {
          "totalStepsInCategory": 1000
        },
        "sourceType": "game_milestone",
        "localizations": {
          "ru": {
            "title": "Первый след на площади",
            "short_description": "Путь начался.",
            "description": "Первые шаги кампании сделаны."
          },
          "en": {
            "title": "First Mark on the Square",
            "short_description": "The journey has started.",
            "description": "The first steps of the campaign are done."
          }
        }
      }
    ],
    "dailyQuests": [
      {
        "id": "samarkand_daily_walk",
        "points": 0,
        "conditions": {
          "stepsInDay": 7000
        },
        "progressCounter": "completionCount",
        "localizations": {
          "ru": {
            "title": "Прогулка дня",
            "short_description": "Набери шаги за день.",
            "description": "Набери шаги за день."
          },
          "en": {
            "title": "Daily Walk",
            "short_description": "Get your steps for the day.",
            "description": "Get your steps for the day."
          }
        }
      }
    ],
    "legendaryAchievement": {
      "id": "legendary_samarkand_completed",
      "points": 50,
      "rewardPlayerTitle": "samarkand_pathfinder",
      "conditions": {
        "allCommonAchievementsCompletedInCategory": "samarkand"
      },
      "localizations": {
        "ru": {
          "title": "Самарканд пройден",
          "short_description": "Все обязательные цели кампании закрыты.",
          "description": "Все обязательные цели кампании закрыты."
        },
        "en": {
          "title": "Samarkand Completed",
          "short_description": "All required campaign goals are complete.",
          "description": "All required campaign goals are complete."
        }
      }
    }
  },
  "equipmentItems": [
    {
      "id": "samarkand_silk_hat",
      "slot": "hat",
      "rarity": "rare",
      "imageFileName": "samarkand_silk_hat.png",
      "localizations": {
        "ru": {
          "name": "Шелковая тюбетейка",
          "description": "Легкая шапка для первых уверенных шагов по маршруту."
        },
        "en": {
          "name": "Silk Cap",
          "description": "A light hat for the first confident steps on the route."
        }
      }
    }
  ],
  "globalLegendaryAchievements": [
    {
      "id": "legendary_first_marathon",
      "points": 10,
      "conditions": {
        "totalDistanceMeters": 42500
      },
      "localizations": {
        "ru": {
          "title": "Первый марафон",
          "short_description": "42,5 км собраны по шагам.",
          "description": "Игрок накопил первую марафонскую дистанцию за все время."
        },
        "en": {
          "title": "First Marathon",
          "short_description": "42.5 km gathered step by step.",
          "description": "The player accumulated the first marathon distance lifetime."
        }
      }
    }
  ]
}
```

## Pull Request Checklist

- PR adds `quests/<campaign_id>/`.
- Campaign folder has `campaign.json`, `README.md`, and icon if ready.
- If the pack has `equipmentItems`, all files from `equipmentItems[].imageFileName` are included next to JSON.
- JSON is valid and has no comments.
- All ids are unique inside the pack.
- All user-facing text exists in `ru` and `en`.
- Daily/weekly quests use `points: 0`.
- Main/passive/legendary achievements use `5` to `50` points, divisible by `5`.
- Campaign legendary achievement references `campaign.id`.
- `equipmentItems` are top-level JSON, have supported slot/rarity, and have PNG/WebP `imageFileName`.
- All `rewardEquipmentItem` values reference an item from this pack or an existing active item.

## Maintainer Check And Import

### Admin UI

For packs without item images, the main maintainer path is:

1. Open Filament admin.
2. Go to `Content -> Campaign Pack Importer`.
3. Upload `campaign.json` and campaign icon.
4. Click `Check`.
5. Review campaign, author, main quests, daily/weekly, hidden achievements, legendary achievements, titles, items, errors, and warnings.
6. If status is `Importable`, click `Import`.

If the pack contains `equipmentItems` with new `imageFileName` files, use CLI import or place the item files next to JSON on the server first: the importer looks for item images in the same folder as `campaign.json`.

### CLI

Dry-run:

```bash
php artisan user-content:import-campaign quests/<campaign_id>/campaign.json --icon=quests/<campaign_id>/<icon>.png --dry-run
```

Apply:

```bash
php artisan user-content:import-campaign quests/<campaign_id>/campaign.json --icon=quests/<campaign_id>/<icon>.png --apply
```

Import rules:

- without `--apply`, the command only validates JSON and prints a report;
- all files from `equipmentItems[].imageFileName` must sit in the same folder as `campaign.json`;
- `campaign.group` must already exist as an active global group;
- one JSON imports campaign, main achievements, daily/weekly, passive hidden, legendary, global legendary, player titles, and equipment items;
- campaign icon and item images are copied into public content storage;
- `256x256` is a normal icon/item image size; smaller than `256x256` is not accepted for campaign icon;
- import does not change user progress, completions, points, wallet, or entitlements.
