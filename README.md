# User Quests for VibeWalk

[English version](README.en.md)

VibeWalk превращает обычную ходьбу в RPG-прогресс. Игрок выбирает активную кампанию, а прогулки, шаги, дистанция, активные калории и подъемы постепенно закрывают цели кампании. Кампания может быть городским маршрутом, книжным или игровым миром, личным челленджем или другой темой. Внутри нее есть основные достижения, daily/weekly квесты, редкие пассивные находки, легендарное завершение и звания персонажа.

Эта папка нужна для авторов пользовательских кампаний. Здесь описан публичный content pack: он удобен для ревью и pull request. После PR maintainer проверяет контент и переносит его в формат приложения.

## Что можно предложить

- `campaign` - отдельная кампания: город, книга, игра, маршрут, челлендж или другая тема.
- `commonAchievements` - основные достижения кампании. Они одноразовые и дают очки.
- `isRequiredForLegendary: true` - обязательное основное достижение, которое нужно закрыть для легендарного завершения кампании.
- `isRequiredForLegendary: false` - необязательное основное достижение. Оно дает награды, но не блокирует легендарку.
- `dailyQuests` - ежедневные повторяемые квесты. Они не дают очки.
- `weeklyQuests` - еженедельные повторяемые квесты. Они не дают очки.
- `passiveHiddenAchievements` - скрытые пассивные достижения, которые открываются по поведению пользователя или системному событию.
- `legendaryAchievement` - редкое достижение за крупное завершение, обычно за закрытие всех обязательных основных достижений кампании.
- `globalLegendaryAchievements` - легендарные достижения за общий прогресс игрока, не привязанные к конкретной кампании.
- `playerTitles` - предлагаемые звания персонажа. Достижение может ссылаться на звание через `rewardPlayerTitle`.

## Как использовать ИИ агента

1. Откройте новый чат с ИИ агентом.
2. Вставьте или прикрепите содержимое `AI_AGENT_INSTRUCTIONS.md` как постоянную инструкцию для русскоязычного чата. Для англоязычного чата используйте `AI_AGENT_INSTRUCTIONS.en.md`.
3. Первым сообщением дайте агенту тему кампании и попросите провести интервью.
4. Ответьте на вопросы агента: название, автор, группа, стиль, обязательные и необязательные цели, daily/weekly квесты, passive/legendary идеи и звания.
5. Проверьте preview-таблицы: названия, условия, очки, обязательность, звания.
6. Попросите агента вывести финальный JSON content pack и текст для `README.md` кампании. Для кампании это всегда один JSON-файл: основные достижения, daily/weekly, passive hidden, legendary и звания находятся вместе.
7. Сделайте pull request с папкой своей кампании внутри `quests/`.

Пример первого сообщения агенту:

```text
Хочу создать кампанию "Ночной город" для VibeWalk. Проведи интервью по инструкции, помоги придумать обязательные и необязательные достижения, daily/weekly квесты, одну легендарку и звание за завершение кампании.
```

Еще примеры запросов:

```text
Сделай кампанию про Самарканд в группе "Города": маршрутная прогулка, базары, площади, древние дороги. Хочу 12 обязательных достижений, 3 необязательных, 2 daily и 2 weekly.
```

```text
Помоги оформить кампанию по вымышленной космической экспедиции. Условия должны быть только по шагам и недельным шагам. Все тексты нужны на русском и английском.
```

```text
Проверь мой JSON content pack: найди дубли id, неправильные points, отсутствующие ru/en локализации и неподдержанные условия.
```

## Структура pull request

В репозитории пользовательские кампании лежат в папке `quests/`. Для новой кампании создайте отдельную папку с тем же `snake_case` именем, что и `campaign.id`.

```text
quests/
  samarkand/
    README.md
    campaign.json
    samarkand_icon.png
```

Что должно быть в папке кампании:

- `campaign.json` - один `packType: "campaign"` JSON для экспорта в игру. В нем находятся основные достижения, daily/weekly квесты, passive hidden achievements, campaign legendary achievement, optional global legendary achievements, звания и автор кампании.
- `README.md` - человекочитаемое описание кампании: что это за кампания, кто автор, какая идея, какие квесты и достижения есть, какие условия у каждого пункта, сколько очков дается и какие звания открываются.
- Иконка кампании - PNG или WebP файл, имя которого совпадает с `campaign.iconFileName` в JSON. Если иконки пока нет, все равно укажите это в PR и в `README.md`.

Минимальный `README.md` внутри папки кампании должен содержать:

