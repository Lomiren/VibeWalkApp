# Шаги воспроизведения

**Campaign ID:** `steps_to_reproduce`

**Группа:** `global_games`

**Автор:** VibeWalk

**Описание автора:** Официальная кампания VibeWalk

Бета-тест на ногах: ищем баги, считаем шаги и делаем вид, что так и задумано.

## Assets

- Иконка кампании: `steps_to_reproduce_icon.png` - готова.
- Питомец: `steps_to_reproduce_bugsy.png` - готов.

## Обязательные цели

| id | Название | Условие | Очки |
| --- | --- | --- | ---: |
| `steps_to_reproduce_first_log` | Первый лог пошел | `{"totalStepsInCategory": 1000}` | 10 |
| `steps_to_reproduce_stable_repro` | Стабильно воспроизводится | `{"totalStepsInCategory": 5000}` | 10 |
| `steps_to_reproduce_ship_it` | Можно выкатывать, наверное | `{"totalStepsInCategory": 10000}` | 10 |

## Необязательные достижения

Нет.

## Daily Quests

Нет.

## Weekly Quests

Нет.

## Passive Hidden Achievements

Нет.

## Global Legendary Achievement

| id | Название | Условие | Очки | Награды |
| --- | --- | --- | ---: | --- |
| `legendary_steps_to_reproduce_completed` | Не баг, а фича | `{"allCommonAchievementsCompletedInCategory": "steps_to_reproduce"}` | 10 | `beta_hero`, `steps_to_reproduce_bugsy` |

## Player Titles

| id | Название | Как получить |
| --- | --- | --- |
| `beta_hero` | Бета-герой | Закрыть глобальное легендарное достижение `legendary_steps_to_reproduce_completed`. |

## Equipment Items

| id | Название | slot | rarity | imageFileName | Как получить |
| --- | --- | --- | --- | --- | --- |
| `steps_to_reproduce_bugsy` | Багсик | `pet` | `legendary` | `steps_to_reproduce_bugsy.png` | Закрыть глобальное легендарное достижение `legendary_steps_to_reproduce_completed`. |

## Notes

- Финальное достижение находится в `globalLegendaryAchievements`, как задано для кампании.
- Изображения не встроены в JSON: в JSON указаны только имена файлов.
