---
layout:     post
title:      React Asset Loader
date:       2015-05-17
categories: javascript react
---

[This](https://github.com/juancabrera/react-asset-loader){:target="_blank"} is simple react component for loading assets. It allows you to load from a single asset to multiple assets without affecting the pageload speed. You can also add custom classes, attributes and callback functions.

## Concept
We all get scared of heavy websites, there are tons and tons of articles telling us why we shouldnt build websites using heavy assets like videos or gifs. But sometimes, you just need to, sometimes you just need to build a cool interactive experience with videos, or so something cool with gifs, or a long page with lots of high quality images, etc. This component is precisely for that.

## How it works
This component allows you to load all the assets you want without affecting the pageload speed, this is because it will load the page and then it will start loading all the assets, it adds a `<div className=loading></div>` wrapper while its loading the asset so you can have a nice custom loader, when the assets are loaded it will replace the loading wrapper with the actual assets.

## Install
{% highlight bash %}
npm install react-asset-loader --save-dev
{% endhighlight %}
Or just grab the component `LoadAsset.js` directly from the repo.

## Usage
__Load a single asset.__

{% highlight javascript %}
<LoadAssets assets={[{"uri":"/static/images/asset.jpg"}]} />
{% endhighlight %}

__Load multiple assets.__  
When you need to load more than one asset for one experience. It will be loadad after all the assets passed are loaded.


{% highlight javascript %}
<LoadAssets assets={[{"uri":"/static/images/video1.mp4"}, {"uri":"/static/images/video2.mp4"}]} />

// or also 

var videos = [ {"uri":"/static/images/video1.mp4"}, {"uri":"/static/images/video2.mp4"} ]
{% endhighlight %}

__Adding classes.__  
You can add classes to each asset you pass to the component.

{% highlight javascript %}
<LoadAssets assets={[{"uri":"/static/images/asset.jpg", "className":"class1 class2"}]} />
{% endhighlight %}

__Adding attributes.__  
You can also add attributes to each asset you pass to the component.

{% highlight javascript %}
<LoadAssets assets={[{"uri":"/static/images/video.mp4", "attributes":[{"autoPlay":"true"}, {"loop":"true"}]}]} />

// or also 

var video = [{ "uri":"/static/images/video.mp4", "attributes": [ {"autoPlay":"true"}, {"loop":"true"} ] }]
{% endhighlight %}

__Custom onLoad callback.__  
You can have a custom callback for when your assets are loaded.

{% highlight javascript %}
<LoadAssets onLoad={this.onCoolVideoLoaded} assets={[{"uri":"/static/images/video.mp4"}]} />
{% endhighlight %}

## Links
__Github__  
[https://github.com/juancabrera/react-asset-loader](https://github.com/juancabrera/react-asset-loader){:target="_blank"}

__NPM package__  
[https://www.npmjs.com/package/react-asset-loader](https://www.npmjs.com/package/react-asset-loader){:target="_blank"}
