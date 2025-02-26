# Style Guide for TypeScript

## Key Principles

*   **Readability:** Code should be easily understood by all team members, regardless of their familiarity with specific parts of the codebase.
*   **Maintainability:** Code should be structured in a way that allows for easy modification, extension, and debugging over time.
*   **Consistency:**  Following a consistent style across the entire project reduces cognitive load, minimizes errors, and improves collaboration.
*   **Efficiency:** Code should be written with performance in mind, avoiding unnecessary computations or resource usage.
*   **Security:**  Code should be written securely to prevent common vulnerabilities.

## TypeScript Specific Style Guide

### Types vs. Interfaces

*   **Use `interface` for public APIs and contracts:** When defining the shape of objects that will be used across modules or by external consumers, prefer `interface`. Interfaces are generally more flexible for extension and declaration merging.
*   **Use `type` for type aliases and more complex type compositions:** Use `type` when you need to create aliases for complex types, union types, intersection types, or when you need to use features like mapped types or conditional types.

    ```typescript
    // Interface example
    interface User {
      id: string;
      name: string;
    }
    
    // Type alias example
    type StringOrNumber = string | number;
    ```

### Naming Conventions

*   **General Naming:** Use descriptive and meaningful names. Avoid abbreviations unless they are widely understood.
*   **Variables:** Use `camelCase` for variables and parameters.

    ```typescript
    let userName: string;
    const itemCount = 10;
    ```

*   **Constants:** Use `PascalCase` for constants. For top-level constants, consider using `UPPER_CASE_SNAKE_CASE`.

    ```typescript
    const Pi = 3.14159;
    const MAX_USERS = 100;
    ```

*   **Functions:** Use `camelCase` for function names.

    ```typescript
    function calculateTotalAmount() { ... }
    ```

*   **Classes:** Use `PascalCase` for class names.

    ```typescript
    class UserAccount { ... }
    ```

*   **Interfaces and Types:** Use `PascalCase` for interface and type names.

    ```typescript
    interface Product { ... }
    type OrderStatus = 'pending' | 'processing' | 'completed';
    ```

*   **Enums:** Use `PascalCase` for enum names and `PascalCase` for enum members.

    ```typescript
    enum Color {
      Red,
      Green,
      Blue,
    }
    ```

*   **Boolean Variables:**  Use names that clearly indicate a boolean value (e.g., `isActive`, `isEnabled`, `hasError`). Avoid negative prefixes like `isNotValid`.

### Type Annotations and Inference

*   **Explicit Type Annotations for Public APIs:**  Always use explicit type annotations for function parameters, return types, and exported variables, especially in component props and API interfaces. This improves readability and provides clear contracts.
*   **Leverage Type Inference for Local Variables:** For variables within function scopes or component bodies, allow TypeScript to infer types when it's clear and doesn't reduce readability.

    ```typescript
    // Explicit type annotation (recommended for function parameters and exports)
    function formatName(name: string): string {
      return name.trim();
    }

    // Type inference (acceptable for local variables when type is obvious)
    const initialValue = 0; // TypeScript infers 'number'
    ```

*   **Avoid `any` Type:**  Minimize the use of `any`. Prefer more specific types or `unknown` when the type is not immediately known. Use type casting or type guards to narrow down `unknown` types.

### Code Organization

*   **Module Structure (Feature-Based):** Organize code by feature or domain. Create directories for features (e.g., `components/`, `pages/`, `lib/`, `services/`, `styles/`).
*   **Import Paths:** Use absolute import paths for modules within your project to improve code clarity and prevent issues with relative paths, especially when moving files. Configure your `tsconfig.json` `compilerOptions.baseUrl` and `compilerOptions.paths` accordingly.

    ```typescript
    // Absolute import (recommended)
    import Button from 'components/Button';
    import { fetchUserData } from 'services/api';

    // Relative import (avoid when possible)
    import Button from '../../components/Button';
    ```

