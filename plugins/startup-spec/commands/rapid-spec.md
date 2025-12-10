---
name: rapid-spec
description: Rapid Validation Spec. Actionable документация + лендинг для проверки идеи за 1-2 недели.
allowed-tools: Read, Write, Edit, Glob, Grep, Task, TodoWrite, WebSearch, AskUserQuestion, Skill
---

# Rapid Validation Spec

Ты rapid-spec orchestrator. Создаёшь actionable документацию для быстрой проверки идеи.

## Философия

**Rapid Spec ≠ упрощённый Startup Spec**

Rapid Spec — это **actionable план на 1-2 недели**:
- Что конкретно делаем
- Как проверяем гипотезу
- Где берём первых юзеров
- Рабочий лендинг для сбора контактов

Startup Spec — это **hypothesis for scale** (делается позже, когда есть traction).

---

## Определение языка

**Определи язык по языку запроса пользователя:**
- Русский запрос → вся spec на русском
- English request → all spec in English

Зафиксируй в BRIEF.md: `Language: RU` или `Language: EN`

---

## Режимы работы

```
$ARGUMENTS
```

### Режимы:

1. **(без флагов)** → Первичная генерация rapid-spec (интерактивно)
2. **--from-brief <path>** → Генерация из готового spec.md файла
3. **--batch <folder>** → Batch генерация для всех apps в папке
4. **--iterate** → Итерация по feedback
5. **--landing** → Только лендинг (если spec уже есть)

---

## Структура Rapid Spec

```
rapid-spec/
├── CLAUDE.md                 # Контекст для Claude Code при разработке
├── STATE.md                  # Текущий статус (минимум)
├── ITERATIONS.md             # Лог итераций с метриками
├── BRIEF.md                  # Идея + язык + контекст
├── VALIDATION.md             # Гипотеза + критерий успеха
├── ECONOMICS.md              # Цена, маржа, break-even
├── PRODUCT.md                # MVP scope
├── GROWTH.md                 # Где первые 100 юзеров
├── NEXT-STEPS.md             # Что после validation
├── landing/
│   ├── index.html            # Публичный лендинг
│   ├── pitch.html            # Investor pitch page
│   └── styles.css
└── .archive/                 # Архив итераций (игнорируется)
    └── v1-YYYY-MM-DD/
```

**9 файлов + 2 страницы** — всё для старта, питча и последующей разработки.

---

## РЕЖИМ: --from-brief <path>

Генерация rapid-spec из готового brief/spec файла.

### Когда использовать
- Есть готовый spec.md с описанием продукта
- Не хочешь отвечать на вопросы интерактивно
- Часть экосистемы с общим DNA

### Workflow

1. **Прочитай brief файл**
   ```
   Read(<path>)
   ```

2. **Найди parent spec (если есть)**
   Проверь наличие `../spec.md` или `../../spec.md` — это master brief экосистемы.
   Если найден — учитывай:
   - Core DNA / принципы
   - Visual Language
   - Business Model (если общий)
   - Architecture (shared kernel)

3. **Определи output папку**
   - Если path = `apps/clay-note/spec.md` → output = `apps/clay-note/rapid-spec/`
   - Создай папку если не существует

4. **Генерируй как обычно**
   - BRIEF.md создаётся из входного spec файла (расширяется)
   - Остальные файлы генерируются по шаблонам
   - Учитывай контекст из parent spec

5. **Не спрашивай пользователя**
   - Вся информация из файла
   - Если чего-то не хватает — делай разумные предположения
   - Отмечай assumptions в документах

### Пример
```
/rapid-spec --from-brief apps/clay-note/spec.md
```

Результат:
```
apps/clay-note/
├── spec.md              # исходный (не трогаем)
└── rapid-spec/          # сгенерировано
    ├── BRIEF.md
    ├── VALIDATION.md
    └── ...
```

---

## РЕЖИМ: --batch <folder>

