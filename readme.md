# emphasize

[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]

ANSI syntax highlighting in for your terminal.
Like [highlight.js][hljs] (through [lowlight][]).

`emphasize` supports [all 191 syntaxes][names] of [highlight.js][hljs].
There are three builds of `emphasize`:

*   `lib/core.js` — 0 languages
*   `lib/common.js` (default) — 35 languages
*   `lib/all.js` — 191 languages

## Install

This package is [ESM only](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c):
Node 12+ is needed to use it and it must be `import`ed instead of `require`d.

[npm][]:

```sh
npm install emphasize
```

## Use

Say `example.css` looks as follows:

```css
@font-face {
  font-family: Alpha;
  src: url('Bravo.otf');
}

body, .charlie, #delta {
  color: #bada55;
  background-color: rgba(33, 33, 33, 0.33);
  font-family: "Alpha", sans-serif;
}

@import url(echo.css);

@media print {
  a[href^=http]::after {
    content: attr(href)
  }
}
```

And `example.js` contains the following:

```js
import fs from 'fs'
import {emphasize} from 'emphasize'

const doc = String(fs.readFileSync('example.css'))

const output = emphasize.highlightAuto(doc).value

console.log(output)
```

Now, running `node example` yields:

```txt
@\u001b[32mfont-face\u001b[39m {
  \u001b[33mfont-family\u001b[39m: Alpha;
  \u001b[33msrc\u001b[39m: \u001b[31murl\u001b[39m(\u001b[36m'Bravo.otf'\u001b[39m);
}

\u001b[32mbody\u001b[39m, \u001b[34m.charlie\u001b[39m, \u001b[34m#delta\u001b[39m {
  \u001b[33mcolor\u001b[39m: \u001b[36m#bada55\u001b[39m;
  \u001b[33mbackground-color\u001b[39m: \u001b[31mrgba\u001b[39m(33, 33, 33, 0.33);
  \u001b[33mfont-family\u001b[39m: \u001b[36m\"Alpha\"\u001b[39m, sans-serif;
}

@\u001b[32mimport\u001b[39m url(echo.css);

@\u001b[32mmedia\u001b[39m print {
  \u001b[32ma\u001b[39m\u001b[35m[href^=http]\u001b[39m\u001b[35m::after\u001b[39m {
    \u001b[33mcontent\u001b[39m: \u001b[31mattr\u001b[39m(href)
  }
}
```

And looks as follows:

![Screenshot showing the code in terminal](screenshot.png)

## API

This package exports the following identifiers: `emphasize`.
There is no default export.

### `emphasize.registerLanguage(name, syntax)`

Register a syntax.
Like [`low.registerLanguage()`][register-language].

### `emphasize.highlight(language, value[, sheet])`

Highlight `value` as a `language` grammar.
Like [`low.highlight()`][highlight], but the return object’s `value` property is
a string instead of a hast root.

You can pass in a `sheet` ([`Sheet?`][sheet], optional) to configure the theme.

### `emphasize.highlightAuto(value[, sheet | options])`

Highlight `value` by guessing its grammar.
Like [`low.highlightAuto()`][highlight-auto], but the return object’s `value`
property is a string instead of a hast root.

You can pass in a `sheet` ([`Sheet?`][sheet], optional) directly or as
`options.sheet` to configure the theme.

### `Sheet`

A sheet is an object mapping [highlight.js classes][classes] to functions.
The `hljs-` prefix must not be used in those classes.
The “descendant selector” (a space) is supported.

Those functions receive a value (`string`), which they should wrap in ANSI
sequences and return.
For convenience, [chalk’s chaining of styles][styles] is suggested.

An abbreviated example is as follows:

```js
{
  'comment': chalk.gray,
  'meta meta-string': chalk.cyan,
  'meta keyword': chalk.magenta,
  'emphasis': chalk.italic,
  'strong': chalk.bold,
  'formula': chalk.inverse
}
```

## Emphasize in the browser

If you’re using `emphasize/lib/core.js`, no syntaxes are included.
Some syntaxes are included if you import `emphasize` (or
`emphasize/lib/common.js`).
All syntaxes are available through `emphasize/lib/all.js`

See [Syntaxes in `lowlight`][syntaxes] for which syntaxes are included where.

## License

[MIT][license] © [Titus Wormer][author]

<!-- Definitions -->

[build-badge]: https://github.com/wooorm/emphasize/workflows/main/badge.svg

[build]: https://github.com/wooorm/emphasize/actions

[coverage-badge]: https://img.shields.io/codecov/c/github/wooorm/emphasize.svg

[coverage]: https://codecov.io/github/wooorm/emphasize

[downloads-badge]: https://img.shields.io/npm/dm/emphasize.svg

[downloads]: https://www.npmjs.com/package/emphasize

[size-badge]: https://img.shields.io/bundlephobia/minzip/emphasize.svg

[size]: https://bundlephobia.com/result?p=emphasize

[npm]: https://docs.npmjs.com/cli/install

[license]: license

[author]: https://wooorm.com

[sheet]: #sheet

[hljs]: https://github.com/highlightjs/highlight.js

[lowlight]: https://github.com/wooorm/lowlight

[names]: https://github.com/highlightjs/highlight.js/blob/master/SUPPORTED_LANGUAGES.md

[classes]: https://highlightjs.readthedocs.io/en/latest/css-classes-reference.html

[styles]: https://github.com/chalk/chalk#styles

[register-language]: https://github.com/wooorm/lowlight#lowregisterlanguagename-syntax

[syntaxes]: https://github.com/wooorm/lowlight#syntaxes

[highlight]: https://github.com/wooorm/lowlight#lowhighlightlanguage-value-options

[highlight-auto]: https://github.com/wooorm/lowlight#lowhighlightautovalue-options
