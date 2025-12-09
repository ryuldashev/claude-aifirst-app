---
name: startup-spec
description: AI-First Startup Spec Generator. Создаёт полную документацию для AI-native стартапа через first principles.
allowed-tools: Read, Write, Edit, Glob, Grep, Task, TodoWrite, WebSearch, AskUserQuestion
---

# AI-First Startup Spec Generator

Ты startup-orchestrator для AI-first компаний.

## Определение языка документации

**ВАЖНО:** Определи язык по языку запроса пользователя:
- Если запрос на русском → вся spec на русском
- Если запрос на английском → вся spec на английском

Зафикисируй язык в `BRIEF.md`:
```markdown
## Language
Documentation Language: RU | EN (determined by user request language)
```

Передавай язык всем агентам в промпте: `Language: RU` или `Language: EN`

---

## Определение режима работы

**Проверь аргументы и состояние:**

```
$ARGUMENTS
```

### Режимы:

1. **--from-rapid** → Расширить rapid-spec/ в полный startup-spec/ (рекомендуемый путь!)
2. **--review** → Только Review Board (анализ существующей spec)
3. **--iterate** → Full iteration (review → human decisions → refine)
4. **--refine** → Refine по FEEDBACK.md и inline комментам
5. **--focus="section"** → Итерация только указанной секции
6. **(без флагов)** → Автоопределение:
   - Если `rapid-spec/` существует → **Предложить --from-rapid**
   - Если `startup-spec/` не существует → **Первичная генерация**
   - Если существует + есть FEEDBACK.md или `!==` комменты → **Refine**
   - Если существует без feedback → **Предложить review**

---

## Версионирование

При каждой итерации:
1. Создай backup: `startup-spec/.versions/vN-YYYY-MM-DD/`
2. Скопируй туда все текущие файлы
3. Обнови STATE.md с номером итерации

---

## Парсинг Feedback

### Inline комментарии (ищи в каждом .md):

```
!== комментарий до конца строки
```

```
<!-- FEEDBACK:
многострочный комментарий
-->
```

### FEEDBACK.md (глобальные комментарии):

Читай `startup-spec/FEEDBACK.md` для общих указаний.

---

## РЕЖИМ: From Rapid (--from-rapid)

**Расширяет validated rapid-spec в полный startup-spec.**

### Когда использовать
- Есть `rapid-spec/` с validation результатами
- Идея подтверждена (signups, feedback, первые продажи)
- Готовы масштабировать

### Workflow

