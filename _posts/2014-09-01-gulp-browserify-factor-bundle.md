---
title: Gulp, browserify and factor-bundle
layout: post
---

This is a quick post about using the [factor-bundle](https://npmjs.org/package/factor-bundle) API with the [gulp](http://gulpjs.com) build system.

Thanks to the excellent work by @substack and @terinjokes on browserify@5.x and factor-bundle@2.x, it is much easier to use these tools in a streaming workflow.

Below is a gulp task[^1] that takes an array of entry points, passes them to browserify, and factors common dependencies into an output bundle called `common.js`, generating inline sourcemaps for all bundles.

```js
var browserify = require('browserify');
var factor = require('factor-bundle');
var gulp = require('gulp');
var merge = require('multistream-merge');
var source = require('vinyl-source-stream');

gulp.task('bundle', function () {
  var files = ['app/entry1.js', 'app/entry2.js'];
  var outputs = ['build/entry1.js', 'build/entry2.js'];

  var b = browserify({ entries: files, debug: true });
  var outputStreams = outputs.map(function (o) { return source(o); });
  b.plugin(factor, { entries: inputs, o: outputStreams });

  var common = b.bundle().pipe(source("common.js"));
  return merge.obj([].concat(outputStreams, common))
      .pipe(gulp.dest('./build'));
});
```

This uses [vinyl-source-stream](https://npmjs.org/package/vinyl-source-stream) for wrapping each output stream in a gulp-compatible Vinyl object and [multistream-merge](https://npmjs.org/package/multistream-merge) for merging the resulting output streams with the common bundle into a single stream of Vinyl objects.

From here, you could do further processing, like extracting sourcemaps into separate files, minifying the bundles in production, etc.

[^1]: *Note:* this does not account for error handling, which you *absolutely should do* in your gulp tasks.
