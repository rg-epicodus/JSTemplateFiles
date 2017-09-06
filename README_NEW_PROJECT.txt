Create Index.html
Create .gitignore
Create README.md
Create gulpfile.js
Create js/template.js (add in exports.xxx = ObjectName;) at bottom of .js
Create js/template-interface.js (add in var ObjectName = require('./../js/template.js').ObjectNameModule;

$ npm init
$ npm install gulp --save-dev
$ npm install browserify --save-dev
$ npm install gulp -g

Add to gulp file.js:
var gulp = require('gulp');

$ npm install vinyl-source-stream --save-dev

Add to gulp file.js:
var browserify = require('browserify');
var source = require('vinyl-source-stream');

gulp.task('jsBrowserify', function() {
  return browserify({ entries: ['./js/pingpong-interface.js'] })
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'));
});

$ gulp jsBrowserify (generates build folder)

change html script tags to:
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.0/jquery.min.js"></script>
    <script type="text/javascript" src="build/js/app.js"></script>

Add secondary interface.js file

$ npm install gulp-concat --save-dev

Add to gulp file.js:
var concat = require('gulp-concat');

...

gulp.task('concatInterface', function() {
  return gulp.src(['./js/pingpong-interface.js', './js/signup-interface.js'])
    .pipe(concat('allConcat.js'))
    .pipe(gulp.dest('./tmp'));
});

modify jsBrowserify task to:

gulp.task('jsBrowserify', ['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js'] })
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'));
});

$ gulp jsBrowserify

$ npm install gulp-uglify --save-dev

Add in gulp file:
var uglify = require('gulp-uglify');

gulp.task("minifyScripts", ["jsBrowserify"], function(){
  return gulp.src("./build/js/app.js")
    .pipe(uglify())
    .pipe(gulp.dest("./build/js"));
});

gulp.task("build", function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});

$ npm install gulp-util --save-dev
Add in gulp file:
var utilities = require('gulp-util');

var buildProduction = utilities.env.production;

$ gulp build
$ npm install del --save-dev

add to gulp file
var del = require('del');
gulp.task("clean", function(){
  return del(['build', 'tmp']);
});

gulp.task("build", ['clean'], function(){
  if (buildProduction) {
    gulp.start('minifyScripts');
  } else {
    gulp.start('jsBrowserify');
  }
});

$ npm install jshint --save-dev
$ npm install gulp-jshint --save-dev

add to gulp file
var jshint = require('gulp-jshint');

gulp.task('jshint', function(){
  return gulp.src(['js/*.js'])
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
});

$ gulp jshint
$ npm install bower -g
$ bower init
$ bower install jquery --save
$ bower install

change ajax.google script to:
<script src="bower_components/jquery/dist/jquery.min.js"></script>

$ bower install bootstrap --save

add before app.js:
 <link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">
    <script src="bower_components/bootstrap/dist/js/bootstrap.min.js"></script>

$ bower install moment --save

add above app.js
<script src="bower_components/moment/min/moment.min.js"></script>

$ npm install bower-files --save-dev

add to gulp file:
var lib = require('bower-files')();

gulp.task('bowerJS', function () {
  return gulp.src(lib.ext('js').files)
    .pipe(concat('vendor.min.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./build/js'));
});

change scripts to:
	<link rel="stylesheet" href="build/css/vendor.css">
    <script src="build/js/vendor.min.js"></script>
    <script type="text/javascript" src="build/js/app.js"></script>

add to gulp file:
gulp.task('bowerCSS', function () {
  return gulp.src(lib.ext('css').files)
    .pipe(concat('vendor.css'))
    .pipe(gulp.dest('./build/css'));
});

var lib = require('bower-files')({
  "overrides":{
    "bootstrap" : {
      "main": [
        "less/bootstrap.less",
        "dist/css/bootstrap.css",
        "dist/js/bootstrap.js"
      ]
    }
  }
});

p.task('bower', ['bowerJS', 'bowerCSS']);

$ npm install browser-sync --save-dev

add to gulp file:
var browserSync = require('browser-sync').create();
gulp.task('serve', function() {
  browserSync.init({
    server: {
      baseDir: "./",
      index: "index.html"
    }
  });
gulp.watch(['js/*.js'], ['jsBuild']);
gulp.watch(['bower.json'], ['bowerBuild']);
});

gulp.task('jsBuild', ['jsBrowserify', 'jshint'], function(){
  browserSync.reload();
});

gulp.task('bowerBuild', ['bower'], function(){
  browserSync.reload();
});

$ npm install jasmine --save-dev
$ ./node_modules/.bin/jasmine init

change bit in JSON file:
"scripts": {
  "test": "jasmine"
}

$ npm test
$ npm install watchify --save-dev
$ npm install karma --save-dev
$ npm install karma-jasmine jasmine-core --save-dev
$ npm install karma-chrome-launcher --save-dev
$ npm install karma-cli --save-dev
$ npm install karma-browserify --save-dev
$ npm install karma-jquery --save-dev
$ npm install karma-jasmine-html-reporter --save-dev
$ karma init

config karma.conf.js
module.exports = function(config) {
  config.set({
    basePath: '',
    frameworks: ['jquery-3.2.1', 'jasmine', 'browserify'],
    files: [
      'js/*.js',
      'spec/*-spec.js',
    ],
    exclude: [
    ],
    preprocessors: {
      'js/*.js': [ 'browserify'],
      'spec/*.js': ['browserify'],
    },
    plugins: [
      'karma-jquery',
      'karma-browserify',
      'karma-jasmine',
      'karma-chrome-launcher',
      'karma-jasmine-html-reporter'
    ],

    reporters: ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    concurrency: Infinity
  })
}

change package.json to
  "scripts": {
    "test": "karma start karma.conf.js"
  },

  $ npm install babelify babel-preset-es2015 --save-dev

  var babelify = require("babelify");
...

gulp.task('jsBrowserify', ['concatInterface'], function() {
  return browserify({ entries: ['./tmp/allConcat.js']})
    .transform(babelify.configure({
      presets: ["es2015"]
    }))
    .bundle()
    .pipe(source('app.js'))
    .pipe(gulp.dest('./build/js'))
});

update karma.conf
browserify: {
      debug: true,
      transform: [ [ 'babelify', {presets: ["es2015"]} ] ]
},

create .jshintrc in root of project
add into new file:
{ "esversion":6 }
