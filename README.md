# api documentation for  [html-snapshots (v0.14.0)](https://github.com/localnerve/html-snapshots)  [![npm package](https://img.shields.io/npm/v/npmdoc-html-snapshots.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-html-snapshots) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-html-snapshots.svg)](https://travis-ci.org/npmdoc/node-npmdoc-html-snapshots)
#### A selector-based html snapshot tool using PhantomJS that sources sitemap.xml, robots.txt, or arbitrary input

[![NPM](https://nodei.co/npm/html-snapshots.png?downloads=true)](https://www.npmjs.com/package/html-snapshots)

[![apidoc](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-html-snapshots_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-html-snapshots/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Alex Grant",
        "email": "alex@localnerve.com",
        "url": "http://localnerve.com"
    },
    "bugs": {
        "url": "https://github.com/localnerve/html-snapshots/issues"
    },
    "contributors": [
        {
            "name": "Alex Grant",
            "email": "alex@localnerve.com"
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
            "name": "localnerve",
            "email": "alex@localnerve.com"
        }
    ],
    "name": "html-snapshots",
    "optionalDependencies": {},
    "pre-commit": [
        "lint"
    ],
    "readme": "ERROR: No README data found!",
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



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module html-snapshots](#apidoc.module.html-snapshots)
1.  [function <span class="apidocSignatureSpan">html-snapshots.</span>run (options, listener)](#apidoc.element.html-snapshots.run)



# <a name="apidoc.module.html-snapshots"></a>[module html-snapshots](#apidoc.module.html-snapshots)

#### <a name="apidoc.element.html-snapshots.run"></a>[function <span class="apidocSignatureSpan">html-snapshots.</span>run (options, listener)](#apidoc.element.html-snapshots.run)
- description and source-code
```javascript
run = function (options, listener) {
  var inputGenerator, notifier, started, result, q, emitter, completion;

  options = options || {};
  prepOptions(options);

  // create the inputGenerator, default to robots
  inputGenerator = inputFactory.create(options.input);

  // clean the snapshot output directory
  if (options.outputDirClean) {
    rimraf(options.outputDir);
  }

  // start async completion notification.
  notifier = new Notifier();
  emitter = new EventEmitter();
  started = notifier.start(options.pollInterval, inputGenerator,
    function (err, completed) {
      emitter.emit("complete", err, completed);
    });

  if (started) {
    // create the completion Promise.
    completion = new Promise(function (resolve, reject) {
      function completionResolver (err, completed) {
        try {
          _.isFunction(listener) && listener(err, completed);
        } catch (e) {
          console.error("User supplied listener exception", e);
        }
        if (err) {
          err.notCompleted = notifier.filesNotDone;
          err.completed = completed;
          reject(err);
        } else {
          resolve(completed);
        }
      }
      emitter.addListener("complete", completionResolver);
    });

    // create a worker queue with a parallel process limit.
    q = asyncLib.queue(function (task, callback) {
      task(_.once(callback));
    }, options.processLimit);

    // have the queue call notifier.empty when last item
    //  from the queue is given to a worker.
    q.empty = notifier.qEmpty.bind(notifier);

    // expose abort callback to input generators via options.
    options._abort = function (err) {
      notifier.abort(q, err);
    };

    // generate input for the snapshots.
    result = inputGenerator.run(options, function (input) {
      // give the worker the input and place into the queue
      q.push(_.partial(worker, input, options, notifier));
    })
      // after input generation, resolve on browser completion.
      .then(function () {
        return completion;
      });
  } else {
    result = Promise.reject("failed to start async notifier");
  }

  return result;
}
```
- example usage
```shell
...
A growing showcase of runnable examples can be found [here](/examples).

An older (version 0.13.2), more in depth usage example is located in this [article](/docs/example-heroku-redis.md) that includes
 explanation and code of a real usage featuring dynamic app routes, ExpressJS, Heroku, and more.

### Simple example
'''javascript
var htmlSnapshots = require('html-snapshots');
htmlSnapshots.run({
  source: "/path/to/robots.txt",
  hostname: "exampledomain.com",
  outputDir: "./snapshots",
  outputDirClean: true,
  selector: "#dynamic-content"
})
.then(function (completed) {
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
