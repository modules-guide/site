### 5. what are the proposed solutions?

#### 1. Double Parsing

Parse the file twice, accepting the first error-free result as the type of the
module. One could either parse the file as a Script first or as a Module first.

There are some issues with this approach:

* Tooling authors have to implement a parse step in order to determine file
  type. Not all languages have a JavaScript parser readily available.
* It is valid to have an ES module with no `import` or `export` statements,
  leading to the potential for ambiguity: if a file was intended to be
  a Node.js module but parsed as an ES module, it could result different
  `"use strict"` semantics being silently applied to the file, causing hard
  to reason about behavior.

#### 2. Directives (e.g., "use module")

Determine the module type of the file by the presence or absence of a `"use
module"` directive, defined and implemented *outside* of the JS specification.

There are some issues with this approach:

* A circular problem arises: in order to accurately determine whether to parse
  the file as an ES module or a Node.js module, one must first parse the file
  looking for directive statements *(Consider that a directive may be proceeded
  by any number of lines of whitespace or comments.)*
* All future ES modules would have to contain `"use modules"`.

#### 3. Magic Bytes (e.g., "js:;")

A variant of the `"use module"` approach, minus the parse step. Instead of
speculatively parsing the file to determine type, only examine the first four
bytes of the file (after stripping any `#!/usr/bin/env node` lines). If `js:;`
is present, the file will be parsed as an ES module, otherwise it will be a
Node module. If evaluated in a browser context, `js:;` will parse as a labeled
empty statement and be ignored.

* This eliminates the problem of having to speculatively parse, but retains
  (and exacerbates!) the problem of having to include an incantation at the top
  of every new ES module.

#### 4. Consumption defines type (no 1st class interop)

In this approach, only `import` may consume ES Modules, and only `require()`
may consume Node.js modules. The act of `import`-ing a file automatically
sets the type of the target file to ES module.

* If a file is consumed incorrectly, different `"use strict"` semantics may
  be erroneously applied to the target file, potentially resulting in
  hard-to-diagnose bugs that could spread to the rest of the program.
* Entry points still need an indicator of which module type they are.

#### 5. `package.json`

Metadata in an associated `package.json` determines whether a given file is a
Node.js module or an ES module. In particular, a `"module"` key is introduced
as an analogue of `"main"`. Packages containing only a `"module"` key will
automatically parse all contained files as ES modules. Packages that contain
both Node.js modules and ES modules may use a `"modules.root"` directive to
indicate that only modules under a certain path are ES modules.

```json
{
  "main": "index.js",
  "module": "module.js",
  "modules.root": "lib"
}
```

More can be read about this proposal
[here](https://github.com/dherman/defense-of-dot-js/blob/master/proposal.md).

* While this removes all ambiguity at parse time, it does so by tightly
  coupling files to a corresponding `package.json`. In order to determine
  the module type of a given file, one must examine every directory up from
  the file's location. For symlinked directories, what constitutes "up" may
  depend on how the file is being viewed.
* Moving a JS file from one directory to another may change how it is parsed
  without any other action being taken on the file, by removing it from a path
  including `package.json`.
* `"modules.root"` introduces an abstraction between `require('x/y')` that may
  result in `require('x/src/y')` being required, which may be difficult for
  beginners to follow.
* Care must be taken in implementing `"modules.root"` to avoid allowing the
  new package root to be set outside of the current package.
* It does not solve for standalone JS files.

#### 6. File Extension (".mjs")

Use a new extension to indicate whether a file is an ES module (`.mjs`) or a
Node.js module (`.js`).

* This requires the registration of a new MIME type.
* Some servers may not be configured to serve `.mjs` files.
* Frontend JavaScript authors may ignore the extension; thus creating an artificial
  barrier between ES modules written for the browser and ES modules written for Node.js.
* A social consideration: the `.js` extension is deeply loved by some users.

#### 7. Disambiguate Modules from Scripts

A variant of double parsing. This involves revising the JavaScript
specification to require at least one `import` or `export` statement in order
to parse a file as a module. This allows runtimes to double parse without fear
of accidental `"use strict"` ambiguity.

* It is unclear how browsers would implement this behavior.
* It is subject to the same tooling considerations as the double parsing solution.

#### 8. Don't Implement ES Modules

Node.js could opt out of implementing ES modules entirely.

* This is not ideal, as it potentially creates a rift between frontend module
  authors and Node.js module authors.