*   **Exports:** Be explicit with exports. Only export what is intended to be public API of a module. Use named exports over default exports for better discoverability and refactoring.

    ```typescript
    // Named export (recommended)
    export function formatPrice(price: number): string { ... }
    export const defaultCurrency = 'USD';
    
    // Default export (use sparingly, mainly for components)
    const Button = () => { ... };
    export default Button;
    ```

### Generics

*   **Use Generics for Reusable Components and Functions:**  Utilize generics to create reusable components, functions, and data structures that can work with various types while maintaining type safety.
*   **Descriptive Generic Type Parameters:**  Use meaningful names for generic type parameters (e.g., `T`, `Item`, `Data`, `Value`) to improve readability.

    ```typescript
    // Generic component
    interface ListProps<T> {
      items: T[];
      renderItem: (item: T) => React.ReactNode;
    }
    
    const List = <T extends {}>(props: ListProps<T>) => { ... };
    
    // Generic function
    function identity<T>(arg: T): T {
      return arg;
    }
    ```

### Utility Types

*   **Leverage TypeScript Utility Types:**  Utilize built-in utility types like `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `ReturnType`, `Parameters`, etc., to manipulate and create new types efficiently.

    ```typescript
    interface Config {
      apiKey: string;
      timeout?: number;
      retries?: number;
    }
    
    type RequiredConfig = Required<Config>; // Makes all properties of Config required
    type OptionalConfig = Partial<Config>;  // Makes all properties of Config optional
    ```

### Null and Undefined

*   **Be Explicit About Nullable and Undefined Types:**  Clearly define when a variable or property can be `null` or `undefined` using union types or optional properties (`?`).
*   **Use Optional Chaining and Nullish Coalescing:**  Use optional chaining (`?.`) and nullish coalescing (`??`) operators to safely access properties that might be `null` or `undefined`.

    ```typescript
    interface Address {
      street?: string;
      city: string;
    }
    
    const userAddress: Address = { city: 'Yorba Linda' };
    
    const streetName = userAddress.street?.toUpperCase(); // Safe access, streetName will be undefined if street is missing
    
    const city = userAddress.city ?? 'Unknown City'; // city will be 'Yorba Linda' if userAddress.city is defined, otherwise 'Unknown City'
    ```

### Comments and JSDoc

*   **JSDoc for Public APIs:** Document all exported functions, classes, interfaces, types, and enums using JSDoc syntax. Explain the purpose, parameters, return values, and any potential side effects.
*   **Concise Comments for Complex Logic:** Add comments to explain complex or non-obvious logic within functions. Focus on explaining the "why" rather than just the "what."
*   **Avoid Redundant Comments:**  Do not add comments that simply repeat what the code already clearly expresses.
*   **Parameter Property Comments:** Use JSDoc comments to describe parameter properties in constructors when appropriate.
*   **Comments When Calling Functions:** Add comments before function calls if the purpose or context of the call is not immediately clear.
*   **Place Documentation Prior to Decorators:**  When using decorators with classes, methods, or properties, place JSDoc comments before the decorator.

    ```typescript
    /**
     * Formats a price into a currency string.
     * @param price The numerical price value.
     * @param currencyCode The currency code (e.g., 'USD', 'EUR').
     * @returns The formatted price string.
     */
    export function formatPrice(price: number, currencyCode: string): string {
      // Implementation details...
    }
    ```

## General Code Style (JavaScript/TypeScript)

### Naming Conventions (General)

*   **Use English:** Write code and comments in English.
*   **Be Concise and Clear:** Choose names that are short but clearly convey the purpose of the variable, function, or class.
*   **Avoid Hungarian Notation:** Do not prefix variable names with type information (e.g., `strName`, `intCount`). TypeScript's type system already provides this information.

### Code Formatting

*   **Consistent Indentation:** Use consistent indentation (e.g., 2 spaces) throughout the project. Configure your editor or use a code formatter (Prettier) to enforce consistent indentation automatically.
*   **Line Length:**  Adhere to a reasonable line length limit (e.g., 80-120 characters) to improve code readability.
*   **Whitespace:** Use whitespace consistently to improve code readability (e.g., spaces around operators, blank lines to separate logical blocks of code).
*   **Code Formatting Tools (Prettier):**  Use a code formatter like Prettier to automatically format your code and enforce consistent formatting rules. Configure Prettier to match your project's style preferences.

### Comments (General)

*   **Explain "Why," Not Just "What":** Comments should explain the reasoning behind the code, the intent, or the context, rather than simply describing what the code is doing (which should be evident from the code itself).
*   **Keep Comments Up-to-Date:**  Ensure comments are updated when the code changes. Stale or inaccurate comments are worse than no comments at all.
*   **Use Comments Sparingly:**  Well-written, self-documenting code should minimize the need for excessive comments.

### Error Handling (General)

*   **Anticipate Errors:**  Think about potential error scenarios and handle them proactively.
*   **`try...catch` for Error Isolation:**  Use `try...catch` blocks to isolate code that might throw exceptions and handle errors gracefully.
*   **Specific Error Handling:** Catch specific error types when possible to handle different error scenarios appropriately.
*   **Avoid Empty `catch` Blocks:**  Do not use empty `catch` blocks, as they can hide errors and make debugging difficult. At least log the error or re-throw it.
*   **Graceful Error Handling:**  Handle errors in a way that prevents the application from crashing and provides a user-friendly experience (e.g., display error messages, fallback to default behavior).

### Efficiency and Performance

*   **Avoid Unnecessary Computations:**  Optimize code to avoid redundant calculations or operations.
*   **Efficient Data Structures and Algorithms:** Choose appropriate data structures and algorithms for the task to ensure good performance.
*   **Minimize Re-renders (React):**  Optimize React component rendering by using techniques like memoization (`React.memo`, `useMemo`, `useCallback`), avoiding unnecessary state updates, and using efficient rendering patterns.
*   **Code Profiling:**  Use browser developer tools or performance profiling tools to identify performance bottlenecks and optimize critical code paths.

### Security

*   **Input Sanitization and Validation:** Sanitize and validate all user inputs to prevent injection attacks (e.g., Cross-Site Scripting (XSS), SQL Injection).
*   **Secure Data Handling:**  Handle sensitive data securely. Avoid storing sensitive information in client-side code or local storage if possible. Use secure methods for transmitting and storing sensitive data.
*   **Authentication and Authorization:** Implement proper authentication and authorization mechanisms to protect application resources and user data.
*   **Dependency Security:** Regularly audit and update dependencies to address known security vulnerabilities. Use tools like `npm audit` or `yarn audit` to check for vulnerabilities.
*   **HTTPS:**  Ensure the application is served over HTTPS to encrypt communication and protect data in transit.

### Maintainability and Readability

*   **Keep Functions and Components Small:**  Write functions and components that are focused and have a single responsibility. Smaller units of code are generally easier to understand, test, and maintain.
*   **DRY (Don't Repeat Yourself):**  Avoid code duplication. Extract common logic into reusable functions, components, or modules.
*   **KISS (Keep It Simple, Stupid):**  Favor simple and straightforward solutions over complex or overly clever code.
*   **Code Reviews:**  Conduct regular code reviews to catch style inconsistencies, potential bugs, and areas for improvement. Code reviews are a valuable tool for maintaining code quality and sharing knowledge within the team.

## Tooling

*   **ESLint:**  Use ESLint with recommended TypeScript plugins to enforce code style rules, catch potential errors, and improve code quality. Configure ESLint to align with this style guide.
*   **Prettier:**  Use Prettier to automatically format code and enforce consistent code formatting. Integrate Prettier with your editor and CI/CD pipeline.
*   **TypeScript Compiler (`tsc`):**  Utilize the TypeScript compiler to catch type errors and ensure type safety. Configure `tsconfig.json` with strict compiler options (e.g., `strict: true`, `noImplicitAny: true`, `noUnusedLocals: true`, `noUnusedParameters: true`).
*   **Editor Configuration:** Configure your code editor (VS Code, JetBrains, etc.) with ESLint, Prettier, and TypeScript extensions to provide real-time feedback and automated code formatting.

