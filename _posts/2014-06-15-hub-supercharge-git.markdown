---
layout: post
title: Hub - Supercharge Git
tags:
- development
- workflow
---

If you're using [GitHub](https://github.com/) and the command line, then [hub][hub] is something for you.

![Supercharge Git](/img/2014-06-15-supercharge-git.svg)

### Why hub?

Have you ever wondered why you always have to add the complete URL when you're adding a git remote?

{% highlight bash %}
git remote add christianv https://github.com/christianv/dotfiles.git
{% endhighlight %}

With [hub][hub] this changes to

{% highlight bash %}
git remote add christianv
{% endhighlight %}

when you are in a git repository.

### Installation

The recommended way to install [hub][hub] is through homebrew:

{% highlight bash %}
brew install hub
{% endhighlight %}

Then you should alias `git` to `hub`

{% highlight bash %}
eval "$(hub alias -s)"
{% endhighlight %}

### Hightlighted Features

* **clone** - Clone a remote repository

{% highlight bash %}
git clone christianv
{% endhighlight %}

* **browse** - Open the current branch in the browser

{% highlight bash %}
git browse
{% endhighlight %}

* **pull-request**  - Create a pull request
{% highlight bash %}
git pull-request -b christianv:master -o
# The `-o` option will open the pull request in the browser.
{% endhighlight %}

### I want more

In this post we highlighted some of the awesome features of [hub][hub]. If you want more, definitely check out [the hub readme][hubreadme].

[hub]: http://github.com/github/hub
[hubreadme]: https://github.com/github/hub#readme
