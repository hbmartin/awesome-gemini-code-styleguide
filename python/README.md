# Style Guide for Modern Python Libraries

## Key Principles

* **Readability:** Code should be easily understood by all team members and users of the library, regardless of their familiarity with specific parts of the codebase.
* **Maintainability:** Code should be structured in a way that allows for easy modification, extension, and debugging over time.
* **Consistency:**  Following a consistent style across the entire library reduces cognitive load, minimizes errors, and improves collaboration.
* **Python Best Practices (PEP 8, PEP 257):**  The style guide should strictly adhere to PEP 8 (style guide for Python code) and PEP 257 (docstring conventions).
* **Efficiency:** Code should be written with performance in mind, avoiding unnecessary computations or resource usage, especially in performance-critical libraries.
* **Security:**  Code should be written securely to prevent common vulnerabilities and protect users of the library.
* **Usability:** The library's API should be intuitive and easy to use for its intended audience.

## Python Specific Style Guide

### PEP 8 Compliance

* **Strictly Adhere to PEP 8:**  This style guide mandates strict adherence to PEP 8 - the Style Guide for Python Code. Use linters (like Flake8, Pylint) to enforce PEP 8 compliance automatically.
* **Key PEP 8 Guidelines:**
  * **Indentation:** Use 4 spaces per indentation level.
  * **Line Length:** Limit lines to a maximum of 79 characters for code and 72 characters for docstrings/comments.
  * **Blank Lines:** Use blank lines to separate functions and classes, and larger blocks of code within functions.
  * **Whitespace:** Use whitespace around operators and after commas, but not inside parentheses or before commas.
  * **Imports:** Imports should be on separate lines, grouped and ordered (standard library imports, related third party imports, local application/library imports). Avoid wildcard imports (`from module import *`).

### Naming Conventions

* **PEP 8 Naming Conventions:** Follow PEP 8 naming conventions for all identifiers.
  * **Variables, function names, method names, package names, module names:** `lowercase_with_underscores`
  * **Classes:** `ClassNameInPascalCase`
  * **Constants:** `UPPER_CASE_WITH_UNDERSCORES`
  * **Type variables:** `PascalCase` short names, preferably `T`, `K`, `V`, etc.
  * **Exception names:** `ClassNameInPascalCase`, suffix with `Error` if it is an error.
  * **Protected members:** `_single_leading_underscore` (internal use, non-public API).
  * **Private members:** `__double_leading_underscore` (name mangling, avoid unless necessary).

* **Be Descriptive and Meaningful:** Choose names that clearly indicate the purpose of the variable, function, class, or module. Avoid single-letter names except for very short-lived variables (e.g., loop counters).
* **Avoid Abbreviations:** Use full words where possible to enhance readability. If abbreviations are necessary, use widely understood and unambiguous ones.
* **Boolean Variables:** Use names that clearly indicate a boolean value (e.g., `is_active`, `has_error`, `should_continue`). Avoid negative prefixes like `is_not_valid`.

### Code Layout

* **Indentation:** Use 4 spaces for indentation. Never use tabs. Configure your editor to insert spaces for tabs.
* **Line Length:** Limit lines to 79 characters for code and 72 characters for docstrings and comments as per PEP 8. Break long lines using parentheses, brackets, or braces to maintain readability.
* **Blank Lines:** Use two blank lines to separate top-level function and class definitions. Use single blank lines within classes to separate method definitions and to separate logical sections within functions.
* **Whitespace:**
  * Surround binary operators with a single space on either side: assignment (`=`), comparisons (`==`, `<`, `>`, `!=`, `<>`, `<=`, `>=`), and arithmetic operators (`+`, `-`, `*`, `/`, `//`, `%`, `@`).
  * Do not use spaces around unary operators (`-`, `+`, `~`).
  * Use spaces after commas in lists, tuples, dictionaries, and function argument lists.
  * Do not use spaces inside parentheses, brackets, or braces.
  * Place a space after the colon in dictionaries, but not before.
  * Do not put spaces around `()` in function calls.

