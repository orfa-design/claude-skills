# Figma Variables — Dev Handoff
<!-- repo: orfa-design/claude-skills | file: figma-variables/references/dev-handoff.md -->
<!-- last verified: 2026-06-13 | source: W3C DTCG spec, figma.com/community, developers.figma.com -->
<!-- BV360 context: Variables передаються як окремий файл на GitHub -->

---

## W3C Design Tokens Community Group (DTCG) формат
<!-- verified: 2026-06-13 | source: design-tokens.github.io/community-group/format/ -->
<!-- ⚠️ re-verify: spec активно розвивається, можливі зміни у $type values -->

Стандарт де-факто для передачі токенів між інструментами.

```json
{
  "color": {
    "surface": {
      "default": {
        "$value": "#FFFFFF",
        "$type": "color",
        "$description": "Default page background"
      },
      "subtle": {
        "$value": "{color.palette.neutral.50}",
        "$type": "color",
        "$description": "Subtle background for secondary content areas"
      }
    }
  },
  "spacing": {
    "16": {
      "$value": 16,
      "$type": "dimension"
    }
  }
}
```

Ключові поля:
- `$value` — значення або alias у `{dot.notation}`
- `$type` — `color`, `dimension`, `fontFamily`, `fontWeight`, `fontSize`, `lineHeight`, `letterSpacing`, `duration`, `cubicBezier`
- `$description` — документація токену (критично важливо для девів)

---

## Export плагіни
<!-- verified: 2026-06-13 | source: figma.com/community -->

### Tokens Brücke (рекомендований для BV360)
- W3C DTCG JSON + пряма інтеграція з GitHub (commit або PR)
- Зберігає aliases як `{dot.notation}` references
- Multi-mode → окремі файли на mode
- Є npm пакет для CLI використання без плагін UI
- Безкоштовний

### Token Exporter
- W3C DTCG JSON + CSS custom properties
- Multi-mode колекції → `[data-theme]` селектори автоматично
- Очищає float precision артефакти Figma
- Безкоштовний

---

## Figma REST API
<!-- verified: 2026-06-13 | source: developers.figma.com/docs/rest-api/changelog/ -->
<!-- ⚠️ re-verify: rate limits змінились листопад 2025 -->
GET /v1/files/{file_key}/variables/local
Повертає всі local variables: values per mode, alias references, scoping, extended collections.

Корисно для:
- Кастомний аудит скрипт (BV360 token audit tool)
- Автоматичний export по розкладу через CI/CD
- Валідація naming conventions

Потрібен: Personal Access Token або OAuth залежно від плану.

Rate limits (оновлено листопад 2025): актуальні ліміти на `developers.figma.com/docs/rest-api/rate-limits`

---

## Власний плагін (Figma Plugin API)
<!-- verified: 2026-06-13 | source: developers.figma.com/docs/plugins/api -->

```javascript
// Отримати всі колекції
const collections = await figma.variables.getLocalVariableCollectionsAsync();

// Отримати змінну за ID
const variable = await figma.variables.getVariableByIdAsync(id);

// Значення по modes
variable.valuesByMode  // { modeId: value }

// Alias reference
// value буде об'єктом { type: 'VARIABLE_ALIAS', id: '...' }
```

---

## Рекомендований GitHub pipeline для BV360
<!-- verified: 2026-06-13 | source: dev.to + figma community best practices -->
Дизайнер оновлює Variables у Figma

↓
Tokens Brücke плагін → Push to GitHub PR

↓
Dev reviewer переглядає diff → merge

↓
GitHub Action → Style Dictionary

↓
CSS / JS / інші output formats готові

### Структура файлів на GitHub
tokens/

├── primitives/

│   ├── colors.tokens.json

│   ├── spacing.tokens.json

│   └── typography.tokens.json

├── semantic/

│   ├── light.tokens.json

│   └── dark.tokens.json

├── brands/

│   ├── brand-01.tokens.json

│   ├── brand-02.tokens.json

│   └── ...

└── README.md

---

## Style Dictionary
<!-- verified: 2026-06-13 | source: styledictionary.com + industry practice -->

Трансформує W3C DTCG JSON у будь-який output:

```css
/* output: tokens.css */
:root {
  --color-action-primary: #3B82F6;
}
[data-theme="dark"] {
  --color-action-primary: #60A5FA;
}
```

```js
/* output: tokens.js */
export const colorActionPrimary = '#3B82F6';
```

---

## Naming alignment: Figma ↔ Code
<!-- verified: 2026-06-13 | source: industry standard -->

Узгодь з девами **до** початку рефакторингу:

| Figma Variable | CSS Custom Property | JS Token |
|----------------|---------------------|----------|
| `color/surface/default` | `--color-surface-default` | `colorSurfaceDefault` |
| `spacing/16` | `--spacing-16` | `spacing16` |
| `radius/md` | `--radius-md` | `radiusMd` |

Правило: Figma `/` → CSS `-` → JS camelCase. Автоматична конвертація через Style Dictionary.

---

## Що документувати для девів у кожному токені

Обов'язково:
- Назва (відповідає CSS custom property)
- Значення або alias chain до resolved value
- `$type`
- `$description` — коли використовувати

Добре мати:
- `deprecated: true` якщо токен виводиться з використання
- `replacedBy` — якщо є заміна
- Usage examples — де саме у компонентах використовується

