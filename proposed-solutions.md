### 5. what are the proposed solutions?

#### 1. double parsing

Under this approach, the file would be parsed twice.
If an `import` or `export` statement is found, evaluate as an ES Module,
or Node Module if there are none.

This is not currently possible because the spec is ambiguous,
however it is possible that the spec may change in this regard.

#### 2. directives ("use module")

This approach is actually possible, given that import statements must be top-level. (That is, not within evaluated code blocks.)

#### 3. magic bytes ("js:;")

You write `js:;` at the beginning of a file to run as an ES Module.

This eliminates the _major_ problem with the directives approach.

#### 4. consumption defines type (no 1st class interop)

In this approach, `import` is only able to import ES Modules,
& `require()` only able to import Node Modules.

This also requires existing code to use something new & undecided to import
any ES Modules.

#### 5. `package.json`

You write external metadata in a `package.json` to determine file type.

```json
{
  "main": "index.js",
  "module": "module.js",
  "modules.root": "lib"
}
```

#### 6. extension (".mjs")

Use a new extension to have the file be run as an ES Module.

#### 7. don't implement
