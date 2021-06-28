# Using SICP JS with the IDE of your choice

## NPM package `sicp`

The [Node.js NPM package `sicp`](https://www.npmjs.com/package/sicp) provides all predeclared functions and constants assumed by the [SICP JS textbook](https://sourceacademy.org/sicpjs). Consult the [README](https://www.npmjs.com/package/sicp) of the NPM package `sicp` for installation instructions.
The functions and constants are [documented here](https://docs.sourceacademy.org/source_4/global.html).

## Programs of SICP JS

The programs of SICP JS are available [as a zip file](https://sicp.sourceacademy.org/sicpjs.zip).

## Caveat

SICP JS relies on a JavaScript feature called "proper tail calls". This feature is specified by the JavaScript standards since 2015, but unfortunately as of 2021, most web browsers do not comply with this aspect of the standard. Node.js does not comply, either, and as a result, some programs in SICP JS will not scale when using the package `sicp` in Node.js in the way they should. This will not affect learners in a major way.

The [Soure Academy](https://sourceacademy.org) avoids the problem by transpiling the SICP JS sublanguage of JavaScript to JavaScript such that the resulting implementation performs proper tail calls regardless whether the underlying JavaScript implemementation has proper tail calls.
