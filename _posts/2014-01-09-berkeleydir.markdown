---
layout: post
title: Berkeleydir
tags:
- development
---

[Berkeleydir][berkeleydir] enables you to easily find people in the Berkeley directory. ([code][code])

### Why build something new?

Currently the way to find people at UC Berkeley is to use [directory.berkeley.edu][berkeleydirectory]. This service wasn't that intuitive for me to use.

* **Slow searching** - Sometimes I didn't know the spelling of a name of someone (e.g. Jon or John). This required multiple searches where you have to keep clicking the back button.
* **Dropdown** - It requires you to select what to search for in a certain format (e.g. `Vuerings, Christian`).
* **Non-mobile** - Since it's not responsive, using it on a mobile device was a pain.

### Features

Aiming for a clean and intuitive design, it adds the following features:

* **Autocomplete** - Searches as soon as you start typing.
* **Responsive** - Works well on any device and resolution.
* **Search always available** - Instead of having to click the back button for a new search, you can use the always available search input.
* **Search multiple properties** - Currently searches for *uid*, *name* and *email*.

### Technologies

* **[Firebase][firebase]** to save the data
* **[LDAP][ldap]** for querying users
* **[AngularJS][angularjs]** in the front-end
* **[Node.js][nodejs]** for crawling and exposing the API
  * **[Async](https://github.com/caolan/async)** which made it easier to handle asynchronous functions in Node.js.
  * **[Express](http://expressjs.com/)** a web application framework for node
  * **[Ldap.js][ldapjs]** a LDAP library for Node.js

### Challenges

**Firebase querying** - Setting up Firebase with AngularJS was pretty straightforward, but querying was somewhat of a pain. There's currently [no way to do database like queries with Firebase](http://stackoverflow.com/questions/11587775/database-style-queries-with-firebase).

**Speed** - Making it fast was of utter importance and the hardest part. In the beginning I loaded the more than 1,000,000 users in the browser. This made it extremely slow and not very scalable. Now it caches it in Node.js and provides 10 results at a time.

**Scraping** - In the first iteration I scraped the web version of the Berkeley directory at [directory.berkeley.edu][berkeleydirectory]. This was incredibly slow and took 40+ hours to complete. Now we use LDAP to get the data and takes approximately 1 hour to complete.

**LDAP with [Node.js][nodejs]** - Thanks to the [great documentation on the Berkeley LDAP service](https://wikihub.berkeley.edu/display/calnet/LDAP+-+Resources+for+Developers), it was very straightforward to connect to it with [ldapsearch][ldapsearch].

```
ldapsearch -H ldap://ldap.berkeley.edu -x -b 'ou=people,dc=berkeley,dc=edu' objectclass=*
```

However, connecting to LDAP with [ldapjs][ldapjs], a great LDAP library for Node.js, proved to be a bit harder. Thanks to [Mark Cavage](https://github.com/mcavage) for explaining that [ldapjs search scopes are backwards](http://stackoverflow.com/questions/20736327/ldapsearch-to-ldapjs-conversion/20748672#20748672). In the end, the only change was to set the `scope` to `sub` instead of using the default `base`.

### Future

As with any project, there are always improvements to be made:

* **University API** - Having a RESTful API for this on [https://developer.berkeley.edu/](https://developer.berkeley.edu/).
* **Images** - Support for images. Already tried using [gravatar](http://gravatar.com) for this but almost no one at Berkeley has their email linked with the that service.
* **Infinite scroll** - Currently it only shows 10 results per search.

If you would like to make any changes yourself, feel free to make a pull request on [https://github.com/christianv/berkeleydir][code].

[angularjs]: http://angularjs.org/
[berkeleydir]: http://berkeleydir.herokuapp.com/
[berkeleydirectory]: http://directory.berkeley.edu
[code]: https://github.com/christianv/berkeleydir
[firebase]: https://www.firebase.com/
[ldap]: http://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol
[ldapjs]: http://ldapjs.org/
[ldapsearch]: http://linux.die.net/man/1/ldapsearch
[nodejs]: http://nodejs.org/