* **Imports:**
  * Put imports at the top of the file, after module docstrings and module globals and constants.
  * Imports should be grouped in the following order:
        1. Standard library imports.
        2. Related third-party imports.
        3. Local application/library specific imports.
  * Put a blank line between each group of imports.
  * Use absolute imports for code within the library package. Use relative imports only for imports within the same package (and sparingly).
  * Avoid imports from subpackages when possible, import directly from the top-level package instead for clarity.
  * Avoid wildcard imports (`from module import *`). They make it unclear what names are present in the namespace and can cause naming conflicts.

### Comments and Docstrings

* **Docstrings (PEP 257):** Write comprehensive docstrings for all public modules, classes, functions, and methods as per PEP 257.
  * **Module Docstrings:**  Start with a one-line summary of the module's purpose, followed by a blank line and a more detailed explanation if needed.
  * **Function and Method Docstrings:**  Use the imperative mood for the summary line ("Do this, not that"). Document arguments, return values, and exceptions raised. Use a consistent docstring style (e.g., Google style, NumPy style, reStructuredText style - choose one and stick to it).
  * **Class Docstrings:**  Summarize the class's behavior and major public attributes in the class docstring. Document the constructor in its docstring.
  * **Keep Docstrings Up-to-Date:**  Ensure docstrings are updated whenever the code changes.
* **Inline Comments:** Use inline comments sparingly to explain complex or non-obvious logic. Focus on explaining the "why" rather than the "what."
* **Comment Style:**  Use complete sentences for comments, starting with a capital letter and ending with a period. Use block comments for multi-line explanations.
* **Avoid Redundant Comments:**  Do not add comments that simply repeat what the code already clearly expresses.
* **Parameter Property Comments:** Use comments to describe parameter properties in constructors when appropriate, especially for complex initialization logic.
* **Comments When Calling Functions:** Add comments before function calls if the purpose or context of the call is not immediately clear.

### Type Hints

* **Embrace Type Hints:**  Use type hints (PEP 484, PEP 526) extensively throughout the library to improve code readability, enable static analysis, and catch type-related errors early.
* **Annotate Function Signatures:**  Type-annotate function parameters and return values.
* **Annotate Variables:**  Type-annotate variables, especially module-level variables, class attributes, and complex data structures.
* **Use `typing` Module:**  Utilize types from the `typing` module for more complex type annotations (e.g., `List`, `Dict`, `Tuple`, `Optional`, `Union`, `Callable`, `Generic`).
* **Be Specific with Types:**  Use the most specific type possible. Avoid using `Any` unless absolutely necessary. Use `Optional[...]` instead of `Union[..., None]` for optional types.
* **Runtime Type Checking (Optional):** Consider using runtime type checking tools (like `beartype`) for critical parts of the library or for enforcing type contracts at runtime, but be mindful of performance overhead.

### Error Handling

* **Use Exceptions for Error Conditions:** Raise exceptions to signal error conditions. Use built-in exceptions where appropriate, or define custom exception classes for library-specific errors.
* **Custom Exception Classes:** Create custom exception classes that inherit from `Exception` or a more specific built-in exception type (e.g., `ValueError`, `TypeError`, `IOError`) to provide more context and allow for specific error handling. Name custom exceptions clearly, often ending with "Error" (e.g., `APIConnectionError`, `InvalidInputError`).
* **`try...except` Blocks for Error Handling:** Use `try...except` blocks to handle expected exceptions gracefully. Catch specific exception types whenever possible to handle different error scenarios appropriately.
* **Avoid Bare `except`:**  Never use bare `except` clauses (`except:`), as they catch all exceptions, including unexpected ones, making debugging difficult and potentially masking errors.
* **`finally` Clause for Cleanup:** Use the `finally` clause in `try...except...finally` blocks to ensure cleanup code (e.g., closing files, releasing resources) is always executed, regardless of whether an exception occurred.
* **`else` Clause in `try...except`:** Use the `else` clause in `try...except...else` blocks for code that should execute only if no exceptions were raised in the `try` block.
* **Logging:** Implement comprehensive logging using the `logging` module to record errors, warnings, and informational messages. Configure logging levels appropriately for different environments (development, testing, production). Log sufficient context to aid in debugging (e.g., timestamps, function names, input parameters, user IDs).

