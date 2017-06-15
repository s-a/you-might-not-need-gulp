# To Remove gulp.js from Repository

1.  Remove `gulp*` from devDependencies and dependencies sections in package.json.
2.  Remove `"prepublish": "gulp prepublish"` from script section in package.json.
3.  Delete `node_modules` folder to get rid of gulp.js.
4.  Type npm install to make something good again.
5.  Type something like `mocha` to run your unit tests.

## package.json scripts

-   `npm install --save-dev should eslint coveralls nyc mocha`

My current node module script stack. Pull requests with scripts replacing gulp.js jobs are welcome or feel free to create an issue when you figured out how to run a gulp.js job from the shell.

```javascript
  "scripts": {
    "lcov-file": "node node_modules/nyc/bin/nyc.js report --reporter=lcov",
    "coverage": "node node_modules/nyc/bin/nyc.js --reporter=html --reporter=text mocha && npm run lcov-file",
    "coveralls": "cat ./coverage/lcov.info | node node_modules/coveralls/bin/coveralls.js",
    "eslint": "node node_modules/eslint/bin/eslint.js ./lib",
    "webpack": "node_modules/.bin/webpack --config webpack.config.js",
    "debug": "iron-node node_modules/mocha/bin/_mocha",
    "prepublish": "npm test",
    "bump": "npm test && npm version patch && git push && git push --tags && npm publish",
    "mocha": "node node_modules/mocha/bin/_mocha",
    "test": "npm run eslint && npm run coverage"
  },
``` 

## .travis.yml

Send coverage Report to [Travis CI](https://travis-ci.org/).

    language: node_js
    node_js:
      - v8
      - v7
      - v6
      - v5
      - v4
    after_success:
      - npm run coveralls

## Some changes

### Removed dependencies

    "devDependencies": {
        "gulp": "^3.9.0",
        "gulp-coveralls": "^0.1.0",
        "gulp-eslint": "^3.0.1",
        "gulp-exclude-gitignore": "^1.0.0",
        "gulp-istanbul": "^1.0.0",
        "gulp-line-ending-corrector": "^1.0.1",
        "gulp-mocha": "^2.0.0",
        "gulp-nsp": "^2.1.0",
        "gulp-plumber": "^1.0.0",
    }

### Number of packages installed

**Before**
`added 732 packages in 38.838s`

**After**
`added 456 packages in 30.177s` deleted `14,1 MB (14.807.040 Bytes)`.

### Runtime for unit, code coverage and code quality tests

**Before**
Seconds           : 7
Milliseconds      : 91

**After**
Seconds           : 5
Milliseconds      : 350

## Conclusion

-   [x] Use the version of your favourite code coverage and unit test module you want where gulp.js plugins always depends on older versions.
-   [x] Get the taste of some performance improvements.
-   [x] Do not stop work where gulp.js plugins sometimes has a bugs which stops everything.
