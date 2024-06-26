---
label: Developers
---

# Resources for developers

## Community

The Source Academy was conceived, designed, and implemented by a community of undergraduate students. See the [contributors page](https://sourceacademy.org/contributors) for the Hall of Fame, current project leadership, and all Github contributors.

## Current Development Activities

Currently (2023-2024) the following intiatives are under way to extend and improve
Source Academy. All of these are being pursued by Computer Science undergraduate
students of the National University of Singapore.

- Numerous improvements in the frontend and backend, including game support, grading, folders, visualization, share links, sessions, stories, contests, and mobile support
- Support for new languages: TypeScript, Scheme, Python, C, Java, C<span>#</span>

## Deployment

- The serverless frontend (GitHub repo `frontend`) is currently auto-deployed to [sourceacademy.org](https://sourceacademy.org).
- The JavaScript language implementations (GitHub repo `js-slang`) are (manually) published to [NPM js-slang](https://www.npmjs.com/package/js-slang). The NPM package `js-slang` is imported by the frontend. The implementations of
  - Python (GitHub repo `py-slang`) and
  - Scheme (GitHub repo `scm-slang`)

  are sub-modules of `js-slang`; they consist of frontends to `js-slang`.
- The C language implementations (GitHub repo `c-slang`) are (manually) published to [NPM @sourceacademy/c-slang](https://www.npmjs.com/package/@sourceacademy/c-slang). The NPM package `@sourceacademy/c-slang` is imported by the frontend.
- The Java language implementations (GitHub repo `java-slang`) are (manually) published to [NPM java-slang](https://www.npmjs.com/package/@sourceacademy/java-slang). The NPM package `@sourceacademy/java-slang` is imported by the frontend.
- The WebAssembly-based language implementation (GitHub repo `sourceror`) is (manually) published to [NPM sourceror](https://www.npmjs.com/package/sourceror).
- The sharing service for collaborative editing (GitHub repo `sharedb-ace`) is (manually) published to [NPM @sourceacademy/sharedb-ace](https://www.npmjs.com/package/@sourceacademy/sharedb-ace).
- The runtime component for Source on embedded systems (GitHub repo `sling`) is (manually) published to [NPM @sourceacademy/sling-client](https://www.npmjs.com/package/@sourceacademy/sling-client).
- The JavaScript module that goes with [SICP JS](https://sourceacademy.org/sicpjs/index) (GitHub repo `sicp`) is auto-deployed to [NPM sicp](https://www.npmjs.com/package/sicp)
- The [SICP JS](https://sourceacademy.org/sicpjs/index) textbook chapters, sections, and subsections (GitHub repo `sicp`) are auto-deployed to `sicp.sourceacademy.org/json`, e.g. [sicp.sourceacademy.org/json/1.json](https://sicp.sourceacademy.org/json/1.json).
- SourceAcademy @ NUS (including the backend, GitHub repo `backend`) is (manually) deployed to an AWS server at [sourceacademy.nus.edu.sg](https://sourceacademy.nus.edu.sg).
- The Source Academy modules (GitHub repo `modules`) are auto-deployed to [source-academy.github.io/modules](https://source-academy.github.io/modules/documentation/) via Github Pages. The following NPM packages are managed by the Source Academy team and imported in `modules`:
  - The package `source-academy-wabt` (GitHub repo `wabt`) is manually deployed to [source-academy-wab](https://www.npmjs.com/package/source-academy-wabt).
  - The package `saar` (GitHub repo `saar`) is manually deployed to [saar](https://www.npmjs.com/package/saar).
  - The package `nbody` (GitHub repo `nbody`) is manually deployed to [nbody](https://www.npmjs.com/package/nbody).
- This site (GitHub repo `general`) is auto-deployed to [about.sourceacademy.org](https://about.sourceacademy.org) via GitHub Pages.

## Get Involved

To get involved, feel free to propose fixes, improvements, or projects using the Issues feature in the [Source Academy repos](https://github.com/source-academy) or send an email to [source.academy.nus@gmail.com](mailto:source.academy.nus@gmail.com).

You can also follow [Source Academy on LinkedIn](https://www.linkedin.com/company/source-academy).