### Code Organization (Modules and Packages)

* **Modular Design:** Design the library as a collection of modules and packages, each responsible for a specific set of functionalities.
* **Package Structure:** Organize related modules into packages. Use a clear and logical package hierarchy that reflects the library's domain.
* **`__init__.py` Files:** Use `__init__.py` files to define packages and control namespace behavior. In modern Python (3.3+), implicit namespace packages are also an option for simpler structures.
* **Clear Module Boundaries:** Define clear boundaries between modules and packages to minimize dependencies and promote modularity.
* **Internal Modules:** Use leading underscores (`_`) for module and function names to indicate internal implementation details that are not part of the public API.
* **Entry Points:** Define clear entry points for the library's main functionalities. For command-line interfaces, use `if __name__ == "__main__":` blocks or dedicated CLI tools (like `Click`, `Typer`, `argparse`).

### Testing

* **Comprehensive Unit Tests:** Write comprehensive unit tests for all public functions, classes, and methods. Aim for high test coverage to ensure code correctness and prevent regressions.
* **Test-Driven Development (TDD) or Test-First Approach (Recommended):** Consider adopting a TDD or test-first approach where tests are written before the code implementation. This helps in designing better APIs and ensures testability.
* **Testing Frameworks:** Use a robust testing framework like `pytest` or `unittest`. `pytest` is generally preferred for its ease of use and powerful features.
* **Test Organization:** Organize tests in a `tests/` directory mirroring the library's package structure. Use clear and descriptive test function names.
* **Test Fixtures and Mocks:** Utilize test fixtures to set up test environments and mocks/stubs to isolate units under test and control dependencies.
* **Integration Tests:** Include integration tests to verify the interaction between different modules and components of the library.
* **Continuous Integration (CI):** Integrate testing into a CI/CD pipeline to automatically run tests on every code change and ensure code quality.

## Library Specific Style Guide

### API Design

* **Clarity and Intuitiveness:** Design APIs that are clear, intuitive, and easy to understand for users. The API should be self-documenting as much as possible through good naming and type hints.
* **Consistency:** Maintain consistency in naming, parameter order, and API patterns across the library. Follow established Python conventions and idioms.
* **Discoverability:** Make the API discoverable. Use clear module and package structures, provide comprehensive documentation, and consider using features like auto-completion and type hinting to aid discoverability in IDEs.
* **Ease of Use:** Design APIs that are easy to use for common use cases. Provide sensible defaults and minimize the amount of boilerplate code users need to write.
* **Flexibility and Extensibility:** Design APIs that are flexible enough to handle a variety of use cases and extensible to accommodate future requirements. Consider providing hooks, callbacks, or plugin mechanisms for customization.
* **Principle of Least Astonishment:** The API should behave in a way that is expected and not surprising to users familiar with Python and similar libraries.
* **Error Handling in APIs:** Design APIs to handle errors gracefully and provide informative error messages to users. Raise exceptions for invalid input or unexpected conditions.

### Versioning and Compatibility

* **Semantic Versioning (SemVer):**  Use Semantic Versioning (SemVer) to manage library releases. Follow SemVer rules for incrementing major, minor, and patch versions based on the types of changes introduced.
* **Backwards Compatibility:**  Strive to maintain backwards compatibility whenever possible, especially for minor and patch releases. Avoid breaking changes unless absolutely necessary.
* **Deprecation Policy:**  When introducing breaking changes or deprecating features, provide clear deprecation warnings and a reasonable migration path for users. Document deprecated features clearly and specify when they will be removed.
* **Changelog:** Maintain a detailed changelog (e.g., `CHANGELOG.md` or release notes) that documents changes in each version, including new features, bug fixes, and breaking changes.

### Documentation (Beyond Docstrings)

