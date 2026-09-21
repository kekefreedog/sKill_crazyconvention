# PHP conventions

Apply the spacing, brace, import, and private-naming choices in `SKILL.md`. The examples use the compact style found in most of the inspected source. Use [Comments and organization](comments-and-organization.md) for complete headers and section banners.

## Files, names, and declarations

- Begin new ordinary PHP source files with `<?php declare(strict_types=1);`, then the file header. Preserve an existing entrypoint's required shebang or boot behavior. Pure PHP files do not need a closing `?>`.
- Place the namespace after the file header, then the dependency block, then the documented class. Use explicit `use` declarations and meaningful aliases where necessary.
- Match PascalCase class names to their PSR-4 filenames and namespace directories. Use camelCase for local variables, parameters, and ordinary methods: `$projectCount`, `$currentProject`, `readProjects()`.
- Use `UPPER_SNAKE_CASE` for class constants, such as `DEFAULT_OPTIONS`. New private properties and methods use `_camelCase`, such as `private string $_projectName` and `private function _loadProjects()`. The underscore follows `$` in a property name; ordinary local variables remain camelCase.
- Keep framework methods, inherited signatures, magic methods, serialized keys, database columns, and external field names unchanged. For example, do not rename CrazyPHP's `State()` helper to satisfy a camelCase preference.
- Declare visibility and useful parameter/return types consistent with the supported runtime and interface. Changing a public signature or introducing strict types to an existing file is a behavioral change, not a formatting cleanup.

## Spacing and blocks

Compact source uses forms such as `if($enabled){`, `foreach($items as $item){`, and `}else{`. Named typed methods commonly use `public function read(string $name):array {`: no padding inside parentheses, compact return type, and a space before the body brace after the return type. Constructors without a return type commonly use `__construct(...){`. Classes commonly use `class Project {`.

Keep four spaces per block. Use a blank line after opening a method body and before its closing brace; separate commented logical steps by one blank line. Avoid turning existing double or whitespace-filled blank lines into a prescribed style.

Use one space after commas and around assignments and binary operators. Keep member access tight: `$item->name`, `ClassName::method()`. Default to double-quoted strings when no existing local convention dictates otherwise; single quotes can keep interpolation literal or avoid unnecessary escapes. Preserve the behavior of existing literals.

Align related associative-array `=>` operators for readability, using spaces. Do not vertically align unrelated declarations across a whole class. Use short arrays and multiline arrays when entries need their own comments or become hard to scan. Preserve the surrounding trailing-comma style; a trailing comma is appropriate for a newly authored multiline array.

```php
<?php declare(strict_types=1);
/**
 * Name Space
 *
 * Objectif of the name space
 *
 * PHP version 8.#.#
 *
 * @package    author/package
 * @author     Author <author@mail.com>
 * @copyright  2022-202# Author
 */
namespace App\Library;

/**
 * Dependances
 */

/**
 * Project Labels
 *
 * Build the labels shown in a project collection.
 */
class ProjectLabels {

    /** Public Static Methods
     ******************************************************
     */

    /**
     * Read Labels
     *
     * @param array $projects Project records to inspect
     * @return array
     */
    public static function readLabels(array $projects):array {

        # Prepare result
        $result = [];

        # Read available labels
        foreach($projects as $project){

            # Ignore records without a label
            if(!isset($project["name"])){

                # Continue
                continue;

            }

            # Add the project label
            $result[] = [
                "id"    => $project["id"],
                "label" => $project["name"],
            ];

        }

        # Return result
        return $result;

    }

}
```

This example omits the full file metadata for focus; standalone source files use the documented file header.

## Wrapping expressions

For a long condition or parameter list, put each logical operand/argument on its own indented line and align the closing delimiter with the start of the statement. Keep logical operators in a consistent position. Do not split a literal value merely to satisfy a column target.

Fluent calls often place one call per line and the terminating semicolon on a separate line aligned with the assignment. Preserve this established layout when extending such a chain. A short call need not become a multiline chain.

Multiline ternaries place `?` and `:` on their continuation lines. Align related branches and preserve parentheses that determine precedence. Do not replace an existing condition with a ternary or short-circuit expression just to make it shorter.

## PHPUnit tests

- Match the neighboring test files: descriptive file and class headers with package/author/copyright metadata, title-style method summaries such as `Test Set Body Email Template`, and the `Public method | Preparation` / `Public method | Tests` section banners where applicable.
- Put fixture/environment setup and cleanup in PHPUnit lifecycle methods when this keeps the test focused on inputs, execution, and assertions. Preserve isolation for global state and caches; use temporary fixture directories rather than application data.
- Separate logical steps with blank lines and concise `#` action comments, such as `# Set input`, `# Get result`, and `# Assert rendered content`. Do not copy stale descriptions or misspellings from older tests.
- Narrow DOM query results before using element-only APIs. `DOMNodeList::item()` can return a `DOMNode` or null; check `instanceof DOMElement` and fail the test if needed before accessing `hasAttribute()`, `getAttribute()`, or `childElementCount`. Prefer a real type guard to suppressing Intelephense diagnostics or asserting an unchecked PHPDoc type.
