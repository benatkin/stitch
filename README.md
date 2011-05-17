<img src="https://github.com/downloads/sstephenson/stitch/logo.jpg"
width=432 height=329>

Develop and test your JavaScript applications as CommonJS modules in
Node.js. Then __Stitch__ them together to run in the browser.

    npm install stitch

Bundle code in lib/ and vendor/ and serve it with [Express](http://expressjs.com/):

``` js
var stitch  = require('stitch');
var express = require('express');

var package = stitch.createPackage({
  paths: [__dirname + '/lib', __dirname + '/vendor']
});

var app = express.createServer();
app.get('/application.js', package.createServer());
app.listen(3000);
```

Or build it to a file:

``` js
var stitch  = require('stitch');
var fs      = require('fs');

var package = stitch.createPackage({
  paths: [__dirname + '/lib', __dirname + '/vendor']
});

package.compile(function (err, source){
  fs.writeFile('package.js', source, function (err) {
    if (err) throw err;
    console.log('Compiled package.js');
  })
})
```

# Compilers

## CoffeeScript

If [coffee-script](https://github.com/jashkenas/coffee-script) is
installed, it will be used to compile files ending in `.coffee` to 
JavaScript.

## eco

If [eco](https://github.com/sstephenson/eco.git) is available, it
will compile any files ending in `.eco` to JavaScript.

## Custom

It's also possible to add or override custom compilers, though the 
API is likely to change. Here is an example of adding a compiler to
include inlined json data:

``` js
var stitch = require('stitch');

stitch.compilers.json = function(module, filename) {
  var content = fs.readFileSync(filename, 'utf8');
  var data    = JSON.parse(content);
  var module  = ["module.exports = ", JSON.stringify(data), ";\n"].join("");
  return module._compile(module, filename);
};
```
