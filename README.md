# Filehound

[![Build Status](https://travis-ci.org/nspragg/filehound.svg)](https://travis-ci.org/nspragg/filehound)

> Flexible and fluent interface for searching the file system

## Installation

```
npm install --save filehound
```

## Features

* Flexible search filters
* Simple fluent interface
* Ability to combine search results from multiple queries
* Supports promises and callbacks

## Usage

```js
var FileHound = require('filehound').create()

const files = FileHound.create()
  .path('/some/dir')
  .ext('json')
  .find();

files.then(console.log); // prints list of json files found

const files = FileHound.create()
  .path('/some/dir')
  .ext('txt')
  .size('>1024')
  .find();

files.then(console.log); // prints list of text files larger than 1024 bytes

const files = FileHound.create()
  .path('/etc/pki/')
  .match('dev*')
  .ext('pem')
  .find();

files.then(console.log); // prints list of pem files starting with 'dev'

const notJsonFiles = FileHound.create()
  .ext('json')
  .not()
  .find();

notJsonFiles.then(console.log) // prints all files except json

const overOneMB = FileHound.create()
  .path('/some/dir')
  .size('>1024')
  .find();

const json = FileHound.create()
  .path('/some/dir')
  .ext('json')
  .find();

const files = FileHound.any(overOneMB, json);

files.then(console.log); // prints files that are either over 1MB or have a .json extension
```

## API

### Static methods

### `FileHound.create() -> FileHound`

##### Parameters
* `opts` - _optional_ - Object contains configuration options
  * debug - display search information.
  * root - override default root

##### Returns
Returns a FileHound instance.

### `FileHound.any(FileHound...) -> Promise`

##### Parameters
* Accepts one or more instances of FileHound. Will unpack an array.

##### Returns
Returns a Promise of all matches. If the Promise fulfills, the fulfillment value is an array of all matching files.

### `FileHound.findFiles(path, globPattern) -> Promise`

##### Parameters
* `path` - Root path to search recursively
* `globPattern` - Optional file glob. By default, will match all files

##### Returns
* If the Promise fulfills, the fulfillment value is an array of matching files.

### `FileHound.not(FileHound...) -> Promise`

##### Parameters
* Accepts one or more instances of FileHound to negate. Will unpack an array.

##### Returns
* If the Promise fulfills, the fulfillment value is an array of negated matches

## Instance methods

### `.paths(paths...) -> FileHound`

Directories to search. Accepts one or more directories or a reference to an array of directories

##### Parameters
* path - array of directories

##### Returns
* Returns a FileHound instance

### `.ext(extension) -> FileHound`

##### Parameters
* extension - file extension to filter by

##### Returns
* Returns a FileHound instance

### `.match(glob) -> FileHound`

##### Parameters
* glob - file glob (as string) to filter by

##### Returns
* Returns a FileHound instance

### `.size(sizeExpression) -> FileHound`

##### Parameters
* sizeExpression - accepts a positive integer representing the file size in bytes. Optionally, can be prefixed with a comparison operator, including <, >, =, <=, >=  

##### Returns
* Returns a FileHound instance

### `.isEmpty() -> FileHound`

##### Parameters - None

##### Returns
* Returns a FileHound instance

### `.find() -> Promise`
##### Parameters - None
##### Returns
* Returns a Promise of all matches. If the Promise fulfills, the fulfillment value is an array of all matching files.

## Test

```
npm test
```

To generate a test coverage report:

```
npm run coverage
```
