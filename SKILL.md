---
name: crazyskill
description: Apply Kévin Zarshenas's coding conventions when writing, editing, formatting, or reviewing PHP, TypeScript, JavaScript, Handlebars, and SCSS in CrazyPHP, CrazyDemo, and related toolkit projects, or when explicitly requested for another project. Covers indentation, spacing, braces, blank lines, naming, comments and headers (a mandatory comment above every statement), single return at the end of functions, code organization, line wrapping, and import order. Use the crazyphp skill separately for framework behavior and architecture.
---

# CrazySkill

Apply the author's coding style without changing the behavior of the code. These conventions consolidate the documented CrazyPHP comment headers and recurring patterns across CrazyPHP, CrazyDemo, and the Rodeo/SmallCreative toolkits. [Evidence and source locations](references/evidence.md) distinguish recurring patterns from historical inconsistencies.

## Scope and precedence

- Use the user's explicit choices first, then applicable repository instructions and intentional formatter configuration. Use this profile for new code where those do not settle the choice.
- For an existing file, keep its surrounding conventions unless the user requests normalization or a confirmed standard requires the changed code to use a new style. Do not reformat unrelated regions.
- A request to apply coding conventions does not authorize API renaming, architecture changes, dependency migration, or changes to runtime behavior. Framework contracts remain authoritative for identifiers and signatures.
- Keep this skill portable. Refer to repositories and project-relative files, and discover checkout locations from the workspace/manifests. Do not write machine-specific installation paths into the skill.
- This skill defines code style. The companion `crazyphp` skill supplies framework workflows when relevant; it is not required to apply these conventions to another project explicitly chosen by the user.

## Common profile

The author confirmed the compact spacing, line-length, single-statement brace, longest-first import, private-naming, comment-per-statement, and single-return rules below. The other defaults consolidate the inspected source. Where the repositories do not establish a single convention, preserve an existing file and make a consistent choice for new code; do not claim a formatter enforces these rules.

| Area           | Convention                                                                                                                                                                                                                    |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Indentation    | Four spaces per source nesting level in PHP, TS/JS, HBS, and SCSS. Do not introduce tabs. YAML follows its own existing indentation, normally two spaces.                                                                     |
| Line endings   | LF. New files end with one newline. Avoid trailing spaces and whitespace-only blank lines. Preserve meaningful whitespace in literal/template content.                                                                        |
| Braces         | Opening brace on the same line as its declaration/control expression; closing brace aligned with that construct. PHP class braces also stay on the declaration line. Braces may be omitted for a single controlled statement. |
| PHP/TS spacing | Compact `if(condition){`, compact type annotations such as `value:string` / `):array`, and `}else{`. Keep spaces around assignments, binary operators, and normal arrow declarations.                                         |
| SCSS spacing   | `selector {`, `property: value;`. HBS keeps `{{expression}}` and `name="value"` compact.                                                                                                                                      |
| Blank lines    | One blank line around logical steps, between documented members, and inside nontrivial method bodies at the opening/closing boundaries. Do not reproduce runs of empty lines.                                                 |
| Line length    | No fixed maximum; wrap for readability. Break long signatures, arguments, conditions, arrays, and attributes at readable structural boundaries. Preserve literal/template content when wrapping would alter it.               |
| Imports        | Longest declaration to shortest within an independent dependency block; preserve equal-length order. Keep multiline declarations as units. Preserve side-effect and dependency ordering.                                      |
| Names          | PascalCase classes and new types; camelCase variables/functions; UPPER_SNAKE_CASE constants; kebab-case HTML/CSS names; snake_case CrazyPHP partial keys/files.                                                               |
| Private names  | Use `_camelCase` for new private properties and methods in both PHP and TypeScript: `$_projectName` / `_loadProjects()` in PHP, `_projectName` / `_loadProjects` in TS. Preserve existing contracts when editing older code.  |
| Strings        | Double quotes by default in new PHP/TS/JS and HTML attributes. Preserve interpolation/escaping behavior and intentional local conventions.                                                                                    |
| Comments       | Documented file/class/method headers and section banners. **Every PHP/TS/JS statement gets its own comment line directly above it** (see below). PHP uses `#`, TS/JS `//`, HBS `{{!-- --}}`, SCSS `///` for documentation.     |
| Returns        | **One `return`, as the last statement of the function/method**, returning a `$result` / `result` variable prepared at the top. No early returns or guard-clause returns (see below).                                       |

LF, a final newline, and removal of incidental trailing whitespace are normalization defaults for new or changed source; the original files are not uniformly clean in these respects.

## Mandatory rules: statement comments and single return

These two rules are strict. Apply them to all new PHP and TS/JS code, and to every function or method you create or change. When writing new code, do not treat them as optional or as a matter of local preference.

### A comment above every statement

