---
layout:     post
title:      My front-end codebase
date:       2014-11-16
categories: javascript node motion
---

After trying different front-end configurations in the past few months (Grunt or Gulp, HAML or Slim, ES6 modules or AMD modules, etc) I have finally found one that Im pretty comfortable with (at least for a while). This codebase is using:

- [Gulp](http://gulpjs.com/){:target="_blank"}  
- [Slim](http://slim-lang.com/){:target="_blank"}  
- [SASS](http://sass-lang.com/){:target="_blank"}  
- [Autoprefixer](https://github.com/postcss/autoprefixer){:target="_blank"}  
- [Traceur](https://github.com/google/traceur-compiler){:target="_blank"}   for ES6 transpile. (wrapper for command line traceur)

__Next steps__

Try and incorporate performance tools (like [uncss](https://github.com/ben-eb/gulp-uncss){:target="_blank"}, [critical-path](https://github.com/addyosmani/critical-path-css-demo){:target="_blank"}) and testing tools.

You can find the codebase on my Github (feel free to contribute if you like it): [https://github.com/juancabrera/gulp-frontend-codebase](https://github.com/juancabrera/gulp-frontend-codebase.).
