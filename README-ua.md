# Налаштування ESLint для сортування імпортів у проектах Expo

Цей репозиторій містить готову конфігурацію ESLint, спеціально розроблену для автоматичного впорядкування та сортування імпортів у проєктах React Native з використанням Expo.

## Проблема

Розробка може бути хаотичною, і безладна організація імпортів у файлах `.js` та `.tsx` є однією з найпоширеніших проблем. Це не тільки ускладнює читання коду, але й ускладнює співпрацю в команді, оскільки кожен розробник може мати свій власний стиль форматування.

**Типовий приклад неупорядкованих імпортів:**
```javascript
import { Ionicons } from "@expo/vector-icons";
import React, { useState } from "react";
import Message from "@/components/Message";
```

## Рішення

Ця конфігурація ESLint вирішує цю проблему, використовуючи правило `import/order`. Вона автоматично сортує імпорти за групами, забезпечуючи чистий, послідовний та передбачуваний стиль коду у всьому проєкті.

## Особливості

- ✅ **React** та вбудовані модулі завжди імпортуються першими
- ✅ Зовнішні бібліотеки (наприклад, `@expo/vector-icons`, `expo-router`) слідують після них
- ✅ Внутрішні модулі (з використанням `@/`) мають окрему групу
- ✅ Відносні імпорти (`../`) ідуть останніми
- ✅ Стилі (`.css`, `.scss` тощо) завжди розміщуються в самому кінці
- ✅ Запобігає поширеній помилці `Cannot redefine plugin "import"`, оскільки не намагається перевизначити плагін, який вже надається конфігурацією Expo

## Встановлення та Використання

Дотримуйтесь цих простих кроків, щоб швидко інтегрувати конфігурацію у свій проєкт.

### Крок 1: Встановлення залежностей

У вашому проєкті Expo встановіть необхідні пакети:

```bash
npx expo install eslint eslint-plugin-import
```

### Крок 2: Додавання файлу конфігурації

Створіть файл з назвою `.eslintrc.js` у кореневому каталозі вашого проєкту (якщо його ще немає) та скопіюйте в нього вміст, поданий нижче.

### Крок 3: Автоматичне форматування

Щоб відформатувати весь код у проєкті та застосувати нові правила, виконайте команду в терміналі:

```bash
npx eslint --fix .
```

> **Порада:** Рекомендується налаштувати ваш редактор коду (наприклад, VS Code) на автоматичне виправлення помилок ESLint при збереженні файлу.

## Конфігурація .eslintrc.js

```javascript
const { defineConfig } = require("eslint-define-config");
const expoConfig = require("eslint-config-expo/flat");

module.exports = defineConfig([
  expoConfig,
  {
    ignores: ["dist/*"],
    rules: {
      "import/no-unresolved": "off",
      "import/order": [
        "error",
        {
          groups: [
            "builtin",
            "external", 
            "internal",
            "parent",
            "sibling",
            "index",
            "object",
            "type",
          ],
          pathGroups: [
            {
              pattern: "react",
              group: "external",
              position: "before",
            },
            {
              pattern: "expo",
              group: "external",
              position: "after",
            },
            {
              pattern: "@/**",
              group: "internal",
            },
            {
              pattern: "**/styles{,.*}",
              group: "index",
              position: "after",
            },
            {
              pattern: "**/*.{css,scss,sass,less,styl}",
              group: "index",
              position: "after",
            },
          ],
          pathGroupsExcludedImportTypes: ["react"],
          "newlines-between": "always",
          alphabetize: { order: "asc", caseInsensitive: true },
        },
      ],
    },
  },
]);
```

## Результат

Після застосування конфігурації ваші імпорти будуть автоматично відсортовані та згруповані:

```javascript
import React, { useState } from "react";

import { Ionicons } from "@expo/vector-icons";

import Message from "@/components/Message";

import "./styles.css";
```

---

**Розроблено з ❤️ для спільноти React Native та Expo**