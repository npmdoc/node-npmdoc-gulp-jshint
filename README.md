# api documentation for  [gulp-jshint (v2.0.4)](http://github.com/spalger/gulp-jshint)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-jshint.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-jshint)
#### JSHint plugin for gulp

[![NPM](https://nodei.co/npm/gulp-jshint.png?downloads=true)](https://www.npmjs.com/package/gulp-jshint)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-jshint/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp_jshint_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-jshint/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-gulp-jshint/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "bugs": {
        "url": "https://github.com/spalger/gulp-jshint/issues"
    },
    "contributors": [
        {
            "name": "Spencer Alger",
            "email": "email@spalger.com"
        },
        {
            "name": "Fractal",
            "email": "contact@wearefractal.com",
            "url": "http://wearefractal.com/"
        }
    ],
    "dependencies": {
        "gulp-util": "^3.0.0",
        "lodash": "^4.12.0",
        "minimatch": "^3.0.3",
        "rcloader": "^0.2.2",
        "through2": "^2.0.0"
    },
    "description": "JSHint plugin for gulp",
    "devDependencies": {
        "gulp": "^3.8.10",
        "jshint": "^2.9.4",
        "mocha": "^3.0.0",
        "should": "^11.0.0"
    },
    "directories": {},
    "dist": {
        "shasum": "f382b18564b1072def0c9aaf753c146dadb4f0e8",
        "tarball": "https://registry.npmjs.org/gulp-jshint/-/gulp-jshint-2.0.4.tgz"
    },
    "engines": {
        "node": ">= 0.4.0"
    },
    "gitHead": "807bead250086847c7b7ca1f85e5fbad97bfcdcc",
    "homepage": "http://github.com/spalger/gulp-jshint",
    "keywords": [
        "gulpplugin"
    ],
    "license": "MIT",
    "main": "./src/index.js",
    "maintainers": [
        {
            "name": "contra",
            "email": "contra@wearefractal.com"
        },
        {
            "name": "fractal",
            "email": "contact@wearefractal.com"
        },
        {
            "name": "spalger",
            "email": "email@spalger.com"
        }
    ],
    "name": "gulp-jshint",
    "optionalDependencies": {},
    "peerDependencies": {
        "jshint": "2.x"
    },
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/spalger/gulp-jshint.git"
    },
    "scripts": {
        "test": "gulp test"
    },
    "version": "2.0.4"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-jshint](#apidoc.module.gulp-jshint)
1.  [function <span class="apidocSignatureSpan">gulp-jshint.</span>extract (when)](#apidoc.element.gulp-jshint.extract)
1.  [function <span class="apidocSignatureSpan">gulp-jshint.</span>loadReporter (reporter)](#apidoc.element.gulp-jshint.loadReporter)
1.  [function <span class="apidocSignatureSpan">gulp-jshint.</span>reporter (reporter, reporterCfg)](#apidoc.element.gulp-jshint.reporter)



# <a name="apidoc.module.gulp-jshint"></a>[module gulp-jshint](#apidoc.module.gulp-jshint)

#### <a name="apidoc.element.gulp-jshint.extract"></a>[function <span class="apidocSignatureSpan">gulp-jshint.</span>extract (when)](#apidoc.element.gulp-jshint.extract)
- description and source-code
```javascript
function extract(when) {
  when = when || 'auto';

  return stream(function (file, cb) {
    fileIgnored(file, function (err, ignored) {
      if (err) return cb(err);
      if (ignored) return cb(null, file);

      file.jshint = file.jshint || {};
      file.jshint.extracted = jshintcli.extract(file.contents.toString('utf8'), when);
      return cb(null, file);
    });
  });
}
```
- example usage
```shell
...

Tells JSHint to extract JavaScript from HTML files before linting (see [JSHint CLI flags](http://www.jshint.com/docs/cli/)). Keep
 in mind that it doesn't override the file's content after extraction. This is your tool of choice to lint web components!

'''js
gulp.task('lintHTML', function() {
  return gulp.src('./src/*.html')
    // if flag is not defined default value is 'auto'
    .pipe(jshint.extract('auto|always|never'))
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});
'''

## LICENSE
...
```

#### <a name="apidoc.element.gulp-jshint.loadReporter"></a>[function <span class="apidocSignatureSpan">gulp-jshint.</span>loadReporter (reporter)](#apidoc.element.gulp-jshint.loadReporter)
- description and source-code
```javascript
loadReporter = function (reporter) {
  // we want the function
  if (typeof reporter === 'function') return reporter;

  // object reporters
  if (typeof reporter === 'object' && typeof reporter.reporter === 'function') return reporter.reporter;

  // load jshint built-in reporters
  if (typeof reporter === 'string') {
    try {
      return exports.loadReporter(require('jshint/src/reporters/' + reporter));
    } catch (err) {}
  }

  // load full-path or module reporters
  if (typeof reporter === 'string') {
    try {
      return exports.loadReporter(require(reporter));
    } catch (err) {}
  }
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-jshint.reporter"></a>[function <span class="apidocSignatureSpan">gulp-jshint.</span>reporter (reporter, reporterCfg)](#apidoc.element.gulp-jshint.reporter)
- description and source-code
```javascript
reporter = function (reporter, reporterCfg) {
  reporterCfg = reporterCfg || {};

  if (reporter === 'fail') {
    return exports.failReporter(reporterCfg);
  }

  var rpt = exports.loadReporter(reporter || 'default');

  if (typeof rpt !== 'function') {
    throw new PluginError('gulp-jshint', 'Invalid reporter');
  }

  // return stream that reports stuff
  return stream(function (file, cb) {
    if (file.jshint && !file.jshint.success && !file.jshint.ignored) {
      // merge the reporter config into this files config
      var opt = defaults({}, reporterCfg, file.jshint.opt);

      rpt(file.jshint.results, file.jshint.data, opt);
    }

    cb(null, file);
  });
}
```
- example usage
```shell
...
'''js
const jshint = require('gulp-jshint');
const gulp   = require('gulp');

gulp.task('lint', function() {
  return gulp.src('./lib/*.js')
    .pipe(jshint())
    .pipe(jshint.reporter('YOUR_REPORTER_HERE'));
});
'''

## Options

Plugin options:
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