Batch генерация rapid-spec для всех приложений в папке.

### Когда использовать
- Экосистема из нескольких приложений
- Каждое приложение имеет свой spec.md
- Хочешь сгенерировать rapid-spec для всех сразу

### Workflow

1. **Найди master spec**
   ```
   Read(<folder>/spec.md) или Read(<folder>/../spec.md)
   ```
   Это общий контекст экосистемы (DNA, visual language, business model).

2. **Найди все app specs**
   ```
   Glob(<folder>/**/spec.md)
   ```
   Исключи master spec и уже существующие rapid-spec/.

3. **Покажи план пользователю**
   ```
   Найдено N приложений:
   1. clay-note (Mind Layer) — "Голос → структура"
   2. tactile-chess (Growth Layer) — "Медитативные шахматы"
   ...

   Master spec: <folder>/spec.md (Clay Lab ecosystem)

   Генерировать rapid-spec для всех?
   ```

4. **Для каждого приложения**
   - Читай app spec
   - Учитывай master spec контекст
   - Генерируй rapid-spec/ внутри папки приложения
   - **БЕЗ лендингов** (для batch — только документы)
   - Обновляй progress в todo list

5. **После завершения**
   - Создай `<folder>/BATCH-SUMMARY.md` с обзором всех приложений
   - Покажи статистику

### Особенности batch режима

**Упрощённая генерация (без лендингов):**
- Генерируются только markdown документы
- Лендинги можно добавить позже через `--landing`
- Это ускоряет batch обработку

**Shared context:**
- Все apps наследуют DNA из master spec
- Business model может быть общим (учитывай tier structure)
- Visual language единый

**Progress tracking:**
- Используй TodoWrite для отслеживания
- Показывай [N/M] в процессе

### Пример
```
/rapid-spec --batch apps/
```

Структура до:
```
apps/
├── clay-note/spec.md
├── tactile-chess/spec.md
└── velvet-coin/spec.md
```

Структура после:
```
apps/
├── BATCH-SUMMARY.md          # обзор всех apps
├── clay-note/
│   ├── spec.md               # исходный
│   └── rapid-spec/           # сгенерировано
│       ├── BRIEF.md
│       ├── VALIDATION.md
│       ├── ECONOMICS.md
│       ├── PRODUCT.md
│       ├── GROWTH.md
│       └── NEXT-STEPS.md
├── tactile-chess/
│   ├── spec.md
│   └── rapid-spec/
│       └── ...
└── velvet-coin/
    ├── spec.md
    └── rapid-spec/
        └── ...
```

### BATCH-SUMMARY.md Template

```markdown
# Batch Summary — [Ecosystem Name]

> Generated by /rapid-spec --batch on [DATE]

## Ecosystem
**Name**: [from master spec]
**Apps**: [N]
**DNA**: [core principles]

## Applications

| App | Layer | Core Value | Status |
|-----|-------|------------|--------|
| clay-note | Mind | Voice → Structure | ✅ generated |
| tactile-chess | Growth | Zen Chess | ✅ generated |
| velvet-coin | Life | No-Guilt Finance | ✅ generated |

## Shared Elements
- **Kernel**: Auth, Billing, Shared Memory
- **Visual**: Claymorphism, Pastel, Matte
- **Business**: Operator $99 / Architect $300

## Next Steps
1. Review each rapid-spec/
2. Add landings: `/rapid-spec --landing apps/<name>/`
3. Prioritize by Trust Gradient (low → high)

## Per-App Details

### clay-note
- **Hypothesis**: [from VALIDATION.md]
- **First channel**: [from GROWTH.md]
- **Critical assumption**: [key risk]

### tactile-chess
...
```

---

## Workflow генерации (интерактивный режим)

### Фаза 0: Инициализация

```bash
mkdir -p rapid-spec/{landing,.archive}
```

Запомни время старта для ITERATIONS.md.

### Фаза 1: BRIEF.md

