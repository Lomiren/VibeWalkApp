# User Quests for VibeWalk

[Русская версия](README.md)

VibeWalk turns ordinary walking into RPG progress. A player chooses an active campaign, and walks, steps, distance, active calories, and climbs gradually complete campaign goals. A campaign can be a city route, a book or game world, a personal challenge, or another theme. Inside a campaign there are common achievements, daily and weekly quests, rare passive discoveries, a legendary completion goal, and player titles.

This repository is for authors of community campaigns. It describes a public content pack format that is convenient for review and pull requests. After a PR is submitted, a maintainer reviews the content before it can be imported into the game.

## What You Can Propose

- `campaign` - a separate campaign: city, book, game, route, challenge, or another theme.
- `commonAchievements` - one-time main achievements inside the campaign. They award points.
- `isRequiredForLegendary: true` - a required main achievement that must be completed for campaign legendary completion.
- `isRequiredForLegendary: false` - an optional main achievement. It awards points, but does not block the legendary achievement.
- `dailyQuests` - repeatable daily quests. They do not award points.
- `weeklyQuests` - repeatable weekly quests. They do not award points.
- `passiveHiddenAchievements` - hidden passive achievements unlocked by user behavior or an app event.
- `legendaryAchievement` - a rare achievement for a major completion, usually for completing all required common achievements in the campaign.
- `globalLegendaryAchievements` - legendary achievements for overall player progress, not tied to a specific campaign.
- `playerTitles` - proposed player titles. An achievement can reference a title through `rewardPlayerTitle`.

## How To Use The AI Agent

1. Open a new chat with an AI agent.
2. Paste or attach `AI_AGENT_INSTRUCTIONS.en.md` as the persistent instruction for an English chat. For a Russian chat, use `AI_AGENT_INSTRUCTIONS.md`.
3. In the first message, give the agent your campaign theme and ask it to interview you.
4. Answer the agent's questions: title, author, group, style, required and optional goals, daily/weekly quests, passive/legendary ideas, and titles.
5. Review the preview tables: titles, conditions, points, required flags, and titles.
6. Ask the agent to output the final JSON content pack and the text for the campaign `README.md`. A campaign must always be one JSON file: common achievements, daily/weekly quests, passive hidden achievements, legendary achievement, and player titles are all kept together.
7. Open a pull request with your campaign folder inside `quests/`.

Example first message to the agent:

```text
I want to create a "Night City" campaign for VibeWalk. Interview me using the instructions, help me design required and optional achievements, daily/weekly quests, one legendary achievement, and a title for completing the campaign.
```

More example prompts:

```text
Create a campaign about Samarkand in the "Cities" group: route walking, bazaars, squares, ancient roads. I want 12 required achievements, 3 optional achievements, 2 daily quests, and 2 weekly quests.
```

```text
Help me prepare a campaign about a fictional space expedition. Conditions should use only steps and weekly steps. All text must be in Russian and English.
```

```text
Check my JSON content pack: find duplicate ids, invalid points, missing ru/en localizations, and unsupported conditions.
```

## Pull Request Structure

Community campaigns live in the `quests/` folder. For a new campaign, create a separate folder with the same `snake_case` name as `campaign.id`.

```text
quests/
  samarkand/
    README.md
    campaign.json
    samarkand_icon.png
```

What must be inside the campaign folder:

- `campaign.json` - one `packType: "campaign"` JSON file for export into the game. It contains common achievements, daily/weekly quests, passive hidden achievements, the campaign legendary achievement, optional global legendary achievements, player titles, and the campaign author.
- `README.md` - a human-readable campaign description: what the campaign is, who the author is, the idea, the quest and achievement list, each condition, point rewards, and unlocked titles.
- Campaign icon - a PNG or WebP file with the same name as `campaign.iconFileName` in JSON. If the icon is not ready yet, mention that in the PR and in the campaign `README.md`.

The minimum `README.md` inside a campaign folder must include:

- campaign title;
- author and short author description;
- simple description of the campaign idea;
- required common achievements with conditions and points;
- optional achievements with conditions and points;
- daily and weekly quests with conditions;
- passive hidden achievements, if any;
- legendary achievement and its condition;
- global legendary achievements, if any;
- player titles and how they are awarded;
- icon status.

## Content Rules

