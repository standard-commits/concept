# Standard Commits Concept

> 🚧 ! THIS DOCUMENT IS **WORK IN PROGRESS** ! 🚧

## Motivation

The _Standard Commit_ format is a structured approach to writing commit messages that enhances clarity and consistency in version control systems. This format is particularly beneficial for projects with multiple contributors, as it provides a common language for describing changes. Without getting lost in too many frills, the question this section aims to answer is:

**What are the benefits of using the Standard Commit_ format over existing commit message formats?**

In the following table, we summarize the main advantages of the _Standard Commit_ format compared to other commit message formats:
| Feature/Aspect                       | _Standard Commit_ Format | Conventional Commits | Tim Pope Style   | Gitmoji Commits  |
|--------------------------------------|--------------------------|----------------------|------------------|------------------|
| Grammar-based                        | Yes                      | Partially            | No               | No               |
| Structured Format                    | High                     | Medium               | Low              | Low              |
| Verbosity                            | Low                      | Medium               | High             | Medium           |
| Readability                          | Medium                   | Medium               | High             | Medium           |
| Consistency                          | High                     | Medium               | Low              | Medium           |
| Grepability                          | High                     | Medium               | Low              | Low              |
| $\frac{Conciseness}{Expressiveness}$ | High                     | Medium               | Low              | Medium           |
| Scope Annotation                     |
| Reason Annotation                    |
| Scope Annotation                     |
| Importance Levels                    |
| Explanatory Body and Footer          |
| Support for Multiple Verbs           |
| Support for Undoing Changes          |
| Support for Breaking Changes         |
| Support for Critical Changes         |
| Support for Non-Breaking Changes     |
| Support for Non-API Changes          |
| Support for API Changes              |




## Layout

The _Standard Commit_ format, as universally recognized, is composed of two distinct fragments: the _structured_ (or formal) component and the _unstructured_ (or expository) component. The former adheres to a prescribed format, ensuring clarity and consistency in commit messages. It is formally expressed as: `<verb><importance>(<scope>)[<reason>]`. The latter expands upon the structured prefix, providing deeper insight into the modification. It is consists of three elements: `<summary>`, `<body>`, and `<footer>`.

**Syntax Specification**

```ebnf
<verb><importance>(<scope>)[<reason>]: <summary>

<body>

<footer>
```

**Example**

```
fix!(lib/parser): handle null values in AST transformation

Previously, the parser crashed when encountering null values in the AST.
This fix ensures that such cases are correctly handled by introducing a
fallback mechanism.

BREAKING CHANGE: The `parseNode` function now returns an `Option<Node>`
instead of `Node`, requiring updates in all downstream consumers.
```

## Structured fragment

### \<verb>

A `<verb>` describes **how** something has changed via an _expectation_. An _expectation_ is a requirement that the code should respect.
Here are the verbs we strongly encourage the usage of (and discourage adding others):


- `add` (_add_) : Adds an expectation

   Adds new content to the repository by **expecting** it to behave as intended.

   ```
   add(lib/ast)[feat]: impl `Display` for `Expression`
   ```

- `rem` (_remove_) : Removes an expectation

   Removes content from the repository, consequently **dropping expectations** about it.

   ```
   rem(style/exe)[simp]: verbose comment of `main`
   ```

- `ref` (_refactor_) : Maintain the expectation by changing the approach

   Change approach (implementation) details by **mantaining the same expectations** about its behaviour.

   ```
   ref(lib/interpreter)[ideom]: pass rules as `Iterator`
   ```

- `fix` (_fix_) : Makes the approach comply with expectations

   Correct the approach by **complying with expectations** (that were previously belived satisfied).

   ```
   fix(lib/parser): parse arbitraly spaced epressions
   ```

   <details>
   <summary> <i>more</i> </summary>

   1. the `<reason>` should **not** be _why the change was made_ -- that is obsviusly to comply with expectations -- but _why it was wrong_.

   </details>

- `undo` (_undo_) : Undoes changes to an expectation

   Bring expetation back to a previus state, by reverting specific commits.

   ```
   undo(lib/parser)[err]: "fix: parse arbitraly spaced expressions"
   ```

   <details>
   <summary> <i>more</i> </summary>

   1. Prefer `undo` over other `add` or `rem` if the changes to revert are perfectly covered by some commits.

   2. Prefer `ref` or `fix` over `undo` if expectations don't change.
   </details>

### \<importance>

This field is **optional**. That is, it should be omitted whenever possible.

- `?` (_question_) : It changes something exposed externally, but not an API. It should **not** be breaking for projects that depend on the underlying repository.

    <details>
    <summary> <i>example</i> </summary>

    An example of an _question_ (`?`) importance could be used when the output of the program shifts to a new more explicative version. For instance, changing the output from:
    ```bash
    Result: 7
    ```
    to
    ```bash
    Calculation Result: 7 (Success)
    ```

    This update does not break compatibility since the core functionality and APIs remain unchanged, but users or scripts parsing the output may notice differences.

    </details>

- `!` (_exclamation_) : It changes an API exposed externally. It is breaking for projects that depend on the underlying repository.

    <details>
    <summary> <i>example</i> </summary>

    An example of an _exclamation_ (`!`) importance would be renaming or removing a public function that downstream projects rely on. For instance, changing the signature of a function from:
    ```rust
    pub fn calculate(a: i32, b: i32) -> i32
    ```
    to
    ```rust
    pub fn calculate(values: Vec<i32>) -> i32
    ```
    </details>

- `!!` (_loud exclamation_) : This change is **critical** -- previous versions have severe issues that must be addressed. Projects depending on the underlying repository should update immediately.
        The footer **must** specify the last safe commit.

    <details>
    <summary> <i>example</i> </summary>

    An example of a _loud exclamation_ (`!!`) importance would be fixing a severe security vulnerability or resolving a bug that causes data corruption. For instance:
    1. Fixing a bug where a memory leak causes system crashes under heavy load.
    2. Patching a security flaw that exposes sensitive user data.

    </details>

