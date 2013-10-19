---
layout: post
title: Loading Everything At Once - HTML, CSS & JavaScript
tags:
- development
---

Some Sakai3 front-end developers had a dream:

> For our widget loading mechanism we want to load the HTML / CSS and JavaScript in one request (without asking the user to use inline CSS or JavaScript)

It felt like quite a big challenge and I wondered if it would be possible from a front-end point of view or not. So I [gave it a go](http://christianv.github.com/everythingatonce/index.html).

### What we need from the back-end

In Nakamura (the Sakai3 back-end) we are using [Apache Sling](http://sling.apache.org/site/index.html) in combination with [Apache Jackrabbit](http://jackrabbit.apache.org/). Which means that when we go to a URL like twitter.2.json we get information back about the structure of it's contents:


{% highlight javascript %}
{
    "jcr:createdBy": "admin",
    "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
    "jcr:primaryType": "sling:Folder",
    "css": {
        "jcr:createdBy": "admin",
        "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
        "jcr:primaryType": "sling:Folder",
        "twitter.css": {
            "jcr:createdBy": "admin",
            "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
            "jcr:primaryType": "nt:file"
        }
    },
    "javascript": {
        "jcr:createdBy": "admin",
        "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
        "jcr:primaryType": "sling:Folder",
        "twitter.js": {
            "jcr:createdBy": "admin",
            "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
            "jcr:primaryType": "nt:file"
        }
    },
    "twitter.html": {
        "jcr:createdBy": "admin",
        "jcr:created": "Mon Apr 19 2010 11:08:18 GMT+0100",
        "jcr:primaryType": "nt:file",
        "jcr:content": {
            "jcr:lastModifiedBy": "admin",
            ":jcr:data": 2465,
            "jcr:primaryType": "nt:resource",
            "jcr:uuid": "65305673-e757-4ff8-a62a-26f10b11a020",
            "jcr:mimeType": "text/html",
            "jcr:lastModified": "Mon Apr 19 2010 11:07:54 GMT+0100"
        }
    }
}
{% endhighlight %}

If we remove all the “jcr:” properties on those nodes, it makes it even more clear:

{% highlight javascript %}
{
    "css": {
        "twitter.css": {}
    },
    "javascript": {
        "twitter.js": {}
    },
    "twitter.html": {}
}
{% endhighlight %}

We basically get the hierarchical folder structure back for the twitter widget. If we change default GET servlet for sling or if we make a URL to something like twitter.content.json, it would be possible to get something like this back:

{% highlight javascript %}
{
    "css": {
        "twitter.css": {
            "content": "body {background:#000000}"
        }
    },
    "javascript": {
        "twitter.js": {
            "content": "alert(\"loaded\");"
        }
    },
    "twitter.html": {
        "content": "<!DOCTYPE html><html lang=\"en\"><head><meta http-equiv=\"Content-type\" content=\"text/html; charset=utf-8\"><title>Loaded page</title><link rel=\"stylesheet\" href=\"css/twitter.css\" type=\"text/css\" /><script type=\"text/javascript\" src=\"javascript/twitter.js\"></script></head><body><h1>Testing whether this could actually work</h1></body></html>"
    }
}
{% endhighlight %}

And that, would be genuinely awesome. This would mean that our widget could be loaded into one request! Except for the images – but those could get base64 encoded…

### About the example

Before I send a request to the back-end to have this feature enabled, I want to make sure that we could actually make it work in cross-browser, fast and easy to use way.

Basically a Sakai3 page process looks like this:

1. Load a Sakai3 page with all it's resources (HTML / CSS / JavaScript / Images)
2. When this page is loaded, load all the widgets you embedded on that page.

The Sakai3 widget process goes a bit deeper:

1. For each widget type, send one GET request to it's path. (e.g. devwidgets/twitter.3.json)
2. Run over all the properties in that file and divide it into 3 sections: HTML / CSS and JavaScript.
3. Save those 3 sections in separate objects / arrays in the JavaScript memory.
4. Remove the original &lt;link&gt; and &lt;script&gt; tags from the widget HTML.
5. Add the widget HTML to the container on the Sakai3 page.
6. Load the CSS and JavaScript dynamically without doing an extra request.

So instead of doing a request to an [HTML file](http://christianv.github.com/everythingatonce/loaded.html), a [CSS file](http://christianv.github.com/everythingatonce/css/loaded.css) and a [JavaScript file](http://christianv.github.com/everythingatonce/javascript/loaded.js), we would just make one request to a [JSON file](http://christianv.github.com/everythingatonce/json/widget.json).

[![Doing the requests separately](/img/2010-04-25-perf1.png)](/img/2010-04-25-perf1_b.png)

vs.

[![Doing everything in one request](/img/2010-04-25-perf2.png)](/img/2010-04-25-perf2_b.png)


Too be honest I got everything working quite fast (+/- 1,5 hour) but it took a bit longer to make everything cross-browser. Especially the last part, loading the JavaScript and CSS dynamically.

I knew jQuery just recently added the [$.getScript](http://api.jquery.com/jQuery.getScript/) but that added an extra Ajax call which I wanted to avoid in the first place. After lurking through the jQuery API and [source code](http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.js), I found out that the [$.globalEval](http://api.jquery.com/jQuery.globalEval/) function did exactly what I wanted for the JavaScript part.

Maybe I didn't look well enough, but I actually couldn't find any native jQuery method that would load the CSS dynamically. So I wrote something that is heavily based on [a post](http://www.phpied.com/dynamic-script-and-style-elements-in-ie/) by [Stoyan Stephanov](http://www.phpied.com/):

{% highlight javascript %}
var head = document.getElementsByTagName('head').item(0);
var tag = $("<link/>");
tag.attr(attributes);
if(tagname === "style" && tag[0].styleSheet){
    tag[0].styleSheet.cssText = content;
}else{
    tag.text(content);
}
head.appendChild(tag[0]);
{% endhighlight %}
All the code that I wrote so far is pretty much WIP and in non-development ready state. For instance at the moment I load all the CSS/JavaScript files I get back from the JSON where I should only use the ones defined in the HTML. That and some other little things still need to be fixed. But those are minor things though and I definitely think I have an awesome proof of concept.