- All `id` values and aliases must use Latin `snake_case`, without spaces or random suffixes.
- Content must have both `ru` and `en` localizations.
- Common, passive, and legendary achievements may award only `10`, `20`, or `50` points.
- Daily and weekly quests always use `points: 0`.
- Soft currency is not described in JSON. The game calculates it separately from the content pack.
- Conditions inside JSON use camelCase.
- A community campaign is submitted as one `packType: "campaign"` JSON file. If the campaign has passive achievements, add them to `campaign.passiveHiddenAchievements`.
- Legendary achievements do not have to be tied to a campaign. If an achievement describes overall player progress, add it to top-level `globalLegendaryAchievements` in the same `campaign.json`.
- Every campaign must have an author: `authorName` and `authorDescription` in both `ru` and `en`.
- Do not embed images in JSON. If there is an icon, set its file name in `iconFileName` and attach the image file separately.

Supported campaign groups:

| group | Name |
| --- | --- |
| `global_cities` | Cities |
| `global_games` | Games |
| `global_books` | Books |

If you need a new group, mention it in the PR idea. It requires a separate maintainer decision.

## Conditions

Conditions are written inside `conditions` and use camelCase. If one achievement has multiple conditions, they work as AND: every condition must be satisfied. For new campaigns, it is better to start with basic conditions around steps, distance, calories, and daily/weekly goals. Advanced conditions are also available in the format, but mark them clearly in the PR so a maintainer can review the idea separately.

Do not add a `raw` field: it is not part of the community content pack.

### All Supported Condition Keys