* **Comprehensive Documentation:**  Provide comprehensive documentation for the library beyond just docstrings. This should include:
  * **Installation Instructions:** Clear instructions on how to install the library (e.g., using `pip`).
  * **Quick Start Guide:** A simple example to get users started quickly with the library.
  * **Tutorials:** Step-by-step tutorials that guide users through common use cases and features.
  * **API Reference:**  Detailed API reference documentation generated from docstrings (using tools like Sphinx, pdoc).
  * **Examples:**  Numerous code examples showcasing different aspects of the library.
  * **Contribution Guidelines:**  If it's an open-source library, provide clear contribution guidelines.
* **Documentation Format:** Use a documentation generator like Sphinx or pdoc to create well-formatted and easily navigable documentation. Host the documentation online (e.g., Read the Docs).
* **Examples in Documentation:** Include plenty of code examples in the documentation to illustrate how to use the library's features. Ensure examples are runnable and tested.

### Packaging and Distribution

* **Setuptools or Poetry:** Use modern packaging tools like `setuptools` or `Poetry` to manage library metadata, dependencies, and packaging. `Poetry` is often preferred for its dependency management and project management features.
* **`pyproject.toml` and `setup.py`:** Configure `pyproject.toml` (for build system configuration) and `setup.py` (or declarative configuration in `setup.cfg`) to define library metadata (name, version, author, description, license, dependencies, entry points, etc.).
* **Wheel and Source Distribution:** Create both wheel (`.whl`) and source distribution (`.tar.gz`) packages for distribution on PyPI (Python Package Index).
* **PyPI Distribution:** Publish the library to PyPI to make it easily installable for users via `pip install`.
* **Virtual Environments:**  Encourage users to use virtual environments (e.g., `venv`, `virtualenv`, `conda`) to isolate library dependencies and avoid conflicts.

### Dependencies Management

* **Minimal Dependencies:**  Minimize the number of dependencies the library relies on. Only include dependencies that are truly necessary.
* **Dependency Pinning:**  Pin dependencies to specific versions or version ranges in `setup.py` or `pyproject.toml` to ensure reproducible builds and avoid compatibility issues. Use version ranges carefully to balance stability and security updates.
* **Optional Dependencies (Extras):**  Use "extras" in `setup.py` or `pyproject.toml` to define optional dependencies that are needed for specific features. This allows users to install only the dependencies they need.
* **Dependency Conflicts:**  Be mindful of potential dependency conflicts with other libraries. Try to use dependencies that are widely used and well-maintained.

## General Code Style (Python)

### Naming Conventions (General)

* **Use English:** Write code and comments in English.
* **Be Concise and Clear:** Choose names that are short but clearly convey the purpose of the variable, function, or class.
* **Avoid Hungarian Notation:** Do not prefix variable names with type information (e.g., `str_name`, `int_count`). Python's dynamic typing and type hints make this unnecessary and less readable.

### Code Formatting

* **Consistent Formatting:**  Maintain consistent code formatting throughout the library. Use a code formatter like Black or autopep8 to automatically format your code and enforce consistent formatting rules. Configure the formatter to align with PEP 8 and project preferences.
* **Code Formatting Tools (Black, autopep8):**  Integrate a code formatter (Black or autopep8) into your development workflow and CI/CD pipeline to automatically format code on save and before commits.

### Comments (General)

* **Explain "Why," Not Just "What":** Comments should explain the reasoning behind the code, the intent, or the context, rather than simply describing what the code is doing (which should be evident from the code itself).
* **Keep Comments Up-to-Date:**  Ensure comments are updated when the code changes. Stale or inaccurate comments are worse than no comments at all.
* **Use Comments Judiciously:**  Well-written, self-documenting code should minimize the need for excessive comments.

### Error Handling (General)

* **Anticipate Errors:**  Think about potential error scenarios and handle them proactively.
* **`try...except` for Error Isolation:**  Use `try...except` blocks to isolate code that might raise exceptions and handle errors gracefully.
* **Specific Error Handling:** Catch specific exception types when possible to handle different error scenarios appropriately.
* **Avoid Empty `except` Blocks:**  Do not use empty `except` blocks, as they can hide errors and make debugging difficult. At least log the error or re-raise it.
* **Graceful Error Handling:**  Handle errors in a way that prevents the library from crashing and provides informative error messages to users.