- Every executable statement has its own comment line immediately above it: assignments, calls, `return`, `continue`, `break`, `throw`, `yield`, `echo`, and every control header (`if`, `foreach`, `for`, `while`, `switch`, `try`). Inside a `switch`, every `case` body follows the same rule.
- One comment covers one statement, never a group of statements. A statement that spans several lines, such as an array, a fluent chain, or a wrapped call, gets one comment above its first line.
- The comment is a short English action phrase in sentence case: `# Prepare result`, `# Check project name`, `# Set result`, `# Stop iteration`, `# Return result`. Short phrases are the established form even when they seem obvious. The comment must match what the line does; fix a stale comment when you change the line.
- After each comment and its statement, leave one blank line before the next comment. In a brace-less one-statement `if`/loop, the body's comment goes on the indented line above the body (see the language examples).
- Exempt lines: opening and closing braces, `}else{` / `}else if(...){` lines (their bodies carry the comments), blank lines, declarations documented by a docblock (file, class, property, method, constant), `namespace` and `use`/`import` lines inside a documented dependency block, and `declare(strict_types=1)`.
- HBS and SCSS keep their section and step comments as described in their reference. Do not comment every markup line or CSS declaration.

### Return only at the end

- A function or method that returns a value has exactly one `return`, and it is the last statement of the body. Declare the result variable at the top with its default (`# Prepare result` / `$result = null;`). Then assign it in branches and return it at the end (`# Return result`).
- Do not use early returns, guard-clause returns, or a `return` inside a loop. Turn a guard into an `if`/`else` that assigns the result or wraps the rest of the body. Turn a return inside a loop into an assignment followed by `break` (PHP `break 2` or a TS labeled `break` for nested loops), so that the first-match behavior stays the same.
- `void` functions do not use a bare `return;` to exit early. Wrap the remaining work in a condition instead.
- `throw` is not a return and stays where the error is detected, with its own comment.
- Allowed exceptions: an arrow callback whose expression body is the value (`item => item.id`, `fn($item) => $item["id"]`); generators, where `yield` is not a return; and a framework contract that requires another shape. Only if the single-exit form truly cannot be written without changing behavior, keep the early return and add a comment that explains why.
- Restructuring must preserve behavior: code after a former early return must still not run, side effects must keep their order, and `finally` blocks must keep their semantics. Apply the rule to code you write or change. Restructure untouched existing functions only when the user asks for normalization.

## Use the relevant language reference

- [PHP](references/php.md): namespace/use layout, declarations, names, arrays, fluent chains, and a class example.
- [TypeScript and JavaScript](references/typescript-and-javascript.md): imports, types, arrow/prototype members, callbacks, and a class example.
- [Handlebars and SCSS](references/hbs-and-scss.md): template nesting and attributes, selectors, declaration spacing, Sass naming and ordering.
- [Comments and organization](references/comments-and-organization.md): exact header/separator forms, attribution, member documentation, section ordering, and YAML/JSON exceptions.

## Important formatting boundaries

- A one-statement conditional or loop may omit braces in the established compact style. Put its body on the next indented line. Use braces for multiple statements, ambiguous nesting, or an `else` whose ownership would be unclear. Comments do not count as statements. Do not create an empty-body `if(...);` accidentally.
- Import order is a visual convention, not a reason to change initialization order. Sort PHP `use` declarations or independent imports only where safe; leave order-sensitive imports, `require(...)`, CSS loads, polyfills, and registrations in their required order. Measure the declaration as a whole, not individual wrapped lines; do not alphabetize specifiers without an existing rule.
- Do not force alphabetical member, attribute, object-key, or CSS-property order. Keep related operations together and preserve order that affects runtime, serialization, rendering, or the cascade.
- Keep method/reference names required by frameworks, routes, reflected properties, and external schemas. Fixing capitalization or adding a private underscore can be a breaking change.
- Preserve HBS inline spacing, escaping, attribute conditionals, and helper context. Preserve SCSS import/cascade order and TS callback `this` binding.
- Do not blindly run a default Prettier or PHP formatter over these projects: its output may differ from the selected conventions. Use a project's configured formatter when applicable; adding a formatter or changing its rules is a separate task.

## Review and verify

For a conventions review, always check the two mandatory rules. Report every PHP/TS/JS statement that has no comment directly above it, every comment that covers more than one statement, and every `return` that is not the last statement of its function. Identify concrete deviations with file locations, the applicable rule, and the minimal correction. Distinguish stylistic inconsistencies from correctness problems and from a deliberate legacy convention.

For edits, check before finishing that every new or changed statement has its own comment and that each changed function ends with its only `return`. Inspect the diff for unintended reordering or semantic changes. Use an appropriate syntax/type/render check when the change can affect parsing or behavior. Do not add tests that merely match indentation or comment wording. Summarize the applied conventions and any checks actually run.
