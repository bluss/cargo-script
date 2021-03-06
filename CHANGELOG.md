
# v0.2.0

**Changed:**

* `cargo-script` now requires Rust 1.6 to build.

* Removed prefix manifests.  All other forms are still supported.

# v0.1.5

**New:**

* `cargo-script` now (typically) defaults to using a shared build location.  This means that, provided you're using the same compiler, dependencies won't need to be constantly re-built.  This can be explicitly controlled by setting the `--use-shared-binary-cache` option to `yes` or `no`.

* Added `--unstable-feature` option.  This allows you to specify unstable language features that should be enabled.

**Changed:**

* The build cache will be retained when the build fails.  This means that dependencies don't have to be re-built when the script itself has an error in it.

**Fixed:**

* Fixed issue with evaluating an expression containing macros that capture.

# v0.1.4

**New:**

* Added `--features` option.  This allows you to specify the features that should be enabled.

**Fixed:**

* `cargo-script` should now build with Rust 1.4 stable or higher.  Rust 1.3 is incompatible due to the breaking change to the behaviour of `str::lines`.

# v0.1.3

**New:**

* `run-cargo-script` trampoline program for use with hashbangs.

**Changed:**

* On UNIX, `cargo-script` will now try to place its cache in `$CARGO_HOME`, falling back to `$HOME`.  Behaviour on Windows is unchanged.

* Various internal fixes and updates.

# v0.1.2

**New:**

* Added `-e` and `-l` as shorthands for `--expr` and `--loop`.

* Added `--dep-extern`/`-D`.  This introduces a dependency and causes an appropriate `#[macro_use] extern crate $name;` item to be added.  This only applies to expression and loop scripts.

* Added `--extern`/`-x`.  This adds a explicit `#[macro_use] extern crate $name` item.  This only applies to expression and loop scripts.

# v0.1.1

**New:**

* Not-Windows support, contributed by @Ryman.

* Added support for two new embedded manifest formats: short comment manifests and code block manifests.  Compared to prefix manifests, these have the advantage of allowing scripts to be valid Rust code, as well as having a measure of self-documentation.

* You can now pass arguments to scripts.  If you want to pass switches, you'll need to add `--` after the script name so `cargo script` knows to stop looking for switches.

* Added the `--clear-cache` flag.  This deletes all cached scripts.  It can be provided by itself or as part of a regular invocation.

* Added the `--pkg-path PATH` flag.  This lets you specify where to create the Cargo package.

* Added the `--gen-pkg-only` flag.  This causes `cargo script` to output a Cargo package, but not attempt to build or run it.

**Changed:**

* Expressions and loop closures are now wrapped in blocks, rather than an expression.  This means you can have multiple statements, link to crates, use things, etc. without having to define a block yourself.

* Expressions now have their output displayed using the `{:?}` format specifier.  This allows more types to work without extra effort, but does make strings a bit ugly.

* Changed to a not-as-slow argument parser.

**Removed:**

* Removed content hashing for scripts.  No longer will even minor changes cause Cargo to go back and re-fetch and re-compile all dependencies!  Scripts are cached based on a hash of their absolute path, instead.  Expressions and loop closures are still cached based on a content hash.
