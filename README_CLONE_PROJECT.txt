start new project
create files:
placeholder.js  / placeholder-interface.js
index.html (make sure <script> has right stuff from repo)
.gitignore (copy from repo for stuff)
README.md (copy from repo)
.jshintrc (add { "esversion":6 } to contents of this file)
gulpfile.js (copy from repo)

run $ npm init
paste dependencies from host package.json over to new target package.json
run $ npm install
run $ npm install bower -g
run $ bower init
paste dependencies from host bower.json over to new target bower.json
run $ bower install
run $ gulp build
run $ karma init
make sure package.json test has "karma start karma.conf.js"
paste everything from host karma.conf.js over to new target karma.conf.js
run $ npm install babelify babel-preset-es2015 --save-dev
