# Figma Variables — Limitations & Known Issues
<!-- repo: orfa-design/claude-skills | file: figma-variables/references/limitations.md -->
<!-- ⚠️ Цей файл старіє найшвидше. ЗАВЖДИ web search перед тим як стверджувати що щось "не існує" або підтверджувати ліміт. -->

---

## Plan Limits — Modes per collection
<!-- verified: 2026-06-13 | source: help.figma.com — Schema 2025 announcement (help.figma.com/hc/en-us/articles/35794667554839) -->
<!-- ⚠️ re-verify: ліміти змінювались кілька разів (було 4, потім 4/8/40, потім поточні значення) -->

| План | Modes per collection |
|------|---------------------|
| Starter/Free | 1 mode |
| Professional | 10 modes |
| Organization | 20 modes |
| Enterprise | 40 modes |

Актуальний список планів: `https://help.figma.com/hc/en-us/articles/360040328273`

**Змінних на колекцію:** до 5,000 (всі плани)
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

---

## Структурні обмеження

**Типи не конвертуються** — не можна змінити тип variable після створення. Color не може посилатись на Number.
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

**Gradient не підтримується як тип** — Variables зберігають тільки solid colors. Для градієнтів — хардкод або окремі color variables для кожного stop.
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

**Немає нативних масових операцій** — bulk rename, переміщення між колекціями, bulk delete через UI дуже повільно при 9k+ змінних. Потрібен плагін або REST API.
<!-- verified: 2026-06-13 | source: community knowledge + Figma forum -->

**Cross-file aliasing** — змінна може посилатись на змінну з іншої опублікованої бібліотеки, але тільки якщо та бібліотека підключена до файлу.
<!-- verified: 2026-06-13 | source: help.figma.com — Create and manage variables and collections -->

---

## UI — пошук та навігація
<!-- ⚠️ УВАГА: цей розділ потребує web search перед будь-якою відповіддю. UI оновлюється часто. -->

**Пошук по hex-коду у Variables panel**
<!-- ⚠️ re-verify: статус змінився у 2025-2026. Станом на 2025-02 був feature request. Станом на 2026-06 — потребує перевірки через web search. Не стверджуй "є" або "немає" без пошуку. -->

**Показ hex у variable picker (при призначенні змінної до елементу)**
<!-- ⚠️ re-verify: довго був feature request (2023-2025). Статус станом на 2026-06 невідомий — web search обов'язковий. -->

**Сортування змінних** — відображаються в порядку створення, нема нативного алфавітного сортування.
<!-- verified: 2026-06-13 | source: Figma forum community knowledge -->
<!-- ⚠️ re-verify: могло з'явитись -->

---

## Extended Collections — Known Issues

**Mode inheritance** — extended collection фіксує "Default" mode, що може вимагати ручного перемикання в instances компонентів.
<!-- verified: 2026-06-13 | source: Figma forum (forum.figma.com/share-your-feedback-26/extended-collections-doesn-t-respect-mode-hierarchy-48123) -->
<!-- ⚠️ re-verify: статус цього обмеження може змінитись -->

**Auto-update bug при cross-collection modes** — extended collection не оновлювала designs при зміні mode через додаткові колекції.
<!-- status: ВИПРАВЛЕНО у Figma v126.0.4, лютий 2026 | source: Figma forum (forum.figma.com/report-a-problem-6/variables-in-extended-collections-connected-to-other-collection-modes-are-not-updated-48714) -->

---

## Що НЕ вміють Variables (фундаментальні обмеження)
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables -->

- **Composite values** — повний typography style як один токен неможливий; треба окремі variables для family, size, weight тощо
- **Gradients** як single value
- **Conditional aliases** — "якщо mode X, посилайся на A, інакше на B"
- **Math між variables** як alias — тільки в prototyping expressions
- **Глобальні modes на весь файл** — mode застосовується до frame/section; але можна застосувати до кореневого frame

---

## Workarounds

**Потрібно більше modes ніж дозволяє план:**
- Плагін "Magic Variables" — unlimited modes (community, безкоштовний)
- Розбити на кілька колекцій

**Bulk rename/restructure 9k+ змінних:**
- Figma REST API Variables endpoint
- Плагіни: "Variables Organizer", "Batch Styler"

**Пошук змінної за hex або value:**
<!-- ⚠️ re-verify нативні можливості перед рекомендацією плагіну -->
- Перевір нативний пошук (міг з'явитись)
- Плагін "Find Variables", "Color Variable Matcher"

**Export для девів:**
- Плагін "Tokens Brücke" — W3C DTCG JSON + прямий push до GitHub
- Плагін "Token Exporter" — W3C + CSS custom properties
- Figma REST API Variables endpoint — для кастомних скриптів
<!-- verified: 2026-06-13 | source: figma.com/community -->
