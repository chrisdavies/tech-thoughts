# Requiring an entire directory with browserify

When building single-page-apps, I like to have a controller per file, and I like to define my routes right alongside my controllers.

Rather than explicitly requiring each controller in my `init.js` file, I'd rather just require the entire `controllers` directory.

Here's how to do that.

## In your gulpfile:

```javascript
var gulp = require('gulp');
var browserify = require('gulp-browserify');
var rename = require('gulp-rename');
var uglify = require('gulp-uglify');
var bulkify = require('bulkify');

gulp.task('js', function () {
  return gulp.src('./src/js/init.js')
    .pipe(browserify({
      transform: ['bulkify']
    }))
    .pipe(rename('app.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./dest/js'));
});
```

## In your init.js (or whatever)

```javascript
var bulk = require('bulk-require');

// Require all of the scripts in the controllers directory
bulk(__dirname, ['controllers/**/*.js']);
```

## Tada

This information was pretty tough for me to trackdown, so here it is for all teh webz. You're welcome, internet.
