# Figma Variables — Capabilities
<!-- repo: orfa-design/claude-skills | file: figma-variables/references/capabilities.md -->
<!-- last verified: 2026-06-13 | source: help.figma.com/hc/en-us/articles/14506821864087 -->

---

## Типи змінних
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

| Тип | Значення | Застосовується до |
|-----|----------|-------------------|
| **Color** | Solid hex/rgba | Fill, stroke, shadow, gradient stops, color styles, інші color variables |
| **Number** | Числа до сотих (напр. 12.75) | Corner radius, dimensions (min/max width/height), font size, weight, line height, letter spacing, paragraph indent/spacing, padding, gap, stroke weight, shadow X/Y/blur/spread, opacity, layout grid |
| **String** | Текстові рядки | Font family, font style/weight (назва, напр. "Bold"), text content, variant instances у prototyping |
| **Boolean** | true / false | Layer visibility, variant properties з true/false |

**Нюанси Number:**
- Letter spacing інтерпретується як `px`, не `%`
- Opacity > 100 автоматично обрізається до 100
- Підтримуються цілі та десяткові до сотих (напр. `12.75`)
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

---

## Collections та Groups
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

- **Collection** = набір змінних + їх modes
- **Group** = підрозділ всередині колекції через `/` у назві: `color/brand/primary`
- Ліміт: до **5,000 змінних** на одну колекцію
- Кількість modes залежить від плану → див. `limitations.md`

---

## Modes
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

Кожна змінна може мати різне значення залежно від активного mode.

Типові use cases:
- Light / Dark теми
- Brand A / Brand B / Brand C
- Mobile / Desktop (spacing, typography)
- Language variants (copy variables)

Mode застосовується на рівні **frame або section** — всі елементи всередині успадковують активний mode колекції.

---

## Aliasing (Variable References)
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

Змінна може посилатись на іншу змінну того ж типу. Основа токен-ієрархії:
primitive/blue/500 = #3B82F6

semantic/color/action/primary → primitive/blue/500

component/button/background → semantic/color/action/primary

При зміні `primitive/blue/500` — автоматично оновлюється все дерево.

**Обмеження:** можна посилатись тільки на змінну **того ж типу** (color → color, number → number).
<!-- verified: 2026-06-13 | source: help.figma.com — Overview of variables, collections, and modes -->

---

## Scoping
<!-- verified: 2026-06-13 | source: help.figma.com — Create and manage variables and collections -->

Обмежує де змінна з'являється як опція при призначенні:
- Number зі scope "corner radius" — не показується в dropdown для font size
- Зменшує cognitive load при роботі з великою кількістю змінних

Доступно для: **Color, Number, String** variables.

---

## Extended Collections
<!-- verified: 2026-06-13 | source: help.figma.com — Schema 2025 announcement + Extend a variable collection -->
<!-- ⚠️ re-verify: plan availability може змінитись -->

Дозволяє бренду "розширити" базову колекцію, перевизначаючи тільки потрібні значення.
Core DS (publisher)

→ Brand A (extends, overrides тільки brand colors)

→ Brand B (extends, overrides тільки brand colors)
- Extended collection успадковує всі змінні та modes від parent
- Бренд override тільки конкретні значення, решта синхронізується автоматично
- При оновленні Core DS — всі extended collections отримують зміни
- **⚠️ Plan limit:** Enterprise only через REST API — перевір актуальний стан UI доступності

---

## Variables у Dev Mode
<!-- verified: 2026-06-13 | source: help.figma.com/hc/en-us/articles/27882809912471 -->

Dev Mode показує при інспекції елемента:
- Назву змінної (token name)
- Resolved value (фінальний hex/число)
- Alias chain якщо є

---

## Prototyping з Variables
<!-- verified: 2026-06-13 | source: help.figma.com — Use variables in prototypes -->

- `Set variable` action — змінює значення під час взаємодії
- Expressions — математика з number variables, конкатенація strings
- Conditionals — if/else логіка на основі значень змінних
- String variables → перемикання variant instances компонентів
