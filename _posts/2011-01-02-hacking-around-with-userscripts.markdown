---
layout: post
title: Hacking Around With Userscripts
tags:
- development
---

### Lately

Lately I’ve been hacking around with userscripts. Those scripts are JavaScript files that can enhance the experience of a user on certain websites. Here’s a list to get you to start thinking:

Make a site more readable by changing the color of the text.
Add support for keyboard shortcuts.
Remove really annoying ads.
Of course you could think of many more things, the list is literately endless.


### My scripts

#### [The Piratebay Ad Remover](http://userscripts.org/scripts/show/89761)

My most popular script so far is an ad remover for [thepiratebay.org](http://thepiratebay.org/). The first version of the script was launched on the 5th of November 2010 and has had 320 downloads so far.

#### [Ted.com Alternative Video Player](http://userscripts.org/scripts/show/93984)

When looking at talks on [ted.com](http://www.ted.com/), the default flash player always annoyed me. The full screen button didn’t work and seeking through the video was buggy. Apart from that, when you go full screen, you still had the flash player controls on the top and bottom of the screen. As an alternative, I wanted to use the open-source [JW Player](http://www.longtailvideo.com/).

#### [mflow Keyboard Shortcuts](http://userscripts.org/scripts/show/93041)

[mflow.com](http://mflow.com/) is a great online music player for discovering new songs. The script ads some basic keyboard shortcuts to pause/resume a song or go to the next one. It was pretty straightforward to do, since I was able to access their JavaScript library.

#### Other scripts

You can find some of the other scripts I wrote and a more up to date list on my [userscripts.org profile page](http://userscripts.org/scripts/show/93984).

### Things to remember

* Don’t use jQuery when you can do it natively.
Both firefox and google chrome support JavaScript quite extensively. As you’ll read further on Chrome also doesn’t support the @require attribute.
* Write plenty of documentation in your code as well as on [userscripts.org](http://userscripts.org/).

### Google Chrome compatibility issues

If you want to make sure that your script also runs in Google Chrome, do the following:

#### @match attribute

Next to the @include attribute, also use the [@match attribute](http://www.chromium.org/developers/design-documents/user-scripts#TOC-Match-Patterns). This will make sure that the user who install the userscript will get an appropriate message: 
instead of 


#### Don’t use the @require attribute.

Most of the time you don’t need this, but if you do, create a script element and append it to the head or body.

#### Don’t use the unsafeWindow variable.

Greasemonkey allows you to access global JavaScript variables through this variable, but google chrome doesn’t. You can find a workaround in my [mflow userscript](http://userscripts.org/scripts/review/93041).

#### More information

You can find more information about userscripts in google chrome on the [chromium userscripts page](http://www.chromium.org/developers/design-documents/user-scripts).