- название кампании;
- автора и короткое описание автора;
- простое описание сути кампании;
- список обязательных основных достижений с условиями и очками;
- список необязательных достижений с условиями и очками;
- список daily и weekly квестов с условиями;
- список passive hidden achievements, если они есть;
- легендарное достижение и его условие;
- глобальные легендарные достижения, если они есть;
- звания персонажа и за что они выдаются;
- статус иконки.

## Правила контента

- Все `id` и aliases пишутся в `snake_case`, латиницей, без пробелов и случайных суффиксов.
- Контент должен иметь локализации `ru` и `en`.
- Основные, пассивные и легендарные достижения дают только `10`, `20` или `50` очков.
- Daily и weekly квесты всегда имеют `points: 0`.
- Внутренняя валюта не описывается в JSON. Игра рассчитывает ее отдельно от content pack.
- Условия внутри JSON пишутся в camelCase.
- Пользовательская кампания оформляется одним `packType: "campaign"` JSON-файлом. Если у кампании есть пассивные достижения, добавляйте их в `campaign.passiveHiddenAchievements`.
- Легендарные достижения не обязаны быть привязаны к кампании. Если достижение относится к общему прогрессу игрока, добавляйте его в top-level `globalLegendaryAchievements` в том же `campaign.json`.
- У кампании обязательно должен быть автор: `authorName` и `authorDescription` в `ru` и `en`.
- Не вставляйте изображения в JSON. Если есть иконка, укажите имя файла в `iconFileName` и приложите файл отдельно.

Поддержанные группы кампаний:

| group | Название |
| --- | --- |
| `global_cities` | Города |
| `global_games` | Игры |
| `global_books` | Книги |

Если нужна новая группа, укажите ее в идее PR. Это потребует отдельного решения maintainer-а.

## Условия

Условия пишутся внутри `conditions` и используют camelCase. Если в одном достижении несколько условий, они работают как AND: нужно выполнить все. Для новых кампаний лучше начинать с базовых условий по шагам, дистанции, калориям и weekly/daily целям. Advanced-условия тоже доступны в формате, но их стоит явно отметить в PR, чтобы maintainer отдельно проверил идею.

Не добавляйте поле `raw`: это не часть пользовательского content pack.

### Все доступные condition keys

