# Comments and organization

The primary header reference is CrazyPHP's [CommentHeaders.md](https://github.com/kekefreedog/CrazyPHP/blob/main/docs/Src/CommentHeaders.md). Use its language-specific comment forms and the examples below; replace example titles and descriptions with the actual purpose of the code.

## Metadata

- Give new standalone source files a descriptive header. Retain existing attribution when editing a file.
- Use the target project's package identity for `@package`; `kzarshenas/crazyphp` is the library identity, not the identity of every unrelated application.
- The author's existing identity is `kekefreedog <kevin.zarshenas@gmail.com>` and copyright name is `Kévin Zarshenas`. Use that attribution for the author's code, not for third-party code being copied.
- For newly authored files, use the actual creation year. Preserve existing year ranges and update them only when appropriate to the change. Do not stamp every header with the historical `2022-2024` range from the documentation.
- A language/runtime version line describes the project's supported target. Keep it accurate when known; do not invent a version from the executing developer machine or copy an outdated version mechanically.
- Correct inaccurate descriptions, parameter tags, and return tags in code being changed. Misspellings, copied class names, `TBD`, debug greetings, and stale tags are not conventions to reproduce.

## PHP and TypeScript/JavaScript headers

Both use a `/** ... */` file header with one leading `*` per line, a title, a blank comment line, a concise description, and aligned metadata labels:

```text
/**
 * Project Collection
 *
 * Prepare project records for the collection view.
 *
 * @package    kzarshenas/crazyphp
 * @author     kekefreedog <kevin.zarshenas@gmail.com>
 * @copyright  2026 Kévin Zarshenas
 */
```

PHP files place their opening tag and strict-types declaration first, then the file header, namespace, dependency block, and documented class. An executable script's shebang stays first when present. A PHP version line, when specified, goes between the description and metadata as in the source documentation.

TypeScript/JavaScript files place the header before dependencies. Add a separate class/function description at the declaration where it explains the abstraction. Exported classes follow the existing class-header style; do not duplicate a long file description word for word.

## Member documentation and step comments

Document methods with a title derived from the action, a useful description when needed, then applicable parameter/return/exception tags. Put a blank comment line between those groups.

```text
/**
 * Read Project Labels
 *
 * Return labels in their original order, excluding blank entries.
 *
 * @param array $projects Project records to inspect
 * @return array
 */
```

That tag syntax is PHP-specific. In TS/JS, use the neighboring file's JSDoc/TSDoc convention and accurate parameter names; don't include PHP `$` prefixes. Code type annotations remain authoritative. Follow an established `@return` or `@returns` convention without rewriting unrelated comments.

Use concise one-line member descriptions such as `/** @var string $name Display name */` in PHP, and the existing JSDoc property style in TS. Use `@param` for parameters rather than inventing parameter tags for ordinary properties.

In PHP and TS/JS, **every statement is introduced by its own action comment**. This is mandatory, and the full rule and its exemptions are in `SKILL.md` under "Mandatory rules":

- PHP: `# Get project records`, `# Prepare result`, `# Check project name`, `# Continue`, `# Return result`.
- TS/JS: `// Get project records`, `// Prepare result`, `// Check project name`, `// Return result`.
- HBS: `{{!-- Project collection --}}` for each meaningful section, not for each line.
- SCSS: `// Component layout` for source-only notes, or existing `/* ... */` comments when emitted CSS comments are intended.

Each comment describes exactly one statement, and its wording must be accurate. Short mechanical phrases such as `# Set result` or `# Stop iteration` are the expected form, not noise to remove. Do not write one comment for a group of statements. Each comment and its single statement form a unit, separated from the next unit by one blank line:

```php
# Prepare result
$result = [];

# Get project records
$projects = $this->_readProjects();

# Return result
return $result;
```

Every function that returns a value ends with its only `return`, preceded by a comment such as `# Return result`. See "Return only at the end" in `SKILL.md`.

## Section separators

PHP and TS/JS use the documented banner form, indented with the code it groups:

```text
    /** Public Methods | Rendering
     ******************************************************
     */
```

Use meaningful groups such as `Public Parameters`, `Private Parameters`, `Public Static Methods`, `Public Methods`, `Private Methods | Init`, `Private Methods | Events`, and `Private Methods | Utilities`. Add sections only when there are members to organize.

The common class order is static metadata/constants and properties, instance properties, constructor, public/lifecycle methods, then private implementation groups. Some established PHP classes place a `Public Constants` section at the end. Keep the file's organization when editing it; use the common order for a new class and keep closely related methods together within a group.

File order is generally header → namespace where applicable → dependencies → supporting declarations → main class/function → required registration or export. Retain a CrazyPHP page's bottom-of-file registration and any initialization order.

Preserve literal section labels consumed by tooling, including partial/style registration sections. The source often spells the dependency heading `Dependances`; preserve it in an existing block. Use `Dependencies` in new blocks when no generator or local convention requires the existing spelling.

## Handlebars headers

Use HBS comments so the header does not become an HTML comment in the response:

```hbs
{{!
  ! Project Collection
  !
  ! Render the available project cards.
  !
  ! @package    kzarshenas/crazyphp
  ! @author     kekefreedog <kevin.zarshenas@gmail.com>
  ! @copyright  2026 Kévin Zarshenas
  !}}
```

The two spaces before `!` belong to the documented header decoration; they do not change the template's four-space nesting indentation. Include an accurate `HANDLEBARS.JS version ...` line if the project maintains that metadata. For small nested partials, retain the existing concise title-comment convention.

For a major HBS section, the documented expanded form is:

```hbs
{{!
  ! Project Actions
  !}}
```

## SCSS headers and documentation

```scss
/// /// ///
/// Project Collection
///
/// Styles for the project collection component.
///
/// @package    kzarshenas/crazyphp
/// @author     kekefreedog <kevin.zarshenas@gmail.com>
/// @copyright  2026 Kévin Zarshenas
/// /// ///
```

Use `///` documentation for functions/mixins with accurate Sass types, parameter names, and return descriptions. Major sections use:

```scss
/// Component Layout
///////////////////////////////////////////////////////
///
```

## YAML and JSON when accompanying the code

YAML uses the documented `# #` header boundary, `#` lines, metadata, and `#` section separators. Preserve its established indentation, commonly two spaces per nesting level. JSON cannot contain comment headers; do not add them. Follow the target configuration's existing schema and key order instead of applying source-code naming rules to data.
