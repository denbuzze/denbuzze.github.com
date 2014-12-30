---
layout: post
title: 5 Lessons Learned Using gulp.js
tags:
- development
---

Using [gulp.js][gulpjs] isn't always straightforward. Here are a couple of tips to help you out.

### 1. Things don't run in sequence

With [gulp.js][gulpjs] tasks don't run in the order you define them. In fact they run in concurrently to make the total execution faster.

If you however want run tasks in order, you can define dependencies like this:

{% highlight javascript %}
gulp.task('one', function(cb) {
    cb(err);
});

gulp.task('two', ['one'], function() {

});

gulp.task('default', ['one', 'two']);
{% endhighlight %}

In previous versions of gulp, you should have used something like [gulp-sequence][gulp-sequence] or [run-sequence][run-sequence].

### 2. Watch carefully

When you watch for changes in files, make sure to only watch the files that change on a regular basis.

{% highlight javascript %}
// Yes
gulp.watch('src/js/**/*.js', ['js']);

// No
gulp.watch('**/*.*', ['js']);
{% endhighlight %}

Otherwise running the `gulp` task in the command line will result in a slower tasks and high CPU / memory usage.

### 3. Gulp-inject

After concatenating and minifying CSS and JavaScript files, I wanted to find a way to dynamically add those files to the HTML page. [gulp-inject][gulp-inject] was the plug-in that did the trick for me.

{% highlight javascript %}
// Make sure all the previous tasks (concatenation / minification) are run first
gulp.task('index', ['images', 'js', 'css', 'fonts'], function() {
    var inject = require('gulp-inject');
    var target = gulp.src('./src/index.html');

    // Read = false will not read the file contents and make the task faster
    var sources = gulp.src([paths.build.js, paths.build.css], {
        read: false
    });

    return target
    .pipe(inject(sources, {

        // Do not add a root slash to the beginning of the path
        addRootSlash: false,

        // Remove the `public` from the path when doing the injection
        ignorePath: 'public'
    }))
    .pipe(gulp.dest('public'));
});
{% endhighlight %}

And this is the `html` part:

{% highlight html %}
<body>
    <!-- inject:js -->
    <!-- endinject -->
</body>
{% endhighlight %}

Next to `html`, it also [supports](https://github.com/klei/gulp-inject/blob/b8618337e09ece47d8e89cf56e5b2c1c248208bd/README.md#injecttransform) `jade`, `jsx`, `slm` and `haml`.

### 4. Using bower?

When you're using [bower][bower] to manage your dependencies, [main-bower-files][main-bower-files] will come in handy. Otherwise you'll have to manually define which bower files you want to include.

{% highlight javascript %}
var mainBowerFiles = require('main-bower-files');

var paths = {
  src: {
    js: mainBowerFiles({

      // Set the base path for your bower components
      base: './bower_components',

      // Only return the JavaScript files
      filter: /.*\.js$/i

    // Concatenate the bower files with your own
    }).concat(['src/js/**/*.js']);
  }
}
{% endhighlight %}

### 5. Node + browsersync: possible but cumbersome

One of the most tedious parts was finding a way to make gulp work with [nodemon][nodemon] (automatically restart node) and [BrowserSync][browsersync]. The [gulpfile](https://github.com/sogko/gulp-recipes/blob/master/browser-sync-nodemon-expressjs/gulpfile.js) from [Hafiz Ismail](https://github.com/sogko) was a great start and I made some small changes.

{% highlight javascript %}
gulp.task('nodemon', function(cb) {
  var nodemon = require('gulp-nodemon');

  // We use this `called` variable to make sure the callback is only executed once
  var called = false;
  return nodemon({
    script: 'server.js',
    watch: ['server.js', 'server/**/*.*']
  })
  .on('start', function onStart() {
    if (!called) {
      cb();
    }
    called = true;
  })
  .on('restart', function onRestart() {

    // Also reload the browsers after a slight delay
    setTimeout(function reload() {
      browserSync.reload({
        stream: false
      });
    }, 500);
  });
});

// Make sure `nodemon` is started before running `browser-sync`.
gulp.task('browser-sync', ['index', 'nodemon'], function() {
  var port = process.env.PORT || 5000;
  browserSync.init({

    // All of the following files will be watched
    files: ['public/**/*.*'],

    // Tells BrowserSync on where the express app is running
    proxy: 'http://localhost:' + port,

    // This port should be different from the express app port
    port: 4000,

    // Which browser should we launch?
    browser: ['google chrome']
  });
});
{% endhighlight %}

### Conclusion

Hope you all taken something away from this post, if you have any remarks / comments, feel free to [contact me on twitter](https://twitter.com/denbuzze).

[bower]: http://bower.io/
[browsersync]: http://www.browsersync.io/
[gulpjs]: http://gulpjs.com/
[gulp-inject]: https://github.com/klei/gulp-inject
[gulp-sequence]: https://github.com/teambition/gulp-sequence
[main-bower-files]: https://github.com/ck86/main-bower-files
[nodemon]: https://github.com/remy/nodemon
[run-sequence]: https://github.com/OverZealous/run-sequence







