<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [BRS: Off-Roku BrightScript](#brs-off-roku-brightscript)
  - [Installation](#installation)
  - [Usage](#usage)
    - [REPL](#repl)
    - [Executing a file](#executing-a-file)
  - [Sure, but why?](#sure-but-why)
  - [So can I use this to watch TV without a Roku?](#so-can-i-use-this-to-watch-tv-without-a-roku)
  - [Building from source](#building-from-source)
    - [Prerequisites](#prerequisites)
    - [Setup](#setup)
    - [The build-test-clean dance](#the-build-test-clean-dance)
      - [Build](#build)
      - [Testing](#testing)
      - [Cleaning](#cleaning)
      - [All Together](#all-together)
  - [Documentation](#documentation)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# BRS: Off-Roku BrightScript
An interpreter for the BrightScript language that runs on non-Roku platforms.

[![CircleCI](https://circleci.com/gh/sjbarag/brs.svg?style=svg)](https://circleci.com/gh/sjbarag/brs)
[![NPM Version](https://badge.fury.io/js/brs.svg?style=flat)](https://npmjs.org/package/brs)

## Installation
The BRS project is published as a `node` package, so use `npm`:

```shell
$ npm install -g brs
```

or `yarn` if that's your preference:

```shell
$ yarn global add brs
```

## Usage
This repo provides the `brs` executable, which operates in two ways.

### REPL
An interactive BrightScript REPL (Read-Execute-Print Loop) is available by running `brs` with no arguments, e.g.:

```
$ brs
brs> ?"Dennis Ritchie said ""Hello, World!"""
Dennis Ritchie said "Hello, World!"
```

Quit by entering `^D` (Control-D).

### Executing a file
BRS can execute an arbitrary BrightScript file as well!  Simply pass the file to the `brs` executable, e.g.:

```
$ cat hello-world.brs
?"Dennis Ritchie said ""Hello, World!"""

$ brs hello-world.brs
Dennis Ritchie said "Hello, World!"
```

## Sure, but why?
The [Roku](https://roku.com) series of media streaming devices are wildly popular amongst consumers, and several [very](https://netflix.com) [popular](https://hulu.com) [streaming](https://amazon.com/primevideo) [services](https://crackle.com) offer Channels for the Roku platform.  Unfortunately, Roku chanels *must* be written in a language called BrightScript, which is only executable directly on a Roku device.  BRS hopes to change that by allowing Roku developers to test their code on their own machines, thus improving the quality of their channels and the end-user's experience as a whole.

## So can I use this to watch TV without a Roku?
Nope!  The BRS project currently has no intention of emulating the Roku user interface, integrating with the Roku store, or emulating content playback.  In addition to likely getting this project in legal trouble, that sort of emulation is a ton of work.

## Building from source
The BRS project follows pretty standard `node` development patterns, with the caveat that it uses `yarn` for dependency management.

### Prerequisites
BRS builds (and runs) in `node`, so you'll need to [install that first](https://nodejs.org).

Once that's ready, install [yarn](https://yarnpkg.com).  Installing it with `npm` is probably the simplest:

```shell
$ npm install -g yarn
```
### Setup
1. Clone this repo:
   ```
   $ git clone https://github.com/sjbarag/brs.git
   ```

2. Install dependencies:
    ```shell
    $ yarn install     # or just `yarn`
    ```

3. Get `brs` onto your `PATH`:
    ``` shell
    $ yarn link
    ```
### The build-test-clean dance
#### Build
This project is written in TypeScript, so it needs to be compiled before it can be executed.  `yarn build` compiles files in `src/` into JavaScript and TypeScript declarations, and puts them in `lib/` and `types/` respectively.

```shell
$ yarn build

$ ls lib/
index.js (and friends)

$ ls types/
index.d.ts (and friends)
```

Alternatively, you can run the build step in "watch" mode. This will run `yarn build` for you automatically, every time it detects source file changes:
```shell
$ yarn watch
```
This is often useful for testing that local changes work in your BrightScript project, without having to run `yarn build` over and over.

#### Testing
Tests are written in plain-old JavaScript with [Facebook's Jest](http://facebook.github.io/jest/), and can be run with the `test` target:

```shell
$ yarn test

# tests start running
```

Note that only test files ending in `.test.js` will be executed by `yarn test`.

#### Cleaning
Compiled output in `lib/` and `types/` can be removed with the `clean` target:

```shell
$ yarn clean

$ ls lib/
ls: cannot access 'lib': No such file or directory

$ ls types/
ls: cannot access 'types': No such file or directory
```

#### All Together
Thanks to the [npm-run-all](https://www.npmjs.com/package/npm-run-all) package, it's trivially easy to combine these into a sequence of tasks without relying on shell semantics:

```shell
$ yarn run-s clean build test
```

## Documentation

For the most part, `brs` attempts to emulate BrightScript as closely as possible. However, there are certain implementation gaps. Also, in the spirit of unit testing, there are a few extensions that will help with testing. All of our documentation for APIs, extensions, gaps, and more is hosted on our docs site, [https://hulu.github.io/roca](hulu.github.io/roca).
