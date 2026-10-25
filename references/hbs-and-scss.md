# Handlebars and SCSS

Use the common indentation and line-wrapping policy in `SKILL.md`, and the language-specific headers in [Comments and organization](comments-and-organization.md).

## Handlebars structure

- Indent nested HTML elements and multiline Handlebars blocks by four spaces. Keep `{{else}}` and a block's closing tag aligned with its opening block.
- Keep helpers compact inside their delimiters: `{{label}}`, `{{#if enabled}}`, `{{> project_card state=project}}`. Separate helper arguments with one space. Do not add spaces around the `=` in HTML attributes or named HBS arguments.
- Use double quotes for HTML attribute values. Preserve the casing and spelling of component attributes, data keys, partial names, and helper names.
- Use blank lines and `{{!-- ... --}}` comments to distinguish meaningful sections: root, navigation, header, body, collection, actions. Do not add a blank line between every tiny text fragment or inline element.
- Short elements and simple text substitutions can remain on one line. Split long opening tags across lines, normally one attribute or cohesive conditional attribute per line. Indent attributes one level; align the closing `>` with the opening tag's indentation.
- Preserve an existing meaningful attribute order. Place related attributes together when authoring new markup; alphabetical attribute sorting is not an established rule.
- Preserve inline conditionals when they are part of one attribute's value. Moving them or inserting newlines can change class names or text. A whitespace-sensitive template takes priority over cosmetic wrapping.
- Keep standard HTML void elements consistent with their template; do not apply JSX syntax rules to HBS.

Example body (a full file also receives its appropriate header):

```hbs
{{!-- Project collection --}}
<section
    id="project-collection"
    class="project-collection{{#if compact}} project-collection--compact{{/if}}"
    aria-label="Projects"
>

    {{#each projects}}
        {{!-- Project card --}}
        <article class="project-card" data-project-id="{{id}}">
            <h2>{{name}}</h2>
        </article>
    {{else}}
        <p>No projects available.</p>
    {{/each}}

</section>
```

Use snake_case for CrazyPHP partial filenames and registration keys (`project_card.hbs`, `project_card`). Component tags and ordinary CSS classes/HTML IDs use kebab-case (`project-card`). Keep an established legacy partial name or framework identifier as-is. A page/component's co-located template remains `template.hbs`.

HBS comments do not appear in rendered HTML. Use HTML comments only when they are intentionally part of the output. Preserve `@root`, `../`, named arguments, whitespace-control markers, and normal versus triple braces. A formatting change must not change escaping or expression scope.

## SCSS structure

- Use four-space nesting, opening braces on the same line with one preceding space, and one declaration per line. Align closing braces with the selector/directive.
- Use `property: value;`: no space before the colon, one after it, and a terminating semicolon even on the last declaration. Keep units attached to numeric values.
- Keep short related declarations together. Separate larger selector blocks and conceptual groups with blank lines. Multi-section functions/mixins follow the same logical-step spacing as the rest of the code.
- Wrap a selector list one selector per line when it becomes long. Keep interpolation and custom-property values intact when wrapping values.
- Use the documented `///` file headers and section banners. Retain comments that explain component states, generated utilities, or framework integration.
- Keep top-level dependencies before the rules that use them, preserving dependency and cascade order. Do not alphabetically sort Sass imports or convert `@import` to `@use` as a formatting change.
- Group base declarations, modifiers/states, nested elements, and responsive rules coherently. Preserve the ordering of duplicate properties, fallbacks, and overlapping rules; declaration order can affect output.

```scss
$project-card-gap: 1rem;

/// Project Cards
///////////////////////////////////////////////////////
///

.project-collection {
    display: grid;
    gap: $project-card-gap;

    &--compact {
        gap: 0.5rem;
    }
}

.project-card {
    padding: 1rem;
    border: 1px solid currentColor;

    h2 {
        margin: 0;
        font-weight: 600;
    }
}
```

Use descriptive kebab-case CSS selectors and new Sass helper names. Existing SCSS includes both hyphenated and underscored variables; follow the module's vocabulary instead of renaming them wholesale. A BEM-style `__element` / `--modifier` is appropriate where the component uses it, but BEM is not a requirement for all legacy styles.

Preserve the project's filename mapping: CrazyPHP page/component styles are `style.scss`; shared partial styles commonly use `_project_card.scss`. The filename's leading underscore has a Sass-module meaning, independent of private PHP/TS naming.