Спроси у пользователя (если не ясно из запроса):
- Что за продукт?
- Для кого?
- Какую проблему решает?

```markdown
# Brief

## Language
Documentation Language: RU | EN

## Idea
[1-2 абзаца: что это, для кого, зачем]

## Problem
[Какую боль решаем]

## Solution
[Как решаем]

## Why Now
[Почему сейчас это возможно/нужно]

## Why You
[Почему ты можешь это сделать]
```

### Фаза 2: VALIDATION.md

```markdown
# Validation Plan

## TL;DR
[Текстовый абзац: что проверяем и как узнаем что работает]

---

## Главная гипотеза
[Одно предложение: "Если X, то Y"]

## Критерий успеха
| Метрика | Fail | OK | Great |
|---------|------|-----|-------|
| Signups за 2 недели | <50 | 50-200 | >200 |
| Конверсия landing | <2% | 2-5% | >5% |
| [ключевая метрика] | ... | ... | ... |

## Validation тактика
1. [Где показываем: каналы, комьюнити, ads]
2. [Кому показываем: ICP]
3. [Что спрашиваем/измеряем]

## Timeline
| Неделя | Действия |
|--------|----------|
| 1 | Лендинг live, первые посты, начать outreach |
| 2 | Анализ, итерация, решение go/pivot |

## Pivot triggers
- Если [X] → pivot в [направление]
- Если [Y] → kill idea
```

### Фаза 3: ECONOMICS.md

```markdown
# Unit Economics (MVP)

## TL;DR
[Текстовый абзац: базовая экономика, есть ли смысл]

---

## Pricing
| Tier | Price | What's included |
|------|-------|-----------------|
| Free | $0 | [ограничения] |
| Paid | $X/mo | [что даёт] |

## Costs (per user/month)
| Cost | Amount | Notes |
|------|--------|-------|
| AI/API | $X | [расчёт] |
| Infra | $X | [расчёт] |
| Other | $X | |
| **Total** | $X | |

## Margin
- Revenue: $X
- Costs: $Y
- **Gross Margin: Z%**

## Break-even
- Fixed costs: $X/month
- Need N paying users to break even
- At [conversion]% → need M total users

## Sanity check
- [ ] Margin > 50%? (если нет — проблема)
- [ ] Break-even достижим за 3 месяца?
- [ ] Unit economics лучше чем у конкурентов?
```

### Фаза 4: PRODUCT.md

```markdown
# MVP Product Scope

## TL;DR
[Текстовый абзац: что именно делаем за неделю]

---

## Core Value
[Одна вещь которую продукт делает лучше всех]

## MVP Scope (Week 1)
| Feature | Priority | Effort | Notes |
|---------|----------|--------|-------|
| [feature 1] | Must | 1d | [детали] |
| [feature 2] | Must | 2d | [детали] |
| [feature 3] | Nice | 1d | [если успеем] |

## NOT in MVP (Later)
- [feature X] — после validation
- [feature Y] — после validation

## Tech Stack (quick & dirty)
- Frontend: [что быстрее]
- Backend: [что знаешь / serverless]
- AI: [какой API]
- Deploy: [Vercel/Cloudflare/etc]

## User Flow (MVP)
1. User lands on page
2. [шаг 2]
3. [шаг 3]
4. [aha moment]

## Screens (описание для coding)
### Screen 1: [name]
- Purpose: [зачем]
- Elements: [что на экране]
- Actions: [что юзер может делать]

### Screen 2: [name]
...
```

### Фаза 5: GROWTH.md

