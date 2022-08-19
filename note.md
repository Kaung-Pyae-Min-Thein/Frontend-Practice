/_npm install_/
first initialize npm with package.json
npm init
then install the node sass
npm install node-sass --save-dev

if already have the package.json initialized with node-sass then just run
npm install

for install live server globally
npm install live-server -g

for watching the css changes

"watch:sass": "node-sass sass/main.scss css/main.css -w"
for run live server
"devserver" : "live-server"

for run all npm scripts
first install
npm install npm-run-all --save-dev

then the scripts
"start" : "npm-run-all --parallel devserver watch:sass"

then build process

first compile the sass to compile css file
"compile:sass": "node-sass sass/main.scss css/main.comp.css"
then install the concat package
npm install concat --save-dev
then concat the css file
"concat:css": "concat -o css/main.concat.css css/main.css"
the add prefixer for all browser
first install prefix package
npm install autoprefixer --save-dev
then run to autoprefixer we need postcss
npm install postcss-cli --save-dev
then add prefix css
"prefix:css": "postcss --use autoprefixer -b 'last 10 versions' css/main.concat.css -o css/main.prefix.css",
then compress all the css file
"compress:css": "node-sass css/main.concat.css --output-style compressed"
the build script
"build:css" : "npm-run-all compile:sass concat:css prefix:css compress:css"

it will look like
"watch:sass": "node-sass sass/main.scss css/main.css -w",
"compile:sass": "node-sass sass/main.scss css/main.comp.css",
"concat:css": "concat -o css/main.concat.css css/main.css",
"prefix:css": "postcss --use autoprefixer -b 'last 10 versions' css/main.concat.css -o css/main.prefix.css",
"compress:css": "node-sass css/main.concat.css --output-style compressed",
"build:css" : "npm-run-all compile:sass concat:css prefix:css compress:css"
then run npm run build:css

//Script setup
https://thinkdobecreate.com/articles/minimum-static-site-sass-setup/
{
"name": "project",
"version": "0.1.0",
"description": "SASS compile|autoprefix|minimize and live-reload dev server using Browsersync for static HTML",
"main": "public/index.html",
"author": "5t3ph",
"scripts": {
"build:sass": "sass --no-source-map src/sass:public/css",
"copy:html": "copyfiles -u 1 ./src/_.html public",
"copy": "npm-run-all --parallel copy:_",
"watch:html": "onchange 'src/_.html' -- npm run copy:html",
"watch:sass": "sass --no-source-map --watch src/sass:public/css",
"watch": "npm-run-all --parallel watch:_",
"serve": "browser-sync start --server public --files public",
"start": "npm-run-all copy --parallel watch serve",
"build": "npm-run-all copy:html build:_",
"postbuild": "postcss public/css/_.css -u autoprefixer cssnano -r --no-map"
},
"dependencies": {
"autoprefixer": "^10.4.2",
"browser-sync": "^2.27.7",
"copyfiles": "^2.4.1",
"cssnano": "^5.0.17",
"npm-run-all": "^4.1.5",
"onchange": "^7.1.0",
"postcss-cli": "^9.1.0",
"sass": "^1.49.8"
}
}
