# game-default-formatters

Default code formatting configurations for game server, networking, and system programming development.

## Configurations

### `uncrustify-objc.cfg`
An [Uncrustify](https://github.com/uncrustify/uncrustify) (0.78+) configuration tailored for **C** and **Objective-C** (including Objective-C v1 syntax).

#### Style Summary
* **Indentation**: 4 spaces, never tabs. 4-space continuation indent and switch-case indent.
* **Brace Placement**:
  * **C Functions & Control Flow**: Opening `{` on the **same line** (K&R / 1TBS) for C functions, conditionals (`if`/`else`), loops (`for`, `while`, `do`), and compound blocks.
  * **Objective-C Constructs**: Opening `{` on a **new line** for `@interface` ivars, `@implementation` ivars, and method definitions (`-` and `+`).
* **Objective-C Spacing & Alignment**:
  * Space after scope (`-` / `+`) and space after return type: `- (type) name;`.
  * Multi-line method declarations and message calls aligned on colons (`:`).
  * Auto-detects `.h`, `.m`, and `.mm` as Objective-C.
* **Pointers**: Pointer asterisk attached to variable name (`type *name`, `id *ptr`) with space before and no space after.
* **Preprocessor**: `#` pinned to column 1; multi-line macro continuation backslashes (`\`) aligned cleanly at line ends; macro body indented 4 spaces.
* **Comments**: Comments preserved verbatim without reflowing.

#### Usage

Format a file in place:
```bash
uncrustify -c uncrustify-objc.cfg --replace --no-backup <file>
```

Check formatting without modifying:
```bash
uncrustify -c uncrustify-objc.cfg --check <file>
```
