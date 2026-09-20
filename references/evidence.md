# Basis of the conventions

The skill is based on the author's instructions, CrazyPHP's documented headers, and source inspected across CrazyPHP, CrazyDemo, `rodeo_toolkit`, `rodeo_toolkit_2`, and `small_toolkit`. Reference projects are identified by name; no machine-specific installation paths belong in this skill.

Canonical library: [kekefreedog/CrazyPHP](https://github.com/kekefreedog/CrazyPHP). Locate a project's installed version through its manifests and dependencies. The initial inspection used library commit `18cdcd9`; inspect current files before relying on details that may have changed.

## Strong recurring patterns

- Four-space source indentation in PHP, TS, HBS, and authored SCSS; copied styles and some older regions differ.
- LF line endings; existing files are inconsistent about the final newline and trailing whitespace.
- Opening braces on the declaration/control line, including PHP classes. Existing source is not uniformly PSR-12 or Prettier output.
- Blank lines inside method bodies and between commented logical steps.
- PascalCase class names, camelCase variables and functions, uppercase constant names; private underscores are common but historically inconsistent.
- Most multi-import sections tend toward longest line first, with exceptions and interleaved asset loads.
- The language-specific headers and separator banners in `docs/Src/CommentHeaders.md`.

The inspection found no shared EditorConfig, Prettier, ESLint, PHP-CS-Fixer, PHPCS, or Stylelint configuration in the examined project-owned trees. Some files are much longer than 120 columns, especially HBS, so a universal hard line limit cannot be inferred from those files.

## Representative source locations

| Project         | Files or sections                                                                     | Useful evidence                                                                  |
| --------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| CrazyPHP        | `docs/Src/CommentHeaders.md`                                                          | Intended headers and separators                                                  |
| CrazyPHP        | `src/Library/Time/DateTime.php`, `src/Library/Array/Arrays.php`                       | PHP formatting, step comments, private/public groups, historical inconsistencies |
| CrazyPHP        | `src/Front/Library/Utility/Money.ts`, `Objects.ts`                                    | Arrow-function members, compact type annotations, method documentation           |
| CrazyPHP        | `src/Front/Library/Crazycomponent2.ts`                                                | Newer spacing and typing, class/member grouping, descriptive comments            |
| CrazyPHP        | `resources/Hbs/App/Partial/Partial.ts.hbs`                                            | Generated partial layout and lifecycle/member ordering                           |
| CrazyPHP        | `resources/Hbs/Partials/form/`, `resources/Scss/style/`                               | HBS attributes/comments and SCSS header/nesting conventions                      |
| CrazyDemo       | `app/Environment/Page/Request/`, `Filter/`, `app/Controller/App/Filter.php`           | Author's template-app conventions                                                |
| rodeo_toolkit   | `app/Front/`, `app/Environment/`, `app/Controller/`                                   | Established application conventions                                              |
| rodeo_toolkit_2 | `app/Environment/Partials/WorkstationCollection.ts`, `app/Environment/Page/Settings/` | Dependencies, private naming, lifecycle and template structure                   |
| small_toolkit   | `app/Controller/App/Project.php`, `app/Environment/Page/Project/index.ts`             | PHP fluent chains, aligned arrays, compact TS and logical-step spacing           |

Observed code is evidence, not proof that every inconsistency is intentional. User-confirmed choices in the skill take precedence. Retain generator markers, public names, required import ordering, template whitespace, and other behavior when applying formatting.
