# Using SICP JS with the IDE of your choice

## NPM package `sicp`

The [Node.js NPM package `sicp`](https://www.npmjs.com/package/sicp) provides all predeclared functions and constants assumed by the [SICP JS textbook](https://source-academy.github.io/sicp). Consult the [README](https://www.npmjs.com/package/sicp) of the NPM package `sicp` for installation instructions.
The functions and constants are [documented here](https://source-academy.github.io/source/source_4/global.html).

## Programs of SICP JS

The programs of SICP JS are available [as a zip file](https://source-academy.github.io/sicp/sicpjs.zip).

## Caveat

SICP JS relies on a JavaScript feature called "proper tail calls". This feature is specified by the JavaScript standards since 2015, but unfortunately only the Safari browser complies with this aspect of the standard. Node.js does not comply, either, and as a result, some programs in SICP JS will not scale when using the package `sicp` in Node.js in the way they should. This will not affect learners in a major way.
