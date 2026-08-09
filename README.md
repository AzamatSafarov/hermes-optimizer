# ⚙️ Hermes Optimizer

<div align="center">

```
╔══════════════════════════════════════════════════════╗
║  Inspect → Discover → Verify → Route → Optimize     ║
╚══════════════════════════════════════════════════════╝
```

[![GitHub](https://img.shields.io/badge/GitHub-AzamatSafarov%2Fhermes--optimizer-161b22?style=flat-square&logo=github)](https://github.com/AzamatSafarov/hermes-optimizer)
[![Status](https://img.shields.io/badge/status-active-success?style=flat-square)](#)
[![Skill](https://img.shields.io/badge/type-Hermes_Skill-blue?style=flat-square)](#)
[![Providers](https://img.shields.io/badge/providers-agnostic-8b5cf6?style=flat-square)](#)
[![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)](LICENSE)

> *Проверяет live-конфиг, находит модели, строит маршруты — без ручного списка.*

</div>

## Что это

**Hermes Optimizer** — reusable skill для оптимизации маршрутизации моделей, очистки скиллов и live-автодискаверинга. Не навязывает пресеты: читает активный `config.yaml`, тестирует реальные провайдеры и строит рекомендации из того, что реально доступно.

## Архитектура

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Пользователь: │───→│   Inspect       │───→│   Detect        │
│  "Оптимизируй   │    │   config.yaml   │    │   providers +   │
│   мой Hermes"   │    │   + routes      │    │   subscriptions │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                       │
                              ┌──────────────────────┘
                              ▼
                       ┌─────────────────┐
                       │   Probe models  │
                       │   live (real    │
                       │   API calls)    │
                       └─────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                   ▼                   ▼
    ┌───────────┐      ┌───────────┐      ┌───────────┐
    │   Heavy   │      │  Medium   │      │   Light   │
    │   lane    │      │   lane    │      │   lane    │
    └───────────┘      └───────────┘      └───────────┘
```

## Структура

```
hermes-optimizer/
├── 📖 README.md                    ← ты здесь
├── ⚙️ SKILL.md                     ← runtime skill
├── 📜 CHANGELOG.md                 ← история изменений
├── 📋 KANBAN.md                    ← доска задач
├── 📄 LICENSE                      ← MIT
│
├── 📦 llms.txt                     ← машиночитаемое summary
├── 📦 llms-full.txt                ← полная версия
│
├── 🛠️ references/                  ← правила и логика
│   ├── provider-autodiscovery.md   ← как определять провайдеров
│   ├── platform-skill-trim.md      ← platform-aware cleanup
│   ├── imported-skills.md          ← откуда пришли скиллы
│   └── synthesis-map.md            ← маппинг источников
│
├── 📝 notes/
│   └── current-routing.md          ← generic provider-примеры
│
├── 🎨 assets/
│   ├── hermes-optimizer-mark.svg   ← логотип
│   ├── hermes-optimizer-hero.svg   ← hero banner
│   ├── architecture-flow.svg       ← workflow diagram
│   └── social-preview.png          ← GitHub social preview
│
└── .github/
    └── workflows/
        └── repo-readme-quality.yml ← CI lint
```

## Статус компонентов

| Компонент | Статус | Описание |
|-----------|--------|----------|
| Provider autodiscovery | ✅ Работает | Читает live config, находит провайдеров |
| Automatic model inventory | ✅ Работает | Перечисляет достижимые модели |
| Lane-based routing | ✅ Работает | Heavy / Medium / Light / Vision lanes |
| Live verification | ✅ Работает | Тестовые вызовы перед рекомендациями |
| Guardrail upgrades | ✅ Работает | Защита от зацикливания, чекпойнты |
| Skill-bloat cleanup | ✅ Работает | Безопасное отключение (disable, не delete) |
| Platform-aware trim | ✅ Работает | macOS-only скиллы на Windows |
| Desktop-compatible aux config | ✅ Работает | Формат, понятный Hermes Desktop |
| GitHub Quality CI | ✅ Работает | Lint README и ссылок на push |
| Registry / Hub listing | 🟡 Планируется | Публикация в Hermes Ecosystem |
| Additional provider profiles | 🟡 Планируется | Больше примеров провайдеров |
| Clean-machine validation | 🟡 Планируется | Проверка установки с нуля |

## Быстрый старт

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

Загрузи в сессию:

```text
/skill hermes-optimizer
```

Или просто попроси:

```text
Help me optimize Hermes.
```

Skill должен сам обнаружить провайдеров и модели — без ручного списка от пользователя.

<details>
<summary>⚙️ Расширенные режимы установки</summary>

### Добавить как skill source

```bash
hermes skills tap add AzamatSafarov/hermes-optimizer
```

### Поделиться с коллегой

```bash
hermes skills install https://raw.githubusercontent.com/AzamatSafarov/hermes-optimizer/main/SKILL.md -y
```

### Публикация

См. [`PUBLISHING.md`](PUBLISHING.md) и [`RELEASE_COMMANDS.md`](RELEASE_COMMANDS.md).

</details>

## Пакет скилла

| Файл | Назначение |
|------|------------|
| `SKILL.md` | Runtime skill с workflow и правилами принятия решений |
| `references/provider-autodiscovery.md` | Правила определения реального provider stack перед изменениями |
| `references/platform-skill-trim.md` | Platform-aware disable для irrelevant built-in скиллов |
| `notes/current-routing.md` | Generic provider-примеры вместо личных дефолтов |
| `assets/hermes-optimizer-hero.svg` | Hero banner для GitHub |
| `assets/architecture-flow.svg` | Workflow diagram оптимизации |
| `llms.txt` / `llms-full.txt` | Машиночитаемое summary для LLM-assisted discovery |

## Lane-модель

| Lane | Типичные задачи | Профиль модели |
|------|-----------------|----------------|
| **Heavy** | `compression`, `web_extract`, `kanban_decomposer`, `curator` | Максимальный контекст, reasoning |
| **Medium** | `approval`, `triage_specifier`, `profile_describer` | Быстрый, дешёвый, стабильный |
| **Light** | `session_search`, `skills_hub`, `mcp`, `title_generation` | Минимальная цена, достаточно контекста |
| **Vision** | multimodal задачи | Только если verified live route |

## Что улучшает

| Область | Типичный эффект |
|---------|-----------------|
| **Auxiliary routing** | Некритичные задачи уходят с premium main-модели на более дешёвые verified routes |
| **Fallback chains** | Вскрывает фейковые fallback-цепочки (когда backup ведёт на ту же слабую модель) |
| **Guardrails** | Включает hard stops, checkpoints, safer approval modes |
| **Skill overhead** | Уменьшает prompt-bloat от лишних enabled скиллов |
| **Platform mismatch** | Отключает очевидно irrelevant скиллы (Apple/macOS defaults на Windows) |
| **Config hygiene** | Показывает deprecated ключи и version drift до поломки |

## Качество

- README/link lint workflow на каждый push
- Generic public примеры вместо личных provider defaults
- Структурированные SVG-ассеты для GitHub rendering
- Машиночитаемое project summary (`llms.txt`) для LLM-агентов

## Roadmap

- [x] Публичный GitHub-репозиторий
- [x] Provider-agnostic логика
- [x] Provider autodiscovery
- [x] Platform-aware skill trim
- [x] Hero + architecture visuals
- [x] LLM discovery файлы
- [x] README quality workflow
- [ ] Registry / Hub listing
- [ ] Дополнительные provider-профили
- [ ] Валидация установки с чистой машины

## Contributing

Fork → branch → улучшай refs / docs / workflow → PR.

Хорошие зоны для contribution:
- Более чёткие provider-профили
- Более безопасные skill-trim эвристики
- Лучшие fallback verification рецепты
- Больше coverage для public installation flows

## License

MIT. См. [LICENSE](LICENSE).

---

<div align="center">

**Собрал и систематизировал [Азамат Сафаров](https://github.com/AzamatSafarov)**

См. также:
- [creative-system](https://github.com/AzamatSafarov/creative-system) — творческая система автопостинга
- [shchedrovitsky-wiki](https://github.com/AzamatSafarov/shchedrovitsky-wiki) — архив работ Щедровицкого

</div>
