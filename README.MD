Gives broccoli typescript powers. Incrementally.

# Why?
* **Bring Your Own TypeScript (>= 1.5 supported)**
  * https://github.com/shinnn/broccoli-typescript is two years old. I.e. Typescript 0.9.  
  * Install typescript in your project directory, we will use that version of typescript.
* **(Theoretical) Incremental build support.** This only works for serve and timepiece. CachePath persistence is not yet supported by broccoli between separate executions (see https://github.com/broccolijs/broccoli/issues/282).   
  * https://github.com/ngParty/broccoli-tsc and https://github.com/tildeio/broccoli-typescript-compiler does not support it either.
    * Incidentally, the latter has 17 contributors and has 91 lines of "actual" code.
* **Passthrough features.** If you want to preserve all files and the folder structure, this will do that.
  * Without passthrough, all non `.ts` files will be ignored.
  * Note that this is implemented by simply copying the folder first.

# Usage
First, install broccoli, then typescript.

    npm install broccoli-cli -g
    npm install typescript broccoli --save-dev

Next, create your `brocfile.js`.

    var compileTypescript = require('broccoli-typescript-incremental');

    var srcDir = 'your/path/to/src';
    var TypescriptTree = compileTypescript(srcDir, {
      sourceMap: true,
      passthrough: true
      //Any typescript compiler options go here.
      //See http://json.schemastore.org/tsconfig
    });

    module.exports = TypescriptTree;
Finally, run `broccoli`. Note that incremental build does not work here.

    broccoli build dist/

For incremental build, use `broccoli-timepiece` instead.

    broccoli-timepiece dist/

This puts a watch on the folder - rebuilding is done automatically and the cache is used.

# Limitations
All files used for compilation must be passed into the broccoli tree for compilation. In particular, take special note of the typescript definitions.

In particular, the "Could not find file in cache" error is related to this.

<!---
# Testing
There is a test project uploaded in github.com/blackening/broccoli-typescript-incremental-test. Currently all tests are done by eyeball.
-->

# Future work

*  **Manage file dependencies**.
When the build process gets sufficiently slow, to rewrite .ts dependencies and replace with preprocessed .ts.d. files which only get rebuilt when required.
