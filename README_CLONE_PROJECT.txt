start new project
create files:
index.html (make sure <script> has right stuff from repo)
.gitignore (copy from repo for stuff)
README.md (copy from repo)
.jshintrc (add { "esversion":6 } to contents of this file)

run $ npm init
run $ bower init
paste dependencies from host package.json over to new target package.json
run $ npm install
paste dependencies from host bower.json over to new target bower.json
run $ bower install
run $ gulp build
run $ karma init
paste everything from host karma.conf.js over to new target karma.conf.js
run $ npm install babelify babel-preset-es2015 --save-dev