### Efficiency and Performance

* **Algorithm and Data Structure Choice:**  Choose efficient algorithms and data structures for performance-critical parts of the library. Consider the time and space complexity of algorithms.
* **Profiling and Optimization:**  Use profiling tools (like `cProfile`, `line_profiler`, `memory_profiler`) to identify performance bottlenecks and memory usage issues. Optimize critical code paths based on profiling results.
* **Vectorization and Libraries (NumPy, etc.):**  Leverage vectorized operations and optimized libraries like NumPy for numerical computations, where appropriate, to improve performance.
* **Cython or Numba (Performance-Critical Code):**  For extremely performance-critical sections, consider using Cython or Numba to compile Python code to native machine code for significant speedups.
* **Caching:**  Implement caching mechanisms (e.g., memoization, caching function results) to avoid redundant computations, especially for expensive operations or frequently accessed data.

### Security

* **Input Validation and Sanitization:** Validate and sanitize all external inputs (user input, data from files, network requests, etc.) to prevent injection vulnerabilities (e.g., command injection, path traversal, format string vulnerabilities).
* **Secure Data Handling:**  Handle sensitive data securely. Avoid logging sensitive information. Use secure methods for storing and transmitting sensitive data.
* **Dependency Security:** Regularly audit and update dependencies to address known security vulnerabilities. Use tools like `pip audit` or `safety` to check for vulnerabilities in dependencies.
* **Principle of Least Privilege:**  Run code with the minimum necessary privileges. Avoid using `sudo` or running as root unnecessarily.
* **Security Reviews:** Conduct security reviews of the library's code, especially for libraries that handle sensitive data or perform security-critical operations. Consider using static analysis security tools (like Bandit, Semgrep).

### Maintainability and Readability

* **Keep Functions and Methods Short:**  Write functions and methods that are focused and have a single responsibility. Shorter units of code are generally easier to understand, test, and maintain.
* **DRY (Don't Repeat Yourself):**  Avoid code duplication. Extract common logic into reusable functions, classes, or modules.
* **KISS (Keep It Simple, Stupid):**  Favor simple and straightforward solutions over complex or overly clever code.
* **Code Reviews:**  Conduct regular code reviews to catch style inconsistencies, potential bugs, and areas for improvement. Code reviews are a valuable tool for maintaining code quality and sharing knowledge within the team.
* **Refactoring:**  Regularly refactor code to improve its structure, readability, and maintainability. Address technical debt proactively.

## Tooling

* **Linters (Flake8, Pylint):**  Use linters like Flake8 and Pylint to enforce PEP 8 compliance, detect style violations, and identify potential code quality issues. Configure linters to align with this style guide and project preferences.
* **Formatters (Black, autopep8):**  Use code formatters like Black or autopep8 to automatically format code and enforce consistent code formatting. Integrate a formatter with your editor and CI/CD pipeline.
* **Type Checkers (MyPy, Pyright):**  Use static type checkers like MyPy or Pyright to perform static type analysis and catch type errors. Configure type checkers with strict settings to maximize type safety.
* **Testing Frameworks (pytest, unittest):**  Use a testing framework like pytest or unittest for writing and running tests.
* **Documentation Generators (Sphinx, pdoc):**  Use documentation generators like Sphinx or pdoc to generate API documentation from docstrings.
* **Virtual Environment Managers (venv, Poetry, Conda):** Use virtual environment managers like `venv`, Poetry, or Conda to manage project dependencies and create isolated environments.
* **Dependency Auditors (pip audit, safety):**  Use dependency auditing tools like `pip audit` or `safety` to check for security vulnerabilities in dependencies.
* **Editor Configuration:** Configure your code editor (VS Code, PyCharm, etc.) with linters, formatters, type checkers, and testing integrations to provide real-time feedback and automated code formatting and analysis.
