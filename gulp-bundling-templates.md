# Loading templates into a JavaScript object

I recently needed (or at least wanted) functionality like `folderify`, but with support for sub-directories. So,
I wrote this gulp task as a proof of concept. I'll probably tidy it up at some point to make it more generic, but
it does the trick.

Basically, it recursively crawls a directory, and puts the content of each `.html` file into a hash, and finally
generates a JavaScript file with the content of the hash as the export:


```javascript
// Turn all views into a JavaScript object
gulp.task('viewify', function () {
  var dir = './src/views';
  var rootDir = dir;
  var data = {};

  (function loadViews (dir) {
    fs.readdirSync(dir).forEach(function (file) {
      var fullPath = path.join(dir, file);

      if (fs.lstatSync(fullPath).isDirectory()) {
        loadViews(fullPath);
      } else if (file.slice(-5).toLowerCase() === '.html') {
        var pathWithoutExt = path.relative(rootDir, fullPath).slice(0, -5);
        data[pathWithoutExt] = fs.readFileSync(fullPath, 'utf8');
      }
    });
  })(dir);

  fs.writeFileSync('./src/js/bundled-views.js', 'module.exports = ' + JSON.stringify(data));
});
```
