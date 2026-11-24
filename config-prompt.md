# Code Formatter Configuration Prompt Templates

A standardized framework for prompting AI coding assistants to create, modify, and audit code formatter configurations across system programming, networking, and game server development projects.

---

## Table of Contents

1. [Master Prompt Template (Generic)](#1-master-prompt-template-generic)
2. [Language-Specific Prompt Templates](#2-language-specific-prompt-templates)
   - [C & C++ (Clang-Format / Uncrustify)](#template-c--c-clang-format--uncrustify)
   - [Rust (rustfmt.toml)](#template-rust-rustfmttoml)
   - [Go (gofumpt / golangci-lint)](#template-go-gofumpt--golangci-lint)
   - [C & Objective-C (Uncrustify)](#template-c--objective-c-uncrustify)
3. [Agent Validation & Verification Protocol](#3-agent-validation--verification-protocol)

---

## 1. Master Prompt Template (Generic)

Copy and adapt this template when requesting formatter configurations for any tool or language:

```markdown
I want you to modify my existing `[CONFIG_FILE_NAME]` configuration for **[TARGET_LANGUAGES]** development using **[FORMATTER_TOOL]**.

### Goals

Create a clean, consistent, professional coding style suitable for [DOMAIN_GOALS, e.g., system programming, networking, high-performance game servers].

### Requirements

1. **Language & Version Support**
   - Target languages/standards: [e.g., C++20, Rust 2021, Go 1.22, Objective-C v1/v2]
   - Preserve valid language syntax and do not introduce rules that break language-specific constructs.

2. **Indentation & Whitespace**
   - Indentation width: [e.g., 4 spaces / tabs].
   - Tab policy: [e.g., Never use tabs / Tabs only for indentation].
   - Consistent indentation for nested blocks, control flows, functions, and composite types.
   - Continuation indentation policy for wrapped expressions and parameter lists.

3. **Brace & Block Placement**
   - Brace style: [e.g., K&R (1TBS) / Allman / Stroustrup].
   - Opening brace rules: [Specify whether opening brace is attached on same line or breaks onto a newline for functions vs. types vs. control flow].
   - Closing brace alignment: Aligned with the corresponding statement/keyword.
   - Attached keywords: Policy for `else`, `catch`, `finally`, `while` in do-while.

4. **Spacing & Operators**
   - Binary operators: Spacing around arithmetic, assignment, comparison, logical, and bitwise operators.
   - Unary operators: Spacing for prefix/postfix operators, address-of, dereference, negation.
   - Parentheses & Brackets: Spacing inside/outside parentheses, square brackets, type casts, and attribute lists.
   - Pointers / References / Types: Attachment and spacing rules (e.g., `Type *name` vs `Type* name`, `&name`).
   - Keyword spacing: Space after control flow keywords (`if`, `for`, `while`, `switch`, `return`), with no space after built-ins like `sizeof`.

5. **Line Width & Wrapping**
   - Maximum column width: [e.g., 100 or 120 columns].
   - Parameter list wrapping: Consistent breaking of long function prototypes, calls, and struct initializers.
   - Multi-line expressions: Operator positioning on continuation lines (leading vs. trailing operators).

6. **Comments**
   - Preserve all existing comments and their contents verbatim.
   - Do not reflow comment text, word-wrap comments, or corrupt ASCII diagrams/tables.
   - Format block and single-line comment continuations cleanly.
   - Do not convert between comment syntaxes (e.g., preserve both `//` and `/* */`).

7. **[LANGUAGE_SPECIFIC_SECTION]**
   - [Add specific rules for templates, macros, attributes, match arms, struct tags, etc.]

### Important Constraints

- First inspect the existing configuration and installed tool version (`[FORMATTER_TOOL] --version`).
- Modify the existing configuration in place rather than replacing it from scratch.
- Only use options supported by the installed tool version.
- Do not modify source code files.
- Do not introduce semantic changes.
- Identify and eliminate conflicting or contradictory options.
- Add descriptive comment headers to configuration sections explaining key style decisions.

### Validation Protocol

After modifying the configuration:
1. Verify the formatter parses the configuration without errors or warnings.
2. Test formatting on representative sample files covering standard and edge-case syntax.
3. Validate syntax integrity of the formatted output using the language compiler or linter.
4. Verify idempotency: running the formatter a second time on formatted output must produce zero changes (`diff` is empty).
5. Summarize key configuration changes and document any unsupported rules or tool limitations.
```

---

## 2. Language-Specific Prompt Templates

### Template: C & C++ (Clang-Format / Uncrustify)

```markdown
I want you to configure my `.clang-format` (or `uncrustify-cpp.cfg`) for **C and modern C++ (C++17 / C++20)** development.

### Goals
Create a clean, robust, and modern style suitable for high-performance networking, low-latency game servers, and systems programming.

### Requirements

1. **Language & Standard Support**
   - Support C11/C17 and modern C++17/C++20.
   - Properly format templates, concepts, lambdas, structured bindings, and attributes (`[[nodiscard]]`).

2. **Indentation**
   - 4 spaces indentation per level.
   - Never use tabs (`UseTab: Never` / `indent_with_tabs = 0`).
   - Indent `switch` cases by 4 spaces.
   - Indent access specifiers (`public:`, `protected:`, `private:`) properly (0 or 2 spaces).
   - Namespace indentation: Do not indent inside namespaces (or indent innermost only).

3. **Braces**
   - K&R / Attach style (`BreakBeforeBraces: Attach`):
     - Functions: Opening brace on same line.
     - Conditionals & loops: Opening brace on same line.
     - Classes & Structs: Opening brace on same line.
   - Attached constructs: `else`, `catch`, and `while` in `do-while` attached to `}` (`} else {`).

4. **Spacing & Pointers**
   - Pointers and references: Pointer asterisk and reference ampersand attached to the variable name (`Type *name`, `const Type &ref`).
   - Templates: No space inside `<...>` (`std::vector<int>`, not `std::vector< int >`).
   - Lambdas: Clean spacing around captures and argument lists (`[&](int x) { ... }`).
   - Space after control flow keywords (`if`, `for`, `while`, `switch`); no space after function names.
   - No unnecessary spaces inside parentheses.

5. **Line Width & Wrapping**
   - Column limit: 100 or 120 columns.
   - Break long function signatures and call arguments at commas.
   - Wrapped boolean expressions: Operators at the start of continued lines (`&&`, `||`).
   - BinPackArguments / BinPackParameters: Format consistently across definitions and calls.

6. **Preprocessor & Macros**
   - `#` pinned to column 1 (`IndentPPDirectives: None` or `BeforeHash`).
   - Align multi-line macro backslashes (`\`) at the right margin.
   - Multi-line macro bodies indented 4 spaces.

7. **Comments**
   - Preserve comments verbatim; do not reflow text or wrap comments (`ReflowComments: false`).

### Validation
- Validate configuration parsing with the installed tool version.
- Format representative C++ files containing templates, lambdas, structs, macros, and control flow.
- Verify syntax with `clang++ -fsyntax-only -std=c++20`.
- Verify idempotency (pass 1 == pass 2).
- Summarize changes and tool limitations.
```

---

### Template: Rust (rustfmt.toml)

```markdown
I want you to configure my `rustfmt.toml` for **Rust (Edition 2021 / 2024)** development.

### Goals
Create an idiomatic, readable, and maintainable formatting style suitable for systems and backend game services.

### Requirements

1. **Rust Edition & Stability**
   - Target: Rust 2021/2024 edition (`edition = "2021"`).
   - Only use stable `rustfmt` configuration options unless nightly flags are explicitly permitted.

2. **Indentation & Line Width**
   - 4 spaces indentation (`tab_spaces = 4`, `hard_tabs = false`).
   - Maximum width: 100 columns (`max_width = 100`).
   - Continuation indent: 4 spaces for multi-line expressions and function arguments.

3. **Control Flow, Match Arms, & Blocks**
   - K&R style braces for functions, structs, enums, `impl` blocks, and loops.
   - Match expressions: Clean formatting of match arms; consistent trailing commas.
   - Single-line vs. multi-line match arm bodies and closures.

4. **Imports (`use` declarations)**
   - Group imports logically (`std`, external crates, crate-internal).
   - Granularity: Preserve or normalize import grouping (`imports_granularity = "Crate"` or `"Item"`).
   - Reorder imports alphabetically (`reorder_imports = true`).

5. **Functions, Chains, & Closures**
   - Chain formatting: Clean indentation for iterator and builder method chains (`.filter().map()`).
   - Consistent placement of closure braces and argument lists.
   - Where clauses: Cleanly wrapped and indented under function signatures.

6. **Structs, Enums, & Attributes**
   - Trailing commas: Always require trailing commas on multi-line struct instantiations, enum variants, and match patterns.
   - Format attributes (`#[derive(...)]`, `#[inline]`) consistently.

7. **Comments**
   - Do not wrap or reflow doc comments (`wrap_comments = false`).
   - Preserve inline and block comments verbatim.

### Validation
- Verify `rustfmt --check` parses the config without unknown option warnings.
- Test on sample files with match expressions, generic traits, closures, and macros.
- Verify syntax with `cargo check` or `rustc --edition 2021`.
- Verify idempotency across consecutive formatting runs.
- Document any nightly-only options required vs. stable fallbacks.
```

---

### Template: Go (gofumpt / golangci-lint)

```markdown
I want you to configure formatting and linting for **Go (Go 1.22+)** development using **gofumpt** and **golangci-lint**.

### Goals
Enforce strict, idiomatic Go standards for microservices, network daemons, and game servers based on standard `gofmt` and enhanced `gofumpt` rules.

### Requirements

1. **Indentation & Formatting Engine**
   - Follow standard Go formatting conventions (tabs for indentation, spaces for alignment).
   - Enable `gofumpt` mode for stricter formatting (empty lines inside functions, composite literals, and control flow).

2. **Imports Management**
   - Enforce deterministic import ordering:
     1. Standard library imports.
     2. Third-party packages.
     3. Local / company module packages.
   - Use `goimports` / `gci` / `gofumpt` to prevent duplicate or messy import blocks.

3. **Structs & Composite Literals**
   - Consistent alignment of struct fields, types, and struct tags.
   - Require trailing commas in multi-line composite literals.
   - Consistent formatting for multi-line slice, map, and struct initializers.

4. **Functions & Error Handling**
   - Standard Go brace placement (opening `{` on the same line as `func`, `if`, `for`, `switch`).
   - Clean formatting of multi-line function signatures and return parameters.
   - Keep standard `if err != nil { return ... }` constructs clean and uncluttered.

5. **Comments**
   - Preserve all doc comments (`// Package ...`, `// Function ...`) verbatim.
   - Ensure comment indentation matches surrounding code blocks.

6. **Linter Integration (`.golangci.yml`)**
   - Configure relevant formatting linters: `gofmt`, `gofumpt`, `goimports`, `gci`, `whitespace`.
   - Prevent conflicting rules between linter auto-fixers.

### Validation
- Run `golangci-lint run` / `gofumpt -l -d` to verify configuration validity.
- Test formatting on representative Go files containing goroutines, channels, interfaces, and struct tags.
- Verify compilation with `go vet` and `go build`.
- Verify idempotency (zero diff on repeated formatting).
- Summarize changes and linter configurations.
```

---

### Template: C & Objective-C (Uncrustify)

```markdown
I want you to modify my existing `uncrustify-objc.cfg` configuration for **C and Objective-C (Objective-C v1/v2)** development using **Uncrustify**.

### Goals
Create a clean, consistent, professional coding style suitable for systems programming, networking, and game server development.

### Requirements

1. **Language & Extension Support**
   - Support C and Objective-C (including Objective-C v1 syntax like ivar blocks inside `@interface { ... }` and `@defs`).
   - Map extensions explicitly so headers are parsed as Objective-C (`file_ext OC .h .m .mm`).
   - Do not introduce rules that break valid Objective-C syntax.

2. **Indentation**
   - Use 4 spaces per level (`indent_columns = 4`, `input_tab_size = 4`, `output_tab_size = 4`).
   - Never use tabs for indentation (`indent_with_tabs = 0`, `indent_cmt_with_tabs = false`).
   - Continuation indent: 4 spaces (`indent_continue = 4`, `indent_param = 4`).
   - Switch case indentation: 4 spaces (`indent_switch_case = 4`, `indent_case_brace = 0`).
   - Class and interface bodies indented (`indent_class = true`, `indent_access_spec = 0`).
   - Objective-C block indentation enabled (`indent_oc_block = true`, `indent_oc_block_msg = 0`).

3. **Brace Placement (Differentiated Style)**
   - **C Functions & Control Flow**: Opening `{` on the **same line** (`nl_fdef_brace = ignore`, `nl_fdef_brace_cond = remove`, `nl_if_brace = remove`, `nl_for_brace = remove`, `nl_while_brace = remove`, `nl_switch_brace = remove`, `nl_do_brace = remove`, `nl_struct_brace = remove`).
   - **Objective-C Constructs**: Opening `{` on a **newline** for:
     - `@interface` ivar blocks (`nl_oc_interface_brace = force`).
     - `@implementation` ivar blocks (`nl_oc_implementation_brace = force`).
     - Method definitions (`nl_oc_mdef_brace = force`).
   - Blocks: Keep `{` on the same line for Objective-C blocks (`nl_oc_block_brace = remove`).
   - Attached constructs: `} else {`, `} while (...);`, `} @catch (...) {`, `} @finally {`.

4. **Spacing & Operators**
   - Binary operators: Space around arithmetic, assignment, boolean, comparison, and conditional operators.
   - Unary operators: No space around unary operators (`!`, `~`, `&`, `*`, `++`, `--`).
   - Keywords: Space after `if`, `for`, `while`, `switch`, `return`; no space after `sizeof`.
   - Parentheses: No unnecessary interior spaces inside parens, casts, or square brackets.
   - Pointers: Standard pointer formatting (`type *name`, `id *ptr`) with space before and no space after `*`.
   - Objective-C:
     - Space after scope indicator (`+` / `-`).
     - Space between return type and selector/method name (`- (type) name`).
     - No space before or after colons in message sends (`[obj method:arg]`).
     - Space between receiver and selector (`[receiver message]`).
     - Space before protocol list (`@interface Foo : Bar <Proto>`) and category (`@interface Foo (Cat)`).

5. **Line Breaks & Colons**
   - Code width limit: 100 columns (`code_width = 100`).
   - Long function signatures split fully at commas (`ls_func_split_full = true`).
   - Objective-C colon alignment:
     - Align parameters on colons in multi-line method declarations (`align_oc_decl_colon = true`).
     - Align parameters on colons in multi-line message sends (`align_oc_msg_colon_span = 1`, `align_oc_msg_colon_first = true`).
   - Chained conditions: Operators lead on continued lines (`pos_bool = lead`), commas trail (`pos_comma = trail`).

6. **Comments**
   - Preserve comments and their contents verbatim (`cmt_reflow_mode = 0`, `cmt_width = 0`).
   - Consistent `*` continuation in block comments (`cmt_star_cont = true`, `cmt_sp_before_star_cont = 1`).
   - Do not convert between C (`/* */`) and C++ (`//`) comment syntax.

7. **Preprocessor & Macros**
   - Pin `#` to column 1 (`pp_indent_at_level = false`).
   - Do not indent code inside `#if` / `#ifdef` guards (`pp_if_indent_code = false`).
   - Indent multi-line macro bodies by 4 spaces (`pp_multiline_define_body_indent = 4`).
   - Align macro continuation backslashes (`\`) at line ends (`align_nl_cont = 1`, `align_nl_cont_spaces = 1`).

### Validation
- Verify Uncrustify can parse the configuration without errors.
- Test on representative C and Objective-C files (structs, macros, ivars, categories, protocols, blocks).
- Verify syntax with `clang -fsyntax-only -x c` and `clang -fsyntax-only -fblocks -x objective-c`.
- Verify idempotency (pass 1 == pass 2).
- Summarize changes and document any unsupported rules.
```

---

## 3. Agent Validation & Verification Protocol

When directing an AI agent to update a code formatter configuration, append this verification checklist to guarantee reliable execution:

```markdown
### Verification Checklist for the Agent

Please execute and report the results of these verification steps before finishing:

1. [ ] **Config Syntax Check**: Formatter CLI parses configuration without errors or unknown option warnings.
2. [ ] **Language Extension Check**: Verify that target file extensions (e.g., `.h`, `.hpp`, `.rs`, `.go`) are recognized by the formatter.
3. [ ] **Representative Code Test**: Format test snippets covering:
   - Function & method declarations/definitions
   - Control flow constructs (`if/else`, loops, switches/matches)
   - Pointer / reference / type formatting
   - Structs, enums, classes, interfaces
   - Macro definitions & preprocessor continuations (if applicable)
4. [ ] **Compiler / Syntax Verification**: Run `clang -fsyntax-only`, `rustc`, `go vet`, or equivalent compiler check on the formatted code to guarantee no syntax corruption.
5. [ ] **Idempotency Test**: Run pass 1 into pass 2. Output of pass 1 must be identical to pass 2 (`diff` is empty).
6. [ ] **Change Log**: Summarize modified settings, rationale, and any formatting limitations inherent to the tool version.
```
