# Figma Variables — Collection Architecture
<!-- repo: orfa-design/claude-skills | file: figma-variables/references/collection-architecture.md -->
<!-- last verified: 2026-06-13 | джерела: help.figma.com, bordercrossingux.com, designsystemscollective.com -->
<!-- Концептуальний файл — стабільніший за limitations.md, але паттерни можуть еволюціонувати -->

---

## Базова ієрархія токенів
<!-- verified: 2026-06-13 | source: help.figma.com/hc/en-us/articles/18490793776023 + industry standard -->

Три шари — стандарт індустрії:
Primitives  →  Semantic  →  Component

(raw values)   (decisions)   (specific usage)
### Primitives
Сирі значення без контексту. Однакові для всіх брендів і тем.
color/palette/blue/100  = #EFF6FF

color/palette/blue/500  = #3B82F6

color/palette/blue/900  = #1E3A8A
spacing/4   = 4

spacing/8   = 8

spacing/16  = 16
radius/sm   = 4

radius/md   = 8

radius/lg   = 16
Naming pattern: `category/name/scale` або `category/scale`

### Semantic
Описують **рішення**, не значення. Тут відбувається mode switching між темами.
color/surface/default           → primitive/palette/neutral/0     (light)

→ primitive/palette/neutral/900   (dark)
color/action/primary/default    → primitive/palette/blue/500

color/action/primary/hover      → primitive/palette/blue/600

color/action/primary/disabled   → primitive/palette/blue/200
spacing/component/padding/sm    → primitive/spacing/8

spacing/component/padding/md    → primitive/spacing/16
Naming pattern: `category/role/property/state`

### Component
Специфічні для компонентів. Не всі системи їх потребують — актуально для великих enterprise систем.
button/background/primary/default  → semantic/color/action/primary/default

button/padding/horizontal/md       → semantic/spacing/component/padding/md

input/border/default               → semantic/color/border/default
button/background/primary/default  → semantic/color/action/primary/default

button/padding/horizontal/md       → semantic/spacing/component/padding/md

input/border/default               → semantic/color/border/default
Naming pattern: `component/element/property/state`

---

## Multi-Brand Архітектура

### Підхід A: Modes per collection
<!-- verified: 2026-06-13 | source: help.figma.com — Modes for variables -->

Одна semantic колекція, кожен бренд = окремий mode.
Collection: "Semantic"

Modes: Light-BrandA | Dark-BrandA | Light-BrandB | Dark-BrandB | ...
**Проблема для BV360:** 10 брендів × 2 теми = 20 modes → потрібен Organization план мінімум.

### Підхід B: Extended Collections (рекомендований для BV360)
<!-- verified: 2026-06-13 | source: help.figma.com/hc/en-us/articles/36346281624471 -->
<!-- ⚠️ re-verify: plan availability для UI (не тільки API) -->
Core Collection (primitives + base semantic)

→ Brand A extends → overrides тільки brand-specific values

→ Brand B extends → overrides тільки brand-specific values

→ Brand C extends → overrides тільки brand-specific values

Переваги:
- Core DS оновлюється → всі бренди автоматично отримують зміни
- Кожен бренд контролює тільки своє
- Чистіша структура ніж 20+ modes

### Підхід C: Окремі файли на бренд
Legacy workaround. Не рекомендується — ламає governance і синхронізацію.

---

## Рекомендована структура колекцій для BV360
<!-- verified: 2026-06-13 | source: industry best practices + BV360 context (10 brands, light/dark, 9200+ variables) -->
Файл: BV360 Design System — Variables
Collection 1: "Primitives"          ← без modes, тільки raw values

├── color/palette/*

├── spacing/*

├── radius/*

├── typography/size/*

├── typography/weight/*

├── typography/family/*

└── opacity/*
Collection 2: "Semantic"            ← modes: Light | Dark

├── color/surface/*

├── color/content/*

├── color/border/*

├── color/action/*

├── color/feedback/*                 (success, error, warning, info)

├── spacing/layout/*

├── spacing/component/*

└── radius/component/*
Collection 3: "Brand"               ← modes: Brand01 | Brand02 | ... | Brand10

├── color/brand/primary

├── color/brand/secondary

├── color/brand/accent

└── typography/brand/family
Collection 4: "Content"             ← string variables, якщо потрібна локалізація

└── copy/*
**Логіка розподілу:** Semantic і Brand — окремі колекції, бо вони мають різні axis змін. Semantic перемикає тему (light/dark), Brand перемикає бренд. Можна комбінувати обидва mode switcher на одному frame.

---

## Naming Conventions
<!-- verified: 2026-06-13 | source: industry standard + W3C DTCG alignment -->

### Правила
- Всі **lowercase**
- Слова в межах одного сегменту через `-`: `brand-primary`
- Рівні ієрархії через `/`: `color/action/brand-primary/default`
- Ніколи не використовуй raw value у назві: `blue-500` ✅, `#3B82F6` ❌
- Описуй **роль**, не **вигляд**: `color/action/primary` ✅, `color/blue-button` ❌

### Паттерни

**Primitives:**
color/palette/blue/50

color/palette/blue/500

spacing/4

spacing/16

radius/md
**Semantic:**
color/surface/default

color/surface/subtle

color/content/primary

color/content/secondary

color/border/default

color/action/primary/default

color/action/primary/hover

color/action/primary/disabled

color/feedback/error/default

color/feedback/success/background
### CSS alignment
color/surface/default  →  --color-surface-default

spacing/16            →  --spacing-16

radius/md             →  --radius-md
Figma `/` → CSS `-` → JS camelCase. Автоматична конвертація через Style Dictionary.

---

## Anti-patterns
<!-- verified: 2026-06-13 | source: industry best practices -->

❌ **Все в одній колекції** — 9k змінних без структури = нечитабельно і важко підтримувати

❌ **Semantic і primitives разом** — ламає ієрархію aliasing, немає чіткого single source of truth

❌ **Mode per brand AND mode per theme в одній колекції** — швидко вичерпуєш mode limit

❌ **Hardcoded values у semantic** — семантичні змінні мають посилатись на primitives, не містити raw hex

❌ **Описова назва з кольором** — `orange-button-bg` застаріє коли колір зміниться

❌ **Непослідовні separators** — mix of `-`, `_`, `/`, camelCase в одній системі
