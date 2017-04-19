# npmdoc-html-snapshots

#### api documentation for  [html-snapshots (v0.14.0)](https://github.com/localnerve/html-snapshots)  [![npm package](https://img.shields.io/npm/v/npmdoc-html-snapshots.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-html-snapshots) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-html-snapshots.svg)](https://travis-ci.org/npmdoc/node-npmdoc-html-snapshots)

#### A selector-based html snapshot tool using PhantomJS that sources sitemap.xml, robots.txt, or arbitrary input

[![NPM](https://nodei.co/npm/html-snapshots.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/html-snapshots)

- [https://npmdoc.github.io/node-npmdoc-html-snapshots/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Alex Grant",
        "url": "http://localnerve.com"
    },
    "bugs": {
        "url": "https://github.com/localnerve/html-snapshots/issues"
    },
    "contributors": [
        {
            "name": "Alex Grant"
        }
    ],
    "dependencies": {
        "async": "2.x || 1.x",
        "async-lock": "~0.3.9",
        "combine-errors": "^3.0.3",
        "lodash": "^4.0.0",
        "mkdirp": "^0.5.1",
        "phantomjs-prebuilt": "2.1.13",
        "request": "^2.67.0",
        "rimraf": "^2.5.0",
        "xml2js": "~0.4.16"
    },
    "description": "A selector-based html snapshot tool using PhantomJS that sources sitemap.xml, robots.txt, or arbitrary input",
    "devDependencies": {
        "coveralls": "^2.11.12",
        "eslint": "^3.13.0",
        "express": "^4.13.3",
        "istanbul": "^0.4.2",
        "mocha": "^3.0.1",
        "precommit-hook": "^3.0.0",
        "server-destroy": "^1.0.1",
        "sitemap-xml": "~0.1.0"
    },
    "directories": {},
    "dist": {
        "shasum": "d1b802c4f5f0df89dc35a3bc7cffad41cff67ad4",
        "tarball": "https://registry.npmjs.org/html-snapshots/-/html-snapshots-0.14.0.tgz"
    },
    "engines": {
        "node": ">= 4.x"
    },
    "gitHead": "c48edf06e0366e7a05c16904ae0eb3089d47c0c3",
    "homepage": "https://github.com/localnerve/html-snapshots",
    "keywords": [
        "SEO",
        "html",
        "snapshots",
        "selector",
        "ajax",
        "SPA",
        "robots.txt",
        "sitemap.xml"
    ],
    "license": "MIT",
    "main": "./lib/html-snapshots",
    "maintainers": [
        {
            "name": "localnerve"
        }
    ],
    "name": "html-snapshots",
    "optionalDependencies": {},
    "pre-commit": [
        "lint"
    ],
    "repository": {
        "type": "git",
        "url": "git+https://github.com/localnerve/html-snapshots.git"
    },
    "scripts": {
        "coverage": "istanbul cover -- _mocha test/mocha/**/*.js --recursive --reporter spec",
        "lint": "eslint .",
        "test": "mocha test/mocha/**/*.js --recursive --reporter spec",
        "test:async": "mocha test/mocha/async/test.js --reporter spec",
        "test:common": "mocha test/mocha/common/*.js --reporter spec",
        "test:html-snapshots": "mocha test/mocha/html-snapshots/*.js --reporter spec",
        "test:input-generators": "mocha test/mocha/input-generators/*.js --reporter spec",
        "validate": "npm ls"
    },
    "version": "0.14.0"
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