| Condition | Plain description | Typical use | Example |
| --- | --- | --- | --- |
| `totalStepsInCategory` | Accumulate N steps inside this campaign. | Campaign common achievements | `{"totalStepsInCategory": 100000}` |
| `totalDistanceMetersInCategory` | Accumulate N meters inside the campaign. You can add `categoryId` if the condition must look at a specific campaign. | Common, passive, legendary | `{"totalDistanceMetersInCategory": 42500, "categoryId": "samarkand"}` |
| `totalDistanceMeters` | Accumulate N meters lifetime across all campaigns. | Global legendary | `{"totalDistanceMeters": 42500}` |
| `totalActiveCaloriesInCategory` | Accumulate N active calories inside the campaign. | Campaign common achievements | `{"totalActiveCaloriesInCategory": 10000}` |
| `totalStairsEquivalentInCategory` | Accumulate N floors or stair equivalents inside the campaign. | Campaign common achievements | `{"totalStairsEquivalentInCategory": 842}` |
| `stepsInDay` | Reach N steps during the current local day of the active campaign. | Daily quests | `{"stepsInDay": 7000}` |
| `activeCaloriesInDay` | Reach N active calories during the current day. Often combined with `stepsInDay`. | Daily quests | `{"stepsInDay": 8000, "activeCaloriesInDay": 300}` |
| `stepsInWeek` | Reach N steps during the current week of the active campaign. | Weekly quests | `{"stepsInWeek": 50000}` |
| `activeCaloriesInWeek` | Reach N active calories during the current week. | Weekly quests | `{"stepsInWeek": 60000, "activeCaloriesInWeek": 1500}` |
| `daysInWeekWithAtLeastSteps` | During a week, reach at least `steps` steps on `days` different days. | Weekly quests | `{"daysInWeekWithAtLeastSteps": {"days": 5, "steps": 5000}}` |
| `event` | A user or app event. Values are listed below. | Passive achievements | `{"event": "firstCategorySelected"}` |
| `times` | How many times the event must happen. Currently useful for `categoryChanged`. | Passive achievements | `{"event": "categoryChanged", "times": 1}` |
| `categoryId` | Bind an event or distance condition to a specific campaign by `campaign.id`. | Passive, legendary | `{"event": "categorySelected", "categoryId": "samarkand"}` |
| `stepsInDayLessThan` | A completed day had fewer than N steps. | Passive achievements | `{"stepsInDayLessThan": 2000}` |
| `stepsInDayBetween` | A completed day had steps in the `min`-`max` range. | Passive achievements | `{"stepsInDayBetween": {"min": 1000, "max": 3000}}` |
| `activeCaloriesInDayLessThan` | A completed day had fewer than N active calories. | Passive achievements | `{"activeCaloriesInDayLessThan": 100}` |
| `appNotOpenedForDaysAtLeast` | The app was not opened for at least N days. Usually combined with `thenEvent`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 7, "thenEvent": "appOpened"}` |
| `thenEvent` | An event that must happen after another condition. Currently usually `appOpened`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 7, "thenEvent": "appOpened"}` |
| `stepsWhileAppNotOpenedAtLeast` | Accumulate N steps while the app was not opened. Usually combined with `appNotOpenedForDaysAtLeast`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 3, "stepsWhileAppNotOpenedAtLeast": 20000}` |
| `stepsInDayAtLeast` | A day had at least N steps. | Passive achievements | `{"stepsInDayAtLeast": 5000}` |
| `appOpenedSameDay` | The app was opened on the same day as the daily condition. | Advanced passive | `{"stepsInDayAtLeast": 5000, "appOpenedSameDay": true}` |
| `stepsBeforeLocalTime` | Reach N steps before local time `HH:mm`. | Advanced passive | `{"stepsBeforeLocalTime": {"time": "10:00", "steps": 2000}}` |
| `stepsAfterLocalTime` | Reach N steps after local time `HH:mm`. | Advanced passive | `{"stepsAfterLocalTime": {"time": "18:00", "steps": 5000}}` |
| `appOpenedAfterLocalTime` | Open the app after local time `HH:mm`. | Advanced passive | `{"appOpenedAfterLocalTime": "23:00"}` |
| `previousDayStepsAtLeast` | The previous day had at least N steps. Usually combined with `currentDayStepsLessThan`. | Passive achievements | `{"previousDayStepsAtLeast": 12000, "currentDayStepsLessThan": 3000}` |
| `currentDayStepsLessThan` | The current checked day has fewer than N steps. Usually combined with `previousDayStepsAtLeast`. | Passive achievements | `{"previousDayStepsAtLeast": 12000, "currentDayStepsLessThan": 3000}` |
| `differenceBetweenTwoConsecutiveDaysStepsAtLeast` | The step difference between two consecutive days is at least N. | Passive achievements | `{"differenceBetweenTwoConsecutiveDaysStepsAtLeast": 10000}` |
| `weekendTotalStepsLessThan` | Saturday and Sunday together had fewer than N steps. | Passive achievements | `{"weekendTotalStepsLessThan": 5000}` |
| `saturdayStepsLessThan` | Saturday had fewer than N steps. Usually combined with `sundayStepsLessThan`. | Passive achievements | `{"saturdayStepsLessThan": 3000, "sundayStepsLessThan": 3000}` |
| `sundayStepsLessThan` | Sunday had fewer than N steps. Usually combined with `saturdayStepsLessThan`. | Passive achievements | `{"saturdayStepsLessThan": 3000, "sundayStepsLessThan": 3000}` |
| `dayOfWeek` | Restrict the condition to a weekday. Values: `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`. | Passive achievements | `{"dayOfWeek": "monday", "stepsInDayLessThan": 3000}` |
| `activeCategorySelected` | The player has an active campaign selected. | Passive achievements | `{"activeCategorySelected": true}` |
| `softCurrencyEarnedAtLeast` | The player has earned at least N soft currency lifetime. Spending does not reduce this counter. | Passive achievements | `{"softCurrencyEarnedAtLeast": 50}` |
| `campaignUnlocksAtLeast` | The player has unlocked at least N paid campaigns. Free campaigns do not count as purchases. | Passive achievements | `{"campaignUnlocksAtLeast": 3}` |
| `timeWindowSteps` | For a recent short period, find days or weekends where steps in a time window are inside a range. Supports `days`, `startTime`, `endTime`, `minSteps`, `maxSteps`, `occurrences`, `consecutive`, `period`. | Passive achievements | `{"timeWindowSteps": {"days": ["monday"], "startTime": "06:00", "endTime": "12:00", "minSteps": 3000, "occurrences": 1}}` |
| `weeklyStepsRange` | For a recent short period, find weeks where total steps are inside a range. Supports `minSteps`, `maxSteps`, `occurrences`, `consecutive`. | Passive achievements | `{"weeklyStepsRange": {"minSteps": 30000, "occurrences": 1}}` |
| `allCommonAchievementsCompletedInCategory` | Complete all required common achievements in the specified campaign. Optional achievements do not block this condition. | Legendary achievements | `{"allCommonAchievementsCompletedInCategory": "samarkand"}` |
| `allLegendaryAchievementsCompleted` | Complete a list of other legendary achievements. | Legendary achievements | `{"allLegendaryAchievementsCompleted": ["legendary_samarkand_completed"]}` |
| `metric` + `target` | Legacy/global lifetime condition based on total player stats. Supported `metric`: `steps`, `distance_meters`, `flights`, `active_energy_kcal`. Use only for global legendary achievements and call it out in the PR. | Global legendary achievements | `{"metric": "steps", "target": 1000000}` |

Supported `event` values: `firstAppLaunch`, `healthAccessGranted`, `firstStepsDataReceived`, `firstCategorySelected`, `categoryChanged`, `categorySelected`.

Fields for `timeWindowSteps`:

- `days`: optional list of weekdays in English, for example `["monday", "wednesday"]`.
- `startTime` and `endTime`: optional local time in `HH:mm` format; `24:00` is allowed as the end of a window.
- `minSteps` or `maxSteps`: minimum or maximum steps in the window. At least one of these fields must be set.
- `occurrences`: how many times the condition must match; default is `1`.
- `consecutive`: `true` if matches must be consecutive.
- `period`: `day` or `weekend`; default is `day`.

Fields for `weeklyStepsRange`:

- `minSteps` or `maxSteps`: minimum or maximum steps for the week. At least one of these fields must be set.
- `occurrences`: how many weeks must match the range; default is `1`.
- `consecutive`: `true` if weeks must be consecutive.

### Global Legendary Achievements

Global legendary achievements are not tied to a specific campaign and do not require `categoryId`. They describe overall player progress across the whole game.

Examples:

| Idea | Condition | Note |
| --- | --- | --- |
| First 100,000 steps | `{"metric": "steps", "target": 100000}` | Global lifetime steps; mark it as global legendary in the PR. |
| First million steps | `{"metric": "steps", "target": 1000000}` | Global lifetime steps; a rare long-term goal. |
| First marathon | `{"totalDistanceMeters": 42500}` | Lifetime distance across all campaigns. |

## Campaign Questionnaire

Before final JSON, collect these answers:

1. What is the campaign called?
2. Who is the author, and how should the author be described in one phrase? Author is required for both `ru` and `en`.
3. Which group fits: `global_cities`, `global_games`, `global_books`, or a new one?
4. What text style should be used: calm, adventurous, ironic, historical, fantasy?
5. Which common achievements are required for campaign completion?
6. Which common achievements are optional and do not block the legendary achievement?
7. Which daily quests are needed?
8. Which weekly quests are needed?
9. Are passive hidden achievements needed?
10. Is a legendary achievement needed? By default, it unlocks after all required common campaign achievements.
11. Which player titles are needed and how are they awarded? By default, propose one title for completing all required campaign goals.
12. Is there a campaign icon? If yes, attach the PNG/WebP separately and set its file name.

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
      },
      {
        "id": "samarkand_hidden_courtyard",
        "points": 20,
        "isRequiredForLegendary": false,
        "conditions": {
          "totalStepsInCategory": 25000
        },
        "sourceType": "optional_challenge",
        "localizations": {
          "ru": {
            "title": "Скрытый двор",
            "short_description": "Необязательный маршрут найден.",
            "description": "Дополнительная цель закрыта, но легендарку она не блокирует."
          },
          "en": {
            "title": "Hidden Courtyard",
            "short_description": "An optional route was found.",
            "description": "The optional goal is complete, but it does not block the legendary achievement."
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
    "weeklyQuests": [
      {
        "id": "samarkand_weekly_route",
        "points": 0,
        "conditions": {
          "stepsInWeek": 50000
        },
        "progressCounter": "completionCount",
        "localizations": {
          "ru": {
            "title": "Неделя дороги",
            "short_description": "Набери шаги за неделю.",
            "description": "Набери шаги за неделю."
          },
          "en": {
            "title": "Road Week",
            "short_description": "Get your steps for the week.",
            "description": "Get your steps for the week."
          }
        }
      }
    ],
    "passiveHiddenAchievements": [
      {
        "id": "passive_samarkand_path_chosen",
        "type": "passive_hidden",
        "scope": "campaign",
        "points": 10,
        "isHiddenUntilUnlocked": true,
        "conditions": {
          "event": "categorySelected",
          "categoryId": "samarkand"
        },
        "localizations": {
          "ru": {
            "title": "Путь к площади выбран",
            "short_description": "Кампания Самарканда стала активной.",
            "description": "Пользователь выбрал Самарканд как активную кампанию."
          },
          "en": {
            "title": "Path to the Square Chosen",
            "short_description": "The Samarkand campaign became active.",
            "description": "The user selected Samarkand as the active campaign."
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

- The PR adds a folder under `quests/<campaign_id>/`.
- The campaign folder contains `campaign.json`, `README.md`, and an icon if it is ready.
- The PR has one `packType: "campaign"` JSON content pack.
- The campaign `README.md` describes the idea, author, quests, achievements, conditions, points, and titles in a human-readable way.
- The PR description includes a short campaign idea.
- The campaign author is set in JSON: `authorName` and `authorDescription` for both `ru` and `en`.
- Icon status is stated: attached, help needed, or no icon yet.
- JSON is valid and contains no comments.
- All `id` values are unique inside the pack.
- All user-facing text exists in both `ru` and `en`.
- Daily/weekly quests have `points: 0`.
- Main/passive/legendary achievements use only `10`, `20`, or `50` points.
- The campaign legendary achievement references `campaign.id`.
- Global legendary achievements, if any, are placed in `globalLegendaryAchievements` and do not require campaign binding.