| Condition | Понятное описание | Где обычно использовать | Пример |
| --- | --- | --- | --- |
| `totalStepsInCategory` | Накопить N шагов внутри этой кампании. | Основные достижения кампании | `{"totalStepsInCategory": 100000}` |
| `totalDistanceMetersInCategory` | Накопить N метров внутри кампании. Можно добавить `categoryId`, если условие должно смотреть на конкретную кампанию. | Основные, passive, legendary | `{"totalDistanceMetersInCategory": 42500, "categoryId": "samarkand"}` |
| `totalDistanceMeters` | Накопить N метров за все время по всем кампаниям. | Глобальные legendary | `{"totalDistanceMeters": 42500}` |
| `totalActiveCaloriesInCategory` | Накопить N активных калорий внутри кампании. | Основные достижения кампании | `{"totalActiveCaloriesInCategory": 10000}` |
| `totalStairsEquivalentInCategory` | Накопить N этажей или условных ступеней внутри кампании. | Основные достижения кампании | `{"totalStairsEquivalentInCategory": 842}` |
| `stepsInDay` | Набрать N шагов за текущий локальный день активной кампании. | Daily quests | `{"stepsInDay": 7000}` |
| `activeCaloriesInDay` | Набрать N активных калорий за текущий день. Часто комбинируется с `stepsInDay`. | Daily quests | `{"stepsInDay": 8000, "activeCaloriesInDay": 300}` |
| `stepsInWeek` | Набрать N шагов за текущую неделю активной кампании. | Weekly quests | `{"stepsInWeek": 50000}` |
| `activeCaloriesInWeek` | Набрать N активных калорий за текущую неделю. | Weekly quests | `{"stepsInWeek": 60000, "activeCaloriesInWeek": 1500}` |
| `daysInWeekWithAtLeastSteps` | В течение недели сделать минимум `steps` шагов в `days` разных дней. | Weekly quests | `{"daysInWeekWithAtLeastSteps": {"days": 5, "steps": 5000}}` |
| `event` | Событие пользователя или приложения. Значения перечислены ниже. | Passive achievements | `{"event": "firstCategorySelected"}` |
| `times` | Сколько раз должно повториться событие. Сейчас полезно для `categoryChanged`. | Passive achievements | `{"event": "categoryChanged", "times": 1}` |
| `categoryId` | Привязать событие или дистанцию к конкретной кампании по `campaign.id`. | Passive, legendary | `{"event": "categorySelected", "categoryId": "samarkand"}` |
| `stepsInDayLessThan` | За завершенный день набрано меньше N шагов. | Passive achievements | `{"stepsInDayLessThan": 2000}` |
| `stepsInDayBetween` | За завершенный день шаги попали в диапазон `min`-`max`. | Passive achievements | `{"stepsInDayBetween": {"min": 1000, "max": 3000}}` |
| `activeCaloriesInDayLessThan` | За завершенный день набрано меньше N активных калорий. | Passive achievements | `{"activeCaloriesInDayLessThan": 100}` |
| `appNotOpenedForDaysAtLeast` | Приложение не открывали минимум N дней. Обычно комбинируется с `thenEvent`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 7, "thenEvent": "appOpened"}` |
| `thenEvent` | Событие, которое должно произойти после другого условия. Сейчас обычно используется `appOpened`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 7, "thenEvent": "appOpened"}` |
| `stepsWhileAppNotOpenedAtLeast` | Набрать N шагов, пока приложение не открывали. Обычно комбинируется с `appNotOpenedForDaysAtLeast`. | Advanced passive | `{"appNotOpenedForDaysAtLeast": 3, "stepsWhileAppNotOpenedAtLeast": 20000}` |
| `stepsInDayAtLeast` | За день набрано минимум N шагов. | Passive achievements | `{"stepsInDayAtLeast": 5000}` |
| `appOpenedSameDay` | Приложение открыто в тот же день, когда выполняется дневное условие. | Advanced passive | `{"stepsInDayAtLeast": 5000, "appOpenedSameDay": true}` |
| `stepsBeforeLocalTime` | Набрать N шагов до локального времени `HH:mm`. | Advanced passive | `{"stepsBeforeLocalTime": {"time": "10:00", "steps": 2000}}` |
| `stepsAfterLocalTime` | Набрать N шагов после локального времени `HH:mm`. | Advanced passive | `{"stepsAfterLocalTime": {"time": "18:00", "steps": 5000}}` |
| `appOpenedAfterLocalTime` | Открыть приложение после локального времени `HH:mm`. | Advanced passive | `{"appOpenedAfterLocalTime": "23:00"}` |
| `previousDayStepsAtLeast` | В предыдущий день было минимум N шагов. Обычно комбинируется с `currentDayStepsLessThan`. | Passive achievements | `{"previousDayStepsAtLeast": 12000, "currentDayStepsLessThan": 3000}` |
| `currentDayStepsLessThan` | В текущий проверяемый день меньше N шагов. Обычно комбинируется с `previousDayStepsAtLeast`. | Passive achievements | `{"previousDayStepsAtLeast": 12000, "currentDayStepsLessThan": 3000}` |
| `differenceBetweenTwoConsecutiveDaysStepsAtLeast` | Разница шагов между двумя соседними днями не меньше N. | Passive achievements | `{"differenceBetweenTwoConsecutiveDaysStepsAtLeast": 10000}` |
| `weekendTotalStepsLessThan` | За субботу и воскресенье вместе меньше N шагов. | Passive achievements | `{"weekendTotalStepsLessThan": 5000}` |
| `saturdayStepsLessThan` | В субботу меньше N шагов. Обычно комбинируется с `sundayStepsLessThan`. | Passive achievements | `{"saturdayStepsLessThan": 3000, "sundayStepsLessThan": 3000}` |
| `sundayStepsLessThan` | В воскресенье меньше N шагов. Обычно комбинируется с `saturdayStepsLessThan`. | Passive achievements | `{"saturdayStepsLessThan": 3000, "sundayStepsLessThan": 3000}` |
| `dayOfWeek` | Ограничить условие днем недели. Значения: `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`, `sunday`. | Passive achievements | `{"dayOfWeek": "monday", "stepsInDayLessThan": 3000}` |
| `activeCategorySelected` | У игрока выбрана активная кампания. | Passive achievements | `{"activeCategorySelected": true}` |
| `softCurrencyEarnedAtLeast` | За все время заработано минимум N внутренней валюты. Траты не уменьшают этот счетчик. | Passive achievements | `{"softCurrencyEarnedAtLeast": 50}` |
| `campaignUnlocksAtLeast` | Открыто минимум N платных кампаний. Бесплатные кампании не считаются покупкой. | Passive achievements | `{"campaignUnlocksAtLeast": 3}` |
| `timeWindowSteps` | За короткий недавний период найти дни или выходные, где шаги в окне времени попали в диапазон. Поддерживает `days`, `startTime`, `endTime`, `minSteps`, `maxSteps`, `occurrences`, `consecutive`, `period`. | Passive achievements | `{"timeWindowSteps": {"days": ["monday"], "startTime": "06:00", "endTime": "12:00", "minSteps": 3000, "occurrences": 1}}` |
| `weeklyStepsRange` | За короткий недавний период найти недели, где сумма шагов попала в диапазон. Поддерживает `minSteps`, `maxSteps`, `occurrences`, `consecutive`. | Passive achievements | `{"weeklyStepsRange": {"minSteps": 30000, "occurrences": 1}}` |
| `allCommonAchievementsCompletedInCategory` | Закрыть все обязательные основные достижения указанной кампании. Optional achievements не блокируют это условие. | Legendary achievements | `{"allCommonAchievementsCompletedInCategory": "samarkand"}` |
| `allLegendaryAchievementsCompleted` | Закрыть список других legendary achievements. | Legendary achievements | `{"allLegendaryAchievementsCompleted": ["legendary_samarkand_completed"]}` |
| `metric` + `target` | Legacy/global lifetime-условие по суммарной статистике игрока. Поддержанные `metric`: `steps`, `distance_meters`, `flights`, `active_energy_kcal`. Используйте только для global legendary и явно отмечайте в PR. | Global legendary achievements | `{"metric": "steps", "target": 1000000}` |

