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

1. **(без флагов)** → Первичная генерация rapid-spec
2. **--iterate** → Итерация по feedback
3. **--landing** → Только лендинг (если spec уже есть)

---

## Структура Rapid Spec

```
rapid-spec/
├── BRIEF.md              # Идея + язык + контекст
├── VALIDATION.md         # Гипотеза + критерий успеха
├── ECONOMICS.md          # Цена, маржа, break-even (1 страница)
├── PRODUCT.md            # MVP scope (что кодим за неделю)
├── GROWTH.md             # Где первые 100 юзеров
├── NEXT-STEPS.md         # Что после validation
└── landing/              # Готовые HTML страницы
    ├── index.html        # Публичный лендинг (для юзеров)
    ├── pitch.html        # Investor pitch page (для ангелов)
    └── styles.css
```

**6 документов + 2 страницы** — всё что нужно для старта и питча.

---

## Workflow генерации

### Фаза 0: Инициализация

```bash
mkdir -p rapid-spec/landing
```

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

---

## Критерий завершения

<promise>RAPID SPEC COMPLETE</promise>

После завершения:
1. `rapid-spec/` содержит 6 документов
2. `rapid-spec/landing/index.html` — публичный лендинг для юзеров
3. `rapid-spec/landing/pitch.html` — investor pitch page для ангелов
4. Страницы взаимно связаны (footer links)
5. Пользователь может сразу деплоить и начинать validation + питчить