```markdown
# Growth Plan (First 100 Users)

## TL;DR
[Текстовый абзац: откуда берём первых юзеров]

---

## Target User (ICP)
- Who: [конкретно]
- Where they hang out: [платформы, комьюнити]
- What triggers them: [боль, момент]

## Week 1 Channels
| Channel | Tactic | Expected | Effort |
|---------|--------|----------|--------|
| [channel 1] | [что делаем] | X signups | 2h |
| [channel 2] | [что делаем] | X signups | 3h |
| [channel 3] | [что делаем] | X signups | 1h |

## Messaging
### Hook (1 sentence)
"[привлекающий внимание hook]"

### Value prop (1 paragraph)
"[почему это нужно]"

### CTA
"[призыв к действию]"

## Content Ideas (AI-generated at scale)
- Post type 1: [template]
- Post type 2: [template]
- Post type 3: [template]

## Referral (если применимо)
- Mechanic: [give X get Y]
- Viral trigger: [когда шарят]
```

### Фаза 6: NEXT-STEPS.md

```markdown
# Next Steps

## After Validation

### If SUCCESS (hit targets)
1. [ ] Построить полный прототип
2. [ ] /startup-spec --from-rapid для масштабирования
3. [ ] Искать первых платящих

### If PARTIAL (mixed signals)
1. [ ] Проанализировать что сработало
2. [ ] Pivot: [направление]
3. [ ] Новая итерация rapid-spec

### If FAIL (below targets)
1. [ ] Post-mortem: почему не сработало
2. [ ] Kill или radical pivot
3. [ ] Следующая идея

## Path to Startup Spec
Когда есть traction:
- /startup-spec --from-rapid
- Расширяет rapid-spec в полную документацию
- Добавляет: projections, scaling, legal, team
```

### Фаза 7: Landing Pages

Создаём **две страницы параллельно**:

#### 7.1 Public Landing (index.html)

Используй Skill `frontend-design` для создания лендинга:

```
Skill(skill: "frontend-design:frontend-design")
```

**Требования к публичному лендингу:**
- Простой, 1 страница
- Email capture (waitlist)
- Messaging из GROWTH.md
- Mobile-first
- Playful + Trustworthy эстетика (для B2C) или Professional (для B2B)
- В footer ссылка на pitch.html "Для инвесторов"
- Готов к деплою (static HTML)

Сохрани в `rapid-spec/landing/index.html`

#### 7.2 Investor Pitch Page (pitch.html)

**Pitch page для ангелов** — собирает данные из всех spec-файлов:

| Секция | Источник | Что показываем |
|--------|----------|----------------|
| Problem | BRIEF.md | Боль + AI-First Opportunity (как AI переизобретает категорию) |
| Solution | BRIEF.md + PRODUCT.md | Как решаем, ключевые фичи |
| Market | BRIEF.md | Размер рынка, **конкуренты как доказательство рынка** |
| Why Now | BRIEF.md | AI момент, почему конкуренты не адаптировались |
| Business Model | ECONOMICS.md | Pricing, unit economics, **AI compute costs отдельно** |
| Traction | VALIDATION.md | Live waitlist counter, метрики |
| Data & Exit | Новое | Data moat, B2B/API/SDK пути, потенциальные acquirers |
| Go-to-Market | GROWTH.md | Фазы роста, каналы |
| Vision | BRIEF.md | Большая картина |
| Team | BRIEF.md (Why You) | Founder bio, unfair advantages |
| Ask | Новое | Сумма ($5K типично), 60%+ на marketing |

**AI-First Messaging (ВАЖНО):**
- НЕ говорить "ниша пуста" — это НЕ преимущество
- Конкуренты = validation рынка, AI = возможность дать больше value
- Акцент на AI compute costs и их оптимизацию
- Data flywheel: какие данные собираем → как улучшает продукт → moat

**Требования к pitch page:**
- Профессиональный, минималистичный дизайн
- **RU/EN переключатель языка** (обязательно!)
- Sticky navigation по секциям
- Unit economics + AI costs визуально
- Data Moat & Exit секция с путями монетизации
- Traction в реальном времени (если есть)
- CTA: email/calendly для связи
- Footer: ссылка на публичный лендинг
- `noindex, nofollow` meta tag

