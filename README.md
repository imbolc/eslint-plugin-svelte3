# eslint-plugin-svelte3

An ESLint plugin for Svelte v3 components.

## Features

- Svelte compiler errors and warnings are exposed as ESLint messages
- Variable references in your template or in store auto-subscriptions are handled when linting for unused variables
- Self-assignments are always allowed, as this is an official pattern for manually triggering reactive updates
- Unused labels called `$` are always allowed, as this is the syntax for reactive assignments
- Experimental opt-in linting of template expressions in addition to the script blocks

## Installation

Install the plugin package:

```
npm install eslint-plugin-svelte3
```

Then add `svelte3` to the `plugins` array in your `.eslintrc.*`.

For example:

```json
{
  "parserOptions": {
    "ecmaVersion": 2019,
    "sourceType": "module"
  },
  "env": {
    "es6": true,
    "browser": true
  },
  "plugins": [
    "svelte3"
  ],
  "rules": {
    "import/first": 0,
    "import/no-duplicates": 0,
  },
  "settings": {

  }
}
```

Also you'd probably want to add `/src/node_modules` to your `.eslintignore`

This plugin needs to be able to `require('svelte/compiler')`. If ESLint, this plugin, and Svelte are all installed locally in your project, this should not be a problem.

**Important!** Make sure you do not have `eslint-plugin-html` enabled on the files you want linted as Svelte components, as the two plugins won't get along.

## Configuration

Some settings work best with a function as their value, which is only possible using a CommonJS `.eslintrc.js` file, and not a JSON or YAML configuration file. Using `overrides` in the configuration file for specific globs will also give you more control over the configuration.

### `svelte3/enabled`

This can be `true` or `false` or a function that accepts a file path and returns whether this plugin should process it.

The default is to lint all files that end in `.svelte`. This can be changed by passing a new function, or by using ESLint `overrides` for this setting for specific globs.

### `svelte3/ignore-warnings`

This can be `true` or `false` or an array of Svelte compiler warning codes or a function that indicates whether to ignore it in the linting. The function will be passed two arguments - the warning code and the full warning object - and should return a boolean.

The default is to not ignore any warnings.

### `svelte3/compiler-options`

Most compiler options do not affect the validity of compiled components, but a couple of them can. If you are compiling to custom elements, or for some other reason need to control how the plugin compiles the components it's linting, you can use this setting.

This can be an object of compiler options or a function that accepts a file path and returns an object of compiler options.

The default is to compile with `{ generate: false }`.

### `svelte3/ignore-styles`

If you're using some sort of preprocessor on the component styles, then it's likely that when this plugin calls the Svelte compiler on your component, it will throw an exception. In a perfect world, this plugin would be able to apply the preprocessor to the component and then use source maps to translate any warnings back to the original source. In the current reality, however, you can instead simply disregard styles written in anything other than standard CSS. You won't get warnings about the styles from the linter, but your application will still use them (of course) and compiler warnings will still appear in your build logs.

This can be `true` or `false` or a function that accepts an object of attributes on a `<style>` tag (like that passed to a Svelte preprocessor) and returns whether to ignore the style block for the purposes of linting.

The default is to not ignore any styles.

### `svelte3/lint-template`

**Experimental! Requires at least Svelte 3.2.0!**

This can be `true` or `false` or a function that accepts a file path and returns whether this plugin should also lint its template expressions.

The default is to not lint any expressions in the template.

## Using the CLI

It's probably a good idea to make sure you can lint from the command line before proceeding with configuring your editor.

Using this with the command line `eslint` tool shouldn't require any special actions. Just remember that if you are running `eslint` on a directory, you need to pass it the `--ext` flag to tell it which nonstandard file extensions you want to lint.

## Integrations

See [INTEGRATIONS.md](INTEGRATIONS.md) for how to use this plugin with your text editor.

## License

[MIT](LICENSE)
