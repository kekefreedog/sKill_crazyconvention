# TypeScript and JavaScript conventions

Apply the common profile in `SKILL.md`. TypeScript examples use the compact type spacing found in most of the inspected source. For file headers, member documentation, and class sections, use [Comments and organization](comments-and-organization.md).

## Names and module layout

- Use PascalCase for classes, new interfaces/type aliases, and class-module filenames. Preserve established lower-camel ambient framework types such as `stateEvent` instead of renaming public contracts.
- Use camelCase for functions, variables, parameters, and ordinary members: `projectCount`, `currentProject`, `readProjects`. New private TypeScript properties and methods use `_camelCase`, such as `_projectName` and `_loadProjects`; do not add underscores to ordinary local variables.
- Use `UPPER_SNAKE_CASE` for true shared constants, such as `DEFAULT_OPTIONS`; a local `const` variable remains camelCase. Framework static properties such as `className`, `html`, `template`, and `styles` keep their declared API names.
- Keep existing utility/function modules' lower-camel filenames. CrazyPHP page/component folders use PascalCase with `index.ts`, `template.hbs`, and `style.scss`; do not flatten or rename that layout as a style correction.
- Organize file header → dependencies → supporting types when needed → documented main class/function → required registration/export. Preserve compiler directives that must remain adjacent to their target statement.
- Match the target's export and loader contract. Existing HBS/SCSS `require(...)` calls and CommonJS helper exports are not automatically errors to convert to ES imports.

## Spacing, statements, and member forms

The compact profile uses `value:string`, `input:Record<string, unknown>`, `string|null`, and `if(condition){`. Keep spaces around assignment, binary operators, and the arrow token in a normal declaration: `public read = (name:string):string => {`.

Use one space after commas and no padding inside call/parameter parentheses. Default string quotes are double quotes; use template literals for interpolation and preserve existing literal semantics. Named imports commonly use `{Crazypage, State}` in the compact style; retain local brace padding in a file that deliberately uses the spaced form.

End imports, calls, assignments, and ordinary variable/property declarations with semicolons. The existing code often omits the optional semicolon after a class arrow-function member's closing brace; follow that surrounding member convention instead of treating it as a syntax defect.

Use a blank line after a method/arrow-body opening brace, before its closing brace, and between statements. Small object literals may stay inline when they remain clear.

Every statement has its own `//` comment directly above it, including statements inside callbacks with a block body. Every function that returns a value prepares `result` at the top and ends with its only `return result;`. This applies to block-bodied callbacks too. Only an expression-bodied arrow callback (`project => project.name`) may return implicitly. Do not use early returns, returns inside loops, or a bare `return;` in a `void` method. Assign and `break` instead (use a labeled `break` for nested loops), or wrap the remaining work in a condition. See "Mandatory rules" in `SKILL.md`.

Public/private arrow-function members are common in existing pages and utilities, especially when passed as callbacks. Prototype methods are also used in newer components. Preserve the required `this` binding, lifecycle signature, and override semantics; choosing an arrow field or prototype method is not purely formatting.

```ts
/**
 * Project Labels
 *
 * Read display labels from a collection of project records.
 */
export default class ProjectLabels {

    /** Public Methods
     ******************************************************
     */

    /**
     * Read Labels
     *
     * @param projects Project records to inspect
     * @return Display labels in their original order
     */
    public readLabels = (projects:Array<{name:string}>):string[] => {

        // Prepare result
        const result:string[] = [];

        // Read non-empty labels
        for(const project of projects){

            // Check project name
            if(project.name)

                // Push project name to result
                result.push(project.name);

        }

        // Return result
        return result;

    }

}
```

This example omits full file metadata for focus. In application code, use the real project's domain types rather than repeatedly spelling a shared record shape inline.

## Types and wrapping

Use explicit types where they clarify a public/member contract or a value starts empty. Let clear local initializers infer their types. New code should use primitive `string`, `number`, and `boolean` rather than boxed `String`, `Number`, or `Boolean`. Match an existing generic/interface contract instead of widening it to `any` solely to avoid a type error.

Use `const` for a binding that will not be reassigned and `let` when reassignment is needed. A mutable array/object can still use `const`. Existing `let` usage does not require a sweeping conversion.

Wrap long generic/signature/argument lists at structural boundaries. Put multiline object entries on separate indented lines, retain related key alignment when it helps, and preserve meaningful property order. A trailing comma is appropriate for newly authored multiline object/array literals; keep surrounding style when extending an existing block.

Keep dependency ordering and any runtime asset registration intact when wrapping imports. Avoid reordering a side-effecting import or `require(...)` in pursuit of a visual length ordering.