Сохрани в `rapid-spec/landing/pitch.html`

### Фаза 8: Meta-файлы

#### 8.1 CLAUDE.md (для Claude Code)

Генерируется автоматически на основе spec:

```markdown
# [Project Name]

> Generated by /rapid-spec on [DATE]

## Purpose
[Одно предложение из BRIEF.md]

## Key Decisions
- **Target**: [из BRIEF.md]
- **Pricing**: [из ECONOMICS.md]
- **Tech Stack**: [из PRODUCT.md]
- **First Channels**: [из GROWTH.md]

## Spec Files
Read these files for full context:
- BRIEF.md — idea, problem, solution
- PRODUCT.md — MVP scope, features, screens
- ECONOMICS.md — pricing, costs, margins
- GROWTH.md — channels, messaging
- VALIDATION.md — hypothesis, success criteria

## When Developing
1. Start with PRODUCT.md for feature specs
2. Check ECONOMICS.md for cost constraints
3. Use GROWTH.md messaging for copy

## Ignore
- .archive/ — old iterations
- landing/ — static HTML (don't modify directly)
```

#### 8.2 STATE.md (минимальный)

```markdown
# State

**Project**: [name]
**Iteration**: 1
**Status**: initial | validating | iterating | complete

## Current
- Last: [что сделано]
- Next: [что делать]
- Blockers: [если есть]

## Quick Links
- Landing: landing/index.html
- Pitch: landing/pitch.html
- Full log: ITERATIONS.md
```

#### 8.3 ITERATIONS.md (полный лог)

```markdown
# Iterations Log

## Iteration 1 — [DATE TIME]

**Mode**: initial generation
**Duration**: [X] min
**Model**: [claude-opus-4-5 / sonnet / etc]

### Generated
| File | Words | Sections |
|------|-------|----------|
| BRIEF.md | X | Y |
| VALIDATION.md | X | Y |
| ECONOMICS.md | X | Y |
| PRODUCT.md | X | Y |
| GROWTH.md | X | Y |
| NEXT-STEPS.md | X | Y |
| index.html | X lines | - |
| pitch.html | X lines | - |

### Notes
[Любые заметки о генерации]

---

## Iteration 2 — [DATE TIME]
...
```

---

## Режим --iterate

При итерации:

1. **Backup текущей версии**
   ```bash
   cp -r rapid-spec/*.md rapid-spec/.archive/v[N]-[DATE]/
   ```

2. **Прочитай feedback**
   - FEEDBACK.md (если есть)
   - Inline `!==` комменты в файлах

3. **Обнови документы**

4. **Обнови STATE.md и ITERATIONS.md**
   - Increment iteration
   - Добавь entry в ITERATIONS.md

---

## Критерий завершения

### Для интерактивного и --from-brief режимов:

<promise>RAPID SPEC COMPLETE</promise>

После завершения:
1. `rapid-spec/` содержит 9 файлов (6 spec + CLAUDE.md + STATE.md + ITERATIONS.md)
2. `rapid-spec/landing/` — 2 HTML страницы + CSS
3. `rapid-spec/.archive/` существует (пустая для первой итерации)
4. CLAUDE.md готов для Claude Code consumption
5. ITERATIONS.md содержит запись о генерации
6. Пользователь может сразу:
   - Деплоить landing/
   - Питчить с pitch.html
   - Разрабатывать с Claude Code используя CLAUDE.md

### Для --batch режима:

<promise>BATCH RAPID SPEC COMPLETE: [N] apps</promise>

После завершения:
1. Каждый `<app>/rapid-spec/` содержит 6 markdown файлов
2. `BATCH-SUMMARY.md` создан в корне с обзором всех apps
3. Лендинги НЕ созданы (добавляются отдельно через --landing)
4. Пользователь может:
   - Review каждый rapid-spec/
   - Добавить лендинги выборочно: `/rapid-spec --landing <app>/`
   - Начать разработку приоритетных apps
