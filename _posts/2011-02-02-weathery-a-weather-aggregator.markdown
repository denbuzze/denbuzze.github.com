---
layout: post
title: Weathery – A Weather Aggregator
tags:
- development
---

As a sailing instructor it's important to know as much information about the weather as possible. You need to be able to see how much wind there's going to be, the height of the waves, the tides, if any storm is approaching, …

If you do this yourself, you usually have to check several websites. Each website has its advantages and disadvantages. Some are good in predicting the wind, others contain information about tides.

### Weathery

[Weathery](http://christianv.github.com/weathery/) is a web scraper that retrieves useful weather information from several sites. At this very moment it only gets the information for one specific place in Belgium, Koksijde.

[![Screenshot of weathery](/img/2011-02-02-weathery_small.png)](//img/2011-02-02-weathery_big.png)

Currently you can see the following:

* Basic weather information from both [Google](http://www.google.com/) and [Wunderground](http://www.wunderground.com/).
* Rainfall radar which is made by [buienradar.nl](http://buienradar.nl/). It allows you to see if any rain is coming.
* A live-stream webcam from [‘t Zoetgenot](http://zoetgenot.be/).
* Very good [wind prediction](http://www.windfinder.com/).
* UV emission prediction, sailing rating and wind information from [meteovista](http://www.meteovista.be/).

The background image you're seeing is randomly generated. It looks at the current weather condition (e.g. cloudy) adds a sea-like term (e.g. beach) and performs a search on [flickr](http://www.flickr.com/). If doesn't find anything, a grey background is shown.

Hopefully some of you like it and maybe you'll be able to use it the next time you jump on boat.
