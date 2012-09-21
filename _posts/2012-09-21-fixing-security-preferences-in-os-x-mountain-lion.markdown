---
layout: post
title: Fixing security preferences in OS X Mountain Lion
tags:
- development
---

When trying to run an application, OS X gave me the following warning:

[![OS X Mountain Lion Warning](/img/2012-09-21-osx-warning.png)](/img/2012-09-21-osx-warning.png)

> “Application name” can’t be opened because it is from an unidentified developer. Your security preferences allow installation of only apps from the Mac App Store and identified developers.

### 1) File fix

1. Right-click on the application you want to open
2. Select "open"

This will override the built-in default security settings for this specific file.

### 2) Global fix

1. Go to `System Preferences > Security & Privacy > General`
2. Change `Allow applications downloaded from` to `Anywhere`

You might need to press the lock icon first and confirm the change.
