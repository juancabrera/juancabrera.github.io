---
layout:     post
title:      The spring effect in JavaScript
date:       2014-01-20
categories: javascript motion animation
---

In the last weeks I was trying to figure out a way to make things scroll nicer and that doesnt affect negatively the performance. When I start I didnt know exactly what was the result I wanted, so after a few frustrated attempts coding something by my self I start looking for some sort of custom easing functions, how they works, etc. This is how I got to this [blog about Physics and JavaScript](http://burakkanber.com/){:target="_blank"}, specifically [to this article about car suspension](http://burakkanber.com/blog/physics-in-javascript-car-suspension-part-1-spring-mass-damper/){:target="_blank"} (Hooke’s Law). And I thought it may works great on scrolling.

After trying several times to use this on scrolling I finally made my first successful attempt, and it was pretty much what I had in mind: [http://code.juan.me/demos/springy/basic/html/](http://code.juan.me/demos/springy/basic/html/){:target="_blank"}. In the demo you can see on the left, the green squares are being eased with the spring effect, the one on the right has no easing (so you can see the difference).

Looking at the first attempt (and being an Android user) I realized there was something familiar with it, it took me a while to realize that it was exactly what the Messages App of iOS 7 does, so of course I couldnt resist to do this: [http://code.juan.me/demos/springy/messages/html/](http://code.juan.me/demos/springy/messages/html/){:target="_blank"}. And now we can say that Apple is using car suspension physics in some of their Apps (?).

I also wanted to try this in a sort of more real use case, so I made two more demos using a grid prototype, this one with images: [http://code.juan.me/demos/springy/grid-prototype/html/](http://code.juan.me/demos/springy/grid-prototype/html/){:target="_blank"} and this one more clean in white: [http://code.juan.me/demos/springy/grid-prototype-2/html/](http://code.juan.me/demos/springy/grid-prototype-2/html/){:target="_blank"}

__The Code__

Note: Ill only go through the parts of code related to the spring effect of the first demo, if you want to see more how the demo was made feel free to take a look at the full code.

{% highlight javascript %}
var options  = {
    stiffness: 100,
    friction: 20,
    threshold: 0.03
}, 
inMotion = Array(false, false, false, false, false, false)
{% endhighlight %}

First at all we define the spring properties, the params stiffness  and friction talk by them self. The threshold option is basically how long you want to keep the spring working, so like if acceleration is equal or less than 0.03 it will stop. The inMotion array specifies which of the squares are currently in motion.

{% highlight javascript %}
core.onScrolling = function(e) {
    lastScrollY = scrollY;
    scrollY = window.scrollY;

    for (var i = 0; i &lt;= 5; i++) inMotion[i] = true;

    if (!animating) {
        animating = true;
        requestAnimFrame(core.stepScrolling);           
    }
}
{% endhighlight %}

When the scrolling starts we set the flag of motion for each square in true and we also call core.stepScrolling using requestAnimFrame.

{% highlight javascript %}
core.stepScrolling = function() {
    $.each($boxItems, function(i, v) {
        var $thisItem = $(v);

        if (inMotion[i]) {
            if (scrollY - lastScrollY &gt; 0) {
                acceleration[i] = (options.stiffness + (i * 5)) * ((scrollY + 50)  + (i * 40) - parseInt($thisItem.css("top"))) - options.friction * velocity[i];
            } else {
                acceleration[i] = (options.stiffness - (i * 5)) * ((scrollY + 50) + (i * 40) - parseInt($thisItem.css("top"))) - options.friction * velocity[i];
            }
            velocity[i] += acceleration[i] * frameRate;
            newTop[i] += velocity[i] * frameRate;
            inMotion[i] = Math.abs(acceleration[i]) &gt;= options.threshold || Math.abs(velocity[i]) &gt;= options.threshold;

            $thisItem.css({top: newTop[i]});
        } else {
            core.completeScrolling();
        }
    });
    if (animating) requestAnimFrame(core.stepScrolling);
}
{% endhighlight %}

Here we go through all the square elements, if it is still in motion we calculate and set the acceleration, velocity and new position for it. When the squares are not in motion we call `core.completeScrolling`.

{% highlight javascript %}
core.completeScrolling = function() {
    for (var i = 0; i &lt;= 5; i++) {
        acceleration[i] = 0;
        velocity[i] = 0;
        inMotion[i] = false;
    }
    animating = false;
    cancelAnimationFrame(core.stepScrolling);
}
{% endhighlight %}

And finally, on `completeScrolling` we reset all our variables and stop the Request Animation Frame that was calling core.stepScrolling.

__Update — Jan 27, 2014.__

I put these demos in a [GitHub repo](https://github.com/juancabrera/demos){:target="_blank"} because some people wanted to contribute with a few improvements in performance :)

[https://github.com/juancabrera/demos](https://github.com/juancabrera/demos){:target="_blank"}