Поддержанные `event` values: `firstAppLaunch`, `healthAccessGranted`, `firstStepsDataReceived`, `firstCategorySelected`, `categoryChanged`, `categorySelected`.

Поля для `timeWindowSteps`:

- `days`: optional список дней недели на английском, например `["monday", "wednesday"]`.
- `startTime` и `endTime`: optional локальное время в формате `HH:mm`; `24:00` допустимо для конца окна.
- `minSteps` или `maxSteps`: минимум или максимум шагов в окне. Нужно указать хотя бы одно из этих полей.
- `occurrences`: сколько раз условие должно встретиться; по умолчанию `1`.
- `consecutive`: `true`, если совпадения должны идти подряд.
- `period`: `day` или `weekend`; по умолчанию `day`.

Поля для `weeklyStepsRange`:

- `minSteps` или `maxSteps`: минимум или максимум шагов за неделю. Нужно указать хотя бы одно из этих полей.
- `occurrences`: сколько недель должны попасть в диапазон; по умолчанию `1`.
- `consecutive`: `true`, если недели должны идти подряд.

### Глобальные легендарные достижения

Глобальные legendary achievements не привязаны к конкретной кампании и не требуют `categoryId`. Они описывают общий прогресс игрока во всей игре.

Примеры:

| Идея | Условие | Комментарий |
| --- | --- | --- |
| Первые 100 000 шагов | `{"metric": "steps", "target": 100000}` | Global lifetime steps; отметьте как global legendary в PR. |
| Первый миллион шагов | `{"metric": "steps", "target": 1000000}` | Global lifetime steps; редкая долгосрочная цель. |
| Первый марафон | `{"totalDistanceMeters": 42500}` | Lifetime-дистанция по всем кампаниям. |

## Опросник для кампании

Перед финальным JSON соберите ответы:

1. Как называется кампания?
2. Кто автор и как его описать в одну фразу? Автор обязателен для `ru` и `en`.
3. Какая группа подходит: `global_cities`, `global_games`, `global_books` или новая?
4. Какой стиль текстов: спокойный, приключенческий, ироничный, исторический, фантастический?
5. Какие основные достижения обязательные для завершения кампании?
6. Какие основные достижения необязательные и не блокируют легендарку?
7. Какие daily quests нужны?
8. Какие weekly quests нужны?
9. Нужны ли passive hidden achievements?
10. Нужна ли легендарка? По умолчанию она открывается за все обязательные основные достижения кампании.
11. Какие звания персонажа нужны и за что они выдаются? По умолчанию можно предложить одно звание за закрытие всех обязательных целей кампании.
12. Есть ли иконка кампании? Если да, приложите PNG/WebP отдельно и укажите имя файла.

## Campaign content pack

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

## Pull request checklist

- В PR добавлена папка `quests/<campaign_id>/`.
- В папке кампании есть `campaign.json`, `README.md` и иконка, если она готова.
- В PR есть один `packType: "campaign"` JSON content pack.
- В `README.md` кампании человекочитаемо описаны идея, автор, квесты, достижения, условия, очки и звания.
- В описании PR есть короткая идея кампании.
- Указан автор кампании в JSON: `authorName` и `authorDescription` для `ru` и `en`.
- Указан статус иконки: приложена, нужна помощь или иконки пока нет.
- JSON валиден и не содержит комментариев.
- Все `id` уникальны внутри pack.
- Все пользовательские тексты есть на `ru` и `en`.
- Daily/weekly имеют `points: 0`.
- Main/passive/legendary имеют только `10`, `20` или `50` очков.
- Легендарка кампании ссылается на `campaign.id`.
- Глобальные легендарные достижения, если есть, лежат в `globalLegendaryAchievements` и не требуют привязки к кампании.
