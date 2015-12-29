## gulp API docs

Jump to:
  [gulp.src](#gulpsrcglobs-options) |
  [gulp.dest](#gulpdestpath-options) |
  [gulp.task](#gulptaskname--deps-fn) |
  [gulp.watch](#gulpwatchglob--opts-tasks-or-gulpwatchglob--opts-cb)

### gulp.src(globs[, options])

Emits files matching provided glob or an array of globs.
Returns a [stream](http://nodejs.org/api/stream.html) of [Vinyl files](https://github.com/wearefractal/vinyl-fs)
that can be [piped](http://nodejs.org/api/stream.html#stream_readable_pipe_destination_options)
to plugins.

```javascript
gulp.src('client/templates/*.jade')
  .pipe(jade())
  .pipe(minify())
  .pipe(gulp.dest('build/minified_templates'));
```

#### globs
Type: `String` or `Array`

Glob or array of globs to read. Globs use [node-glob syntax] except that negation is fully supported.

Globbing is provided by [glob-stream](https://github.com/gulpjs/glob-stream) module. Please report any file watching problems directly to its [issue tracker](https://github.com/gulpjs/glob-stream/issues).

A glob that begins with `!` excludes matching files from the glob results up to that point. For example, consider this directory structure:

    client/
      a.js
      bob.js
      bad.js

The following expression matches `a.js` and `bad.js`:

    gulp.src(['client/*.js', '!client/b*.js', 'client/bad.js'])

You can pass any combination of globs. One caveat is that you can not only pass a glob negation, you must give it at least one positive glob so it knows where to start. All given must match for the file to be returned.

Globs are executed in order, so negations should follow positive globs. For example:

    gulp.src(['!b*.js', '*.js'])

would not exclude any files, but this would

    gulp.src(['*.js', '!b*.js'])


#### options
Type: `Object`


#### options.allowEmpty
Type:  `Boolean`
Default: false

If true, won't emit an error when a glob pointing at a single file fails to match


##### options.base
Type: `String`
Default: everything before a glob starts (see [glob2base])

E.g., consider `somefile.js` in `client/js/somedir`:

```js
gulp.src('client/js/**/*.js') // Matches 'client/js/somedir/somefile.js' and resolves `base` to `client/js/`
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/somedir/somefile.js'

gulp.src('client/js/**/*.js', { base: 'client' })
  .pipe(minify())
  .pipe(gulp.dest('build'));  // Writes 'build/js/somedir/somefile.js'
```


##### options.cwd
Type:  `String`
Default: process.cwd()

The current working directory in which to search.


##### options.cwdbase
Type: `Boolean`
Default: `false`

When true it is the same as saying opt.base = opt.cwd


##### options.debug
Type: `Boolean`
Default: `false`

Set to enable debug logging.


##### options.dot
Type: `Boolean`
Default: `false`

Include .dot files in normal matches and globstar matches. Note that an explicit dot in a portion of the pattern will always match dot files.


##### options.follow
Type: `Boolean`
Default: `false`

Follow symlinked directories when expanding ** patterns. Note that this can result in a lot of duplicate references in the presence of cyclic links.


##### options.mark
Type: `Boolean`
Default: `false`

Add a / character to directory matches. Note that this requires additional stat calls.


##### options.matchBase
Type: `Boolean`
Default: `false`

Perform a basename-only match if the pattern does not contain any slash characters. That is, *.js would be treated as equivalent to **/*.js, matching all js files in all directories.


##### options.nobrace
Type: `Boolean`
Default: `false`

Do not expand {a,b} and {1..3} brace sets.


##### options.nodir
Type: `Boolean`
Default: `false`

Do not match directories, only files. (Note: to match only directories, simply put a / at the end of the pattern.)
ignore Add a pattern or an array of glob patterns to exclude matches. Note: ignore patterns are always in dot:true mode, regardless of any other settings.


##### options.noglobstar
Type: `Boolean`
Default: `false`

Do not match ** against multiple filenames. (i.e., treat it as a normal * instead.)


##### options.noext
Type: `Boolean`
Default: `false`

Do not match +(a|b) "extglob" patterns.


##### options.nocase
Type: `Boolean`
Default: `false`

Perform a case-insensitive match. Note: on case-insensitive filesystems, non-magic patterns will match by default, since stat and readdir will not raise errors.


##### options.nomount
Type: `Boolean`
Default: `false`

By default, a pattern starting with a forward-slash will be "mounted" onto the root setting, so that a valid filesystem path is returned. Set this flag to disable that behavior.


##### options.nonull
Type: `Boolean`
Default: `true`

Set to never return an empty set, instead returning a set containing the pattern itself.


##### options.nosort
Type: `Boolean`
Default: `false`

Don't sort the results.


##### options.read
Type: `Boolean`
Default: `true`

Setting this to `false` will return `file.contents` as null and not read the file at all.


##### options.realpath
Type: `Boolean`
Default: `false`

Set to true to call fs.realpath on all of the results. In the case of a symlink that cannot be resolved, the full absolute path to the matched entry is returned (though it will usually be a broken symlink)


##### options.root
Type: `String`
Default: path.resolve(options.cwd, "/") (/ on Unix systems, and C:\ or some such on Windows.)

The place where patterns starting with / will be mounted onto.


##### options.silent
Type: `Boolean`
Default: `false`

When an unusual error is encountered when attempting to read a directory, a warning will be printed to stderr. Set the silent option to true to suppress these warnings.


##### options.stat
Type: `Boolean`
Default: `false`

Set to true to stat all results. This reduces performance somewhat, and is completely unnecessary, unless readdir is presumed to be an untrustworthy indicator of file existence.


##### options.strict
Type: `Boolean`
Default: `false`

When an unusual error is encountered when attempting to read a directory, the process will just continue on in search of other matches. Set the strict option to raise an error in these cases.


The full set of options is available on [glob-stream](https://github.com/gulpjs/glob-stream).

### gulp.dest(path[, options])

Can be piped to and it will write files. Re-emits all data passed to it so you can pipe to multiple folders.  Folders that don't exist will be created.

```javascript
gulp.src('./client/templates/*.jade')
  .pipe(jade())
  .pipe(gulp.dest('./build/templates'))
  .pipe(minify())
  .pipe(gulp.dest('./build/minified_templates'));
```

The write path is calculated by appending the file relative path to the given
destination directory. In turn, relative paths are calculated against the file base.
See `gulp.src` above for more info.

#### path
Type: `String` or `Function`

The path (output folder) to write files to. Or a function that returns it, the function will be provided a [vinyl File instance](https://github.com/wearefractal/vinyl).

#### options
Type: `Object`

##### options.cwd
Type: `String`
Default: `process.cwd()`

`cwd` for the output folder, only has an effect if provided output folder is relative.

##### options.mode
Type: `String`
Default: `0777`

Octal permission string specifying mode for any folders that need to be created for output folder.

### gulp.task(name [, deps, fn])

Define a task using [Orchestrator].

```js
gulp.task('somename', function() {
  // Do stuff
});
```

#### name
Type: `String`

The name of the task. Tasks that you want to run from the command line should not have spaces in them.

#### deps
Type: `Array`

An array of tasks to be executed and completed before your task will run.

```js
gulp.task('mytask', ['array', 'of', 'task', 'names'], function() {
  // Do stuff
});
```

**Note:** Are your tasks running before the dependencies are complete?  Make sure your dependency tasks are correctly using the async run hints: take in a callback or return a promise or event stream.

You can also omit the function if you only want to run a bundle of dependency tasks:

```js
gulp.task('build', ['array', 'of', 'task', 'names']);
```

**Note:** The tasks will run in parallel (all at once), so don't assume that the tasks will start/finish in order.

#### fn
Type: `Function`

The function performs the task's main operations. Generally this takes the form of:

```js
gulp.task('buildStuff', function() {
  // Do something that "builds stuff"
  var stream = gulp.src(/*some source path*/)
  .pipe(somePlugin())
  .pipe(someOtherPlugin())
  .pipe(gulp.dest(/*some destination*/));

  return stream;
  });
```

#### Async task support

Tasks can be made asynchronous if its `fn` does one of the following:

##### Accept a callback

```javascript
// run a command in a shell
var exec = require('child_process').exec;
gulp.task('jekyll', function(cb) {
  // build Jekyll
  exec('jekyll build', function(err) {
    if (err) return cb(err); // return error
    cb(); // finished task
  });
});
```

##### Return a stream

```js
gulp.task('somename', function() {
  var stream = gulp.src('client/**/*.js')
    .pipe(minify())
    .pipe(gulp.dest('build'));
  return stream;
});
```

##### Return a promise

```javascript
var Q = require('q');

gulp.task('somename', function() {
  var deferred = Q.defer();

  // do async stuff
  setTimeout(function() {
    deferred.resolve();
  }, 1);

  return deferred.promise;
});
```

**Note:** By default, tasks run with maximum concurrency -- e.g. it launches all the tasks at once and waits for nothing. If you want to create a series where tasks run in a particular order, you need to do two things:

- give it a hint to tell it when the task is done,
- and give it a hint that a task depends on completion of another.

For these examples, let's presume you have two tasks, "one" and "two" that you specifically want to run in this order:

1. In task "one" you add a hint to tell it when the task is done.  Either take in a callback and call it when you're
done or return a promise or stream that the engine should wait to resolve or end respectively.

2. In task "two" you add a hint telling the engine that it depends on completion of the first task.

So this example would look like this:

```js
var gulp = require('gulp');

// takes in a callback so the engine knows when it'll be done
gulp.task('one', function(cb) {
    // do stuff -- async or otherwise
    cb(err); // if err is not null and not undefined, the run will stop, and note that it failed
});

// identifies a dependent task must be complete before this one begins
gulp.task('two', ['one'], function() {
    // task 'one' is done now
});

gulp.task('default', ['one', 'two']);
```


### gulp.watch(glob [, opts], tasks) or gulp.watch(glob [, opts, cb])

Watch files and do something when a file changes. This always returns an EventEmitter that emits `change` events.

### gulp.watch(glob[, opts], tasks)

#### glob
Type: `String` or `Array`

A single glob or array of globs that indicate which files to watch for changes.

#### opts
Type: `Object`

Options, that are passed to [`gaze`](https://github.com/shama/gaze).

#### tasks
Type: `Array`

Names of task(s) to run when a file changes, added with `gulp.task()`

```js
var watcher = gulp.watch('js/**/*.js', ['uglify','reload']);
watcher.on('change', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

### gulp.watch(glob[, opts, cb])

#### glob
Type: `String` or `Array`

A single glob or array of globs that indicate which files to watch for changes.

#### opts
Type: `Object`

Options, that are passed to [`gaze`](https://github.com/shama/gaze).

#### cb(event)
Type: `Function`

Callback to be called on each change.

```js
gulp.watch('js/**/*.js', function(event) {
  console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
});
```

The callback will be passed an object, `event`, that describes the change:

##### event.type
Type: `String`

The type of change that occurred, either `added`, `changed` or `deleted`.

##### event.path
Type: `String`

The path to the file that triggered the event.


[node-glob]: https://github.com/isaacs/node-glob
[node-glob documentation]: https://github.com/isaacs/node-glob#options
[node-glob syntax]: https://github.com/isaacs/node-glob
[glob-stream]: https://github.com/wearefractal/glob-stream
[gulp-if]: https://github.com/robrich/gulp-if
[Orchestrator]: https://github.com/robrich/orchestrator
[glob2base]: https://github.com/wearefractal/glob2base
