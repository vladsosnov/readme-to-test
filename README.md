# readme-to-test

A simple README code examples validator.

## Introduction

The `readme-to-test` libary allows to easily validate code examples defined in the README file.

It finds all JavaScript blocks of code defined in target project's README file and converts them to unit tests.
Tests are run with Mocha and can be easily integrated with the build process (and Git's pre-commit hook).

## Installation

`npm install readme-to-test --save-dev`

## Command line 

The validation can be triggered by executing the following command in the target project's main directory:
```
node ./node_modules/readme-to-test/validate.js
```
The README file is automatically located. The validator creates a temporary directory (`.tmp` by default) and creates a separate self-contained unit test file for each block of JavaScript code found in the README. Then all the generated tests are run with Mocha. The directory is only deleted after a successful validation.

## Integrating with the build

The validate-readme-examples script can supplement the existing tests:

```
{
  "name": "library",
  "version": "1.0.0",
  "main": "main-script.js",
  "scripts": {
    "validate-readme-examples": "node ./node_modules/readme-to-test/validate.js",
    "test": "mocha unit/tests/ && npm run validate-readme-examples"
  }
}
```

## Examples

#### With console.log

The following block of code:

``` js
import library from 'library';

const result = library.someFunction();

console.log(result);
// prints 'result from some function';
```

will be converted into a unit test:

``` js
import assert from 'assert';
import library from './main-script.js';

const result = library.someFunction();

console.log(result);
assert.deepEqual(result, 'result from some function');
```

#### No console output

``` js
var library = require('library');
// library.version === '1.0'
```

``` js
var assert = require('assert');
var library = require('library');
assert.deepEqual(library.version, '1.0');
```

If imports are not used the generated tests are compatible with ES5.

# Configuration

The libary uses the following default settings:
```
--opts-file=test/r2t.opts --temp-dir=.tmp --temp-delete-after=true --temp-clear-before=true
```

The temporary folder settings can be overriden in `test/r2t.opts` file. 

# Post Scriptum

Yes, it validates its own README as well!
