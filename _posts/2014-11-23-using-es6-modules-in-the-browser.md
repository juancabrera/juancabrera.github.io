---
layout:     post
title:      Using ES6 modules in the browser
date:       2014-11-16
categories: javascript ES6
---

There are a lot of information about Gulp, not so much for ES6 and just a very few articles about how to implement ES6 modules (for the browser) properly.

On my last project I was using Gulp + ES6 and I had to spent some time figuring out how to get ES6 modules working properly. This is how I finally did it without using Browserify or any AMD loader.

My first attempt was to use [gulp-es6-transpiler](https://www.npmjs.com/package/gulp-es6-transpiler){:target="_blank"} (that basically is a wrapper for [es6-transpiler](https://www.npmjs.com/package/es6-transpiler){:target="_blank"}) but it doesn’t support modules, then I took a look to [gulp-es6-module-jstransform](https://www.npmjs.org/package/gulp-es6-module-jstransform) but it only transpile to CommonJS, meaning that we’ll need to use Browserify, then I tried [Traceur](https://github.com/google/traceur-compiler){:target="_blank"} (from Google) and it has two options for browser modules, one is ‘AMD (meaning that we’ll need to use RequireJS or similar) and ‘inline’ that basically what it does is generate one file with all the transpiled modules on it (which is the closet option to “native” ES6 modules).
The thing is that the ‘inline’ option works perfect if you are using the command line, but if you use the Traceur’s node API (like [gulp-traceur](https://www.npmjs.org/package/gulp-traceur){:target="_blank"}) it’ll throw an error. 

__[UPDATE]__ [I've put this issue on their Github](https://github.com/google/traceur-compiler/issues/1282){:target="_blank"} and finally got fixed, but then we realized that the transpile from the node API wasn’t generating the same output as the command line, so we finally decided to build a small Gulp plugin wrapper for the command line Traceur (Thanks Edward!)

[Here is the plugin](https://www.npmjs.com/package/gulp-traceur-cmdline){:target="_blank"} and this is how you can use it:

__Install__  
Make sure you have installed Traceur globally:

{% highlight bash %}
npm install traceur --global
{% endhighlight %}

Install `gulp-traceur-cmdline` to your project:

{% highlight bash %}
npm install gulp-traceur-cmdline --save-dev
{% endhighlight %}

__Usage__  
{% highlight javascript %}
var gulpTraceurCmdline = require('gulp-traceur-cmdline');

gulp.task('gulpTraceurCmdline',function() {
  gulp.src("./source/styleguide/js/main.js")
    .pipe(gulpTraceurCmdline('/usr/local/bin/traceur', {
      modules : 'inline',
      out     : './dist/styleguide/js/main.js',
      debug   : false
    }))
});
{% endhighlight %}

Github: [https://github.com/juancabrera/gulp-traceur-cmdline](https://github.com/juancabrera/gulp-traceur-cmdline){:target="_blank"}

NPM: [https://www.npmjs.org/package/gulp-traceur-cmdline](https://github.com/juancabrera/gulp-traceur-cmdline){:target="_blank"}
