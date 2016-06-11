<div id="goals"><h3>What are our goals?</h3></div>

Not all of these are requirements, but they are goals that have come from
various stakeholders.

#### 1. Backwards compatibility

There are hundreds of thousands of packages that can be installed from npm
today. Those packages should continue to work without modification once ES
modules have been added to the Node platform.

#### 2. Tooling & workflow integration

(Also see <a href="#stakeholders">stakeholders</a>, above.)

Modern web development workflows depend on a broad and diverse array of tools,
many of which are meant to edit, bundle, optimize, or otherwise manage
JavaScript. It should be able to integrate ES modules into this workflow, in a
way that doesn't overly complicate existing workflows.

#### 3. Error clarity

In situations where there's the possibility of ambiguity (e.g. trying to use an
ES module in a Node module context) or operational error, the error messages
coming out of Node should be easily understandable and actionable.

#### 4. Should be easily teachable

The rules for using ES modules, alone or in combination with Node modules,
should be explicit and unambiguous to make them straightforward to teach and
easy to learn.

#### 5. Prefer "1st class interoperability"

It would be nice to be able to use ES modules and Node modules interchangeably.
Put another way, it should be possible to migrate from Node modules to ES
modules without breaking things along the way.

#### 6. Don't fragment JS between browsers and node

The implementation of ES modules should work identically in browsers and Node.

#### 7. Moving files should still work

You should be able to move one module from a package to another place without
having to change it to anything – files shouldn't be coupled to other files.

#### 8. `package.json`-less files

It should be possible to start work on a new module without having to build the
infrastructure required to package or shipping it. Prototyping should remain
fast and painless!

#### 9. Startup time

More complicated Node applications are made of hundreds or thousands of
modules. Loading and parsing those files needs to be as fast as possible so
that services and applications boot quickly.
