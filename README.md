# ESLint Import Sorting Configuration for Expo Projects

This repository contains a ready-to-use ESLint configuration specifically designed for automatic organization and sorting of imports in React Native projects using Expo.

## The Problem

Development can be chaotic, and disorganized imports in `.js` and `.tsx` files are one of the most common issues. This not only makes code harder to read but also complicates team collaboration, as each developer may have their own formatting style.

**Typical example of unorganized imports:**
```javascript
import { Ionicons } from "@expo/vector-icons";
import React, { useState } from "react";
import Message from "@/components/Message";
```

## The Solution

This ESLint configuration solves this problem using the `import/order` rule. It automatically sorts imports by groups, ensuring clean, consistent, and predictable code style throughout your project.

## Features

- ✅ **React** and built-in modules are always imported first
- ✅ External libraries (e.g., `@expo/vector-icons`, `expo-router`) follow after them
- ✅ Internal modules (using `@/`) have their own separate group
- ✅ Relative imports (`../`) come last
- ✅ Styles (`.css`, `.scss`, etc.) are always placed at the very end
- ✅ Prevents the common `Cannot redefine plugin "import"` error by not attempting to redefine a plugin already provided by Expo configuration

## Installation and Usage

Follow these simple steps to quickly integrate the configuration into your project.

### Step 1: Install Dependencies

In your Expo project, install the required packages:

```bash
npx expo install eslint eslint-plugin-import
```

### Step 2: Add Configuration File

Create a file named `.eslintrc.js` in your project's root directory (if it doesn't exist yet) and copy the content provided below into it.

### Step 3: Auto-formatting

To format all code in your project and apply the new rules, run the following command in your terminal:

```bash
npx eslint --fix .
```

> **Tip:** It's recommended to configure your code editor (e.g., VS Code) to automatically fix ESLint errors when saving files.

## .eslintrc.js Configuration

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

## Result

After applying the configuration, your imports will be automatically sorted and grouped:

```javascript
import React, { useState } from "react";

import { Ionicons } from "@expo/vector-icons";

import Message from "@/components/Message";

import "./styles.css";
```

## Contributing

We welcome contributions! Please feel free to submit a Pull Request or open an Issue if you have suggestions for improvements.

## License

This project is open source and available under the [MIT License](LICENSE).

---

**Made with ❤️ for the React Native and Expo community**