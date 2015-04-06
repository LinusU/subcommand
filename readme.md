# subcommand

Create CLI tools with subcommands. A minimalist CLI router based on [minimist](https://www.npmjs.com/package/minimist) and [cliclopts](https://www.npmjs.com/package/cliclopts).

[![NPM](https://nodei.co/npm/subcommand.png)](https://nodei.co/npm/subcommand/)

[![js-standard-style](https://raw.githubusercontent.com/feross/standard/master/badge.png)](https://github.com/feross/standard)

[![Build Status](https://travis-ci.org/maxogden/subcommand.svg?branch=master)](https://travis-ci.org/maxogden/subcommand)

## usage

first, define your CLI API in JSON like this:

```js
var commands = [
  {
    name: 'foo',
    options: [ // cliclopts options
      {
        name: 'loud',
        boolean: true,
        default: false,
        abbr: 'v'
      }
    ],
    command: function foo (args) {
      // called when `foo` is matched
    }
  },
  {
    name: 'bar',
    command: function bar (args) {
      // called when `bar` is matched
    }
  }
]
```

then pass them into `subcommand`:

```js
var subcommand = require('subcommand')
var match = subcommand(commands, opts)
```

`subcommand` returns a function (called `match` above) that you can use to match/route arguments to their commands

```js
var matched = match(['foo'])
// matched will be true, and foo's `command` function will be called

var matched = match(['foo', 'baz', 'taco'])
// matched will be true, and foo's `command` function will be called with `['baz', 'taco']`

var matched = match(['bar'])
// matched will be true, and bar's `command` function will be called

var matched = match(['uhoh'])
// matched will be false
```

to pass options to the 'top level' command (e.g. when no subcommand is passed in), pass a command in your commands array that has `name: ''`

```js
var commands = [
  {
    name: '',
    options: [ // cliclopts options
      {
        name: 'loud',
        boolean: true,
        default: false,
        abbr: 'v'
      }
    ],
    command: function (args) {
      // called when no subcommand is specified
    }
  }
]
```
