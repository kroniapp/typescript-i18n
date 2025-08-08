# Typescript-i18n

A simple and type-safe internationalization (i18n) library for TypeScript projects.

## Features

- ✅ Type-safe: Get autocompletion for your translation keys.
- ✅ Simple API: Easy to learn and use.
- ✅ Lightweight: No external dependencies.
- ✅ Flexible: Supports nested objects and functions for dynamic translations.
- ✅ Fallback: Automatically falls back to the default language for missing translations.

## Installation

Install the package using your favorite package manager:

```bash
npm install typescript-i18n
# or
yarn add typescript-i18n
# or
pnpm add typescript-i18n
```

## Getting Started

1.  **Create your translation files:**

    `en.ts`
    ```typescript
    export const en = {
      hello: "Hello",
      user: {
        name: "Name",
        email: "Email"
      }
    };
    ```

    `es.ts`
    ```typescript
    export const es = {
      hello: "Hola",
      user: {
        name: "Nombre",
        email: "Correo electrónico"
      }
    };
    ```

2.  **Initialize the library:**

    `i18n.ts`
    ```typescript
    import { TypescriptI18n } from "typescript-i18n";
    import { en } from "./en";
    import { es } from "./es";

    export const lang = new TypescriptI18n(
      { en, es },
      { defaultLanguage: "en" }
    );
    ```

3.  **Use it in your application:**

    `main.ts`
    ```typescript
    import { lang } from "./i18n";

    // Get the translation strings for the current language
    const t = lang.s;

    console.log(t.hello); // Output: Hello
    console.log(t.user.name); // Output: Name

    // Switch to Spanish
    lang.currentLanguage = "es";

    console.log(t.hello); // Output: Hola
    console.log(t.user.name); // Output: Nombre
    ```

## Usage

### Variables in translations

You can use functions to create dynamic translations with variables.

`en.ts`
```typescript
export const en = {
  welcome: ({ name }: { name: string }) => `Welcome, ${name}!`
};
```

`main.ts`
```typescript
import { lang } from "./i18n";

const t = lang.s;

console.log(t.welcome({ name: "Alice" })); // Output: Welcome, Alice!
```

### Pluralization

Functions can also be used for pluralization.

`en.ts`
```typescript
export const en = {
  apples: (count: number) => `You have ${count} ${count === 1 ? "apple" : "apples"}.`
};
```

`main.ts`
```typescript
import { lang } from "./i18n";

const t = lang.s;

console.log(t.apples(1)); // Output: You have 1 apple.
console.log(t.apples(5)); // Output: You have 5 apples.
```

### Nested Translations

You can organize your translations into nested objects for better organization.

`en.ts`
```typescript
export const en = {
  screens: {
    home: {
      title: "Home Screen",
      subtitle: "Welcome to our app!"
    },
    profile: {
      title: "Profile Screen"
    }
  }
};
```

`main.ts`
```typescript
import { lang } from "./i18n";

const t = lang.s;

console.log(t.screens.home.title); // Output: Home Screen
```

You can also use the `get()` method with dot notation to access nested translations:

```typescript
console.log(lang.get("screens.home.title")); // Output: Home Screen
```

### Fallback for Missing Translations

If a translation key is not found in the current language, the library will automatically fall back to the default language.

`en.ts`
```typescript
export const en = {
  hello: "Hello",
  onlyInEnglish: "This is only in English"
};
```

`es.ts`
```typescript
export const es = {
  hello: "Hola"
};
```

`i18n.ts`
```typescript
import { TypescriptI18n } from "typescript-i18n";
import { en } from "./en";
import { es } from "./es";

export const lang = new TypescriptI18n(
  { en, es },
  { defaultLanguage: "en" }
);

lang.currentLanguage = "es";
const t = lang.s;

console.log(t.hello); // Output: Hola
console.log(t.onlyInEnglish); // Output: This is only in English
```

## API Reference

### `TypescriptI18n<Strings, CurrentLanguage>`

The main class for the i18n library.

#### `constructor(strings: Strings, options?: Partial<TypescriptI18nOptions<CurrentLanguage>>)`

-   `strings`: An object containing all the language translations.
-   `options`: An optional configuration object.
    -   `defaultLanguage`: The language to use as a fallback. Defaults to `"en"`.

#### `s: Strings[CurrentLanguage]`

A getter that returns the translation strings for the current language.

#### `get(key: string, variables?: any): string`

Retrieves a translation by its key.

-   `key`: The key of the translation to retrieve. Can use dot notation for nested objects (e.g., `"user.name"`).
-   `variables`: An optional object of variables to pass to a function-based translation.

#### `languages: (keyof Strings)[]`

A getter that returns an array of all available language codes.

#### `currentLanguage: keyof Strings`

A getter and setter for the current language.

#### `getUserLanguage(languages: string | readonly string[]): key of Strings`

Determines the best matching language from a list of user-preferred languages (e.g., from `navigator.languages`).

-   `languages`: A string or array of strings representing the user's preferred languages.

## License

MIT