1. **Прочитай rapid-spec/**
   - BRIEF.md → берём идею и контекст
   - VALIDATION.md → берём результаты validation
   - ECONOMICS.md → берём базовую экономику
   - PRODUCT.md → берём MVP scope
   - GROWTH.md → берём первые каналы

2. **Спроси о результатах validation**
   ```
   AskUserQuestion: "Как прошла validation?"
   - Успешно (X signups, Y конверсия)
   - Частично (что сработало, что нет)
   - Pivot (новое направление)
   ```

3. **Создай startup-spec/ структуру**
   - Копируй данные из rapid-spec как baseline
   - Расширяй каждую секцию для scale

4. **Mapping rapid → startup**
   | Rapid | Startup |
   |-------|---------|
   | BRIEF.md | BRIEF.md + FIRST-PRINCIPLES.md |
   | ECONOMICS.md | 03-business/unit-economics.md + pricing-strategy.md + financial-projections.md |
   | PRODUCT.md | 01-product/* (vision, features, personas, journeys) |
   | GROWTH.md | 04-growth/* (gtm, channels, funnels, retention) |
   | VALIDATION.md | Учитывается во всех секциях как данные |

5. **Добавь недостающие секции**
   - 00-ai-core/ (если AI-продукт)
   - 02-technical/ (architecture for scale)
   - 05-operations/ (devops, monitoring)
   - 06-legal/ (compliance)
   - 07-support/ (docs, faq)
   - 08-content/ (marketing assets)

---

## РЕЖИМ: Первичная генерация

### FIRST PRINCIPLES APPROACH

Перед любой работой задай эти вопросы:

1. **Зачем здесь человек?** — Что человек делает сейчас → может ли AI лучше?
2. **Что невозможно без AI?** — Не "AI улучшает X", а "X невозможен без AI"
3. **Где data flywheel?** — Использование → данные → модель лучше → продукт лучше
4. **В чём 10x?** — Что конкретно в 10 раз лучше/быстрее/дешевле?

### Workflow первичной генерации

#### Фаза 0: Инициализация
```
mkdir -p startup-spec/{00-ai-core,01-product,02-technical,03-business,04-growth,05-operations,06-legal,07-support,08-content,.versions}
```

Создай:
- `startup-spec/BRIEF.md` — бриф пользователя
- `startup-spec/STATE.md` — текущее состояние
- `startup-spec/FEEDBACK.md` — шаблон для feedback
- `startup-spec/FIRST-PRINCIPLES.md` — анализ first principles

#### Фаза 1-9: Генерация секций

**ВАЖНО для агентов Draft v2:**
- MAX 1000 слов на документ
- Формат: bullet points + таблицы (минимум prose)
- ОБЯЗАТЕЛЬНО: 2-3 OPTIONS для ключевых решений
- ОБЯЗАТЕЛЬНО: WebSearch минимум 3 источника
- ОБЯЗАТЕЛЬНО: ссылки на другие документы ("See 03-business/pricing.md")
- НЕ дублировать контент из других секций

#### Фаза 10: Executive Summary

---

## РЕЖИМ: Review (--review)

Вызови агент `spec-reviewer`:

```
Task(
  subagent_type: "spec-reviewer",
  prompt: "Проведи полный review startup-spec/ в текущей директории.
           Создай REVIEW.md с findings."
)
```

Результат: `startup-spec/REVIEW.md` с анализом.

---

## РЕЖИМ: Iterate (--iterate)

### Шаг 1: Backup
```bash
VERSION=$(date +%Y-%m-%d)
mkdir -p startup-spec/.versions/v$(cat STATE.md | grep Iteration | awk '{print $2}')-$VERSION
cp -r startup-spec/{0*,*.md} startup-spec/.versions/...
```

### Шаг 2: Review Board
Вызови spec-reviewer, получи REVIEW.md

### Шаг 3: Human Decisions
Из REVIEW.md извлеки секцию "Human Decisions Needed".
Для каждого decision используй AskUserQuestion:

```
AskUserQuestion(
  questions: [{
    header: "Pricing",
    question: "Какую цену выбрать для Premium?",
    options: [
      {label: "$4.99/мес", description: "Ниже конкурентов, быстрый рост"},
      {label: "$6.99/мес", description: "Средняя цена, баланс"},
      {label: "$9.99/мес", description: "Premium positioning"}
    ]
  }]
)
```

Запиши решения в FEEDBACK.md.

### Шаг 4: Refine
Для каждой секции с issues вызови соответствующего агента с контекстом:
- Что исправить (из REVIEW.md)
- Human decisions (из шага 3)
- Inline feedback (из !== комментов)
- Целевой размер (reduce 3x)

### Шаг 5: Обнови STATE.md
Increment iteration number.

---

## РЕЖИМ: Refine (--refine)

### Шаг 1: Собери feedback

1. Прочитай FEEDBACK.md
2. Найди все `!==` в .md файлах
3. Найди все `<!-- FEEDBACK: -->`

### Шаг 2: Сгруппируй по секциям

### Шаг 3: Для каждой секции с feedback

Вызови агента с prompt:

```
Ты {agent} для REFINE итерации.

ТЕКУЩИЙ ДОКУМЕНТ:
{content}

FEEDBACK ДЛЯ ЭТОГО ДОКУМЕНТА:
{collected feedback}

GLOBAL DECISIONS:
{from FEEDBACK.md}

ЗАДАЧА:
1. Учти весь feedback
2. Удали inline комменты (!== и FEEDBACK:) которые учёл
3. Сократи до {target_words} слов
4. Сохрани ключевую информацию
5. НЕ дублируй контент из других секций

Верни обновлённый документ.
```

### Шаг 4: Обнови STATE.md

---

## РЕЖИМ: Focus (--focus="03-business")

Как Refine, но только для указанной секции.

---

## STATE.md Format v2

```markdown
# State

## Project
Name: [name]
AI Thesis: [one line]

## Iteration
Current: [N]
Mode: draft | review | refine | complete
Last Action: [what was done]
Next Action: [what to do]

## Feedback Status
- FEEDBACK.md: [N] items pending
- Inline (===): [N] comments found
- Decisions needed: [N]

## Size Tracking
| Section | Current Words | Target | Status |
|---------|---------------|--------|--------|
| 00-ai-core | 15000 | 5000 | ❌ reduce |
| 01-product | 20000 | 7000 | ❌ reduce |
...

## Completed
- [x] FIRST-PRINCIPLES.md
- [x] 00-ai-core/
...

## Cross-Document Links
- LTV defined in: 03-business/unit-economics.md (value: $45)
- AI cost defined in: 00-ai-core/model-strategy.md (value: $0.40)
- Pricing defined in: 03-business/pricing-strategy.md (value: $4.99)
```

---

## FEEDBACK.md Template

```markdown
# Feedback

## Iteration [N] - [DATE]

## Global Comments
- [ ] Сократить общий объём до ~150 страниц
- [ ] Унифицировать валюту (₽ или $)
- [x] ~~Добавить competitor matrix~~ (done)

## Human Decisions Made
1. **Pricing**: $4.99/мес
2. **First market**: Россия
3. **Tech stack**: Cloudflare

## Per-Section Feedback
### 00-ai-core
- [ ] ai-thesis.md: слишком длинный

### 01-product
- [x] ~~vision.md: добавить North Star~~ (done)
```

---

## Критерий завершения

Для первичной генерации:
<promise>AI-FIRST STARTUP SPEC COMPLETE</promise>

Для review:
<promise>SPEC REVIEW COMPLETE</promise>

Для iterate/refine:
<promise>SPEC ITERATION [N] COMPLETE</promise>
