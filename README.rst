Cross-Site Login Detection
=============================

Numerous major web sites have a security vulnerability that discloses the existence of an account. The vulnerability allows any random web site to establish that users have accounts at various other web sites. Because most people have accounts at multiple web sites and because all web sites have non-random userbase demographics, the vulnerability could be used to infer personal details about users. Moreover, it could be exploited en mass to build databases such as those being used by the many companies that track people online.

Demonstration
-------------
Either manually download the three index files, or clone this repo

::

    git clone https://github.com/abstract-theory/cross-site-login-detection.git

Open up ``index.html`` in your browser. This page will scan 34 web sites and list them as either logged in or not logged in.

Basic Principles
------------------

Web sites that maintain user accounts often have a login URL that allows redirection after login. The redirection, though, behaves differently depending on whether or not a user is already logged in.  For example, pressume ``example.com`` is a vulnerable website. If a user is not logged in and they attempt to visit a non-public-page, they will be redirected to the login URL shown below.

::

    https://www.example.com/login?redirect=non-public-page

At this URL, the user will be prompted for login credentials. However, if a logged in user visits this URL, they will be redirected to the non-public page specified in the URL argument.

This type of login URL allows redirected cross-site requests. When such a request succeeds, it confirms the existance of an account.


Additional Technical Details
----------------------------
Modern browsers block many types of cross-site requests. This is known as the Same Origin Policy (SOP). Browsers, however, do not apply SOP to all requests. For example, images, CSS, JavaScript, and fonts are not blocked. The same goes for media enclosed in ``<video>`` and ``<audio>`` tags. Server responses with certain CORS headers are also not subject to SOP. While many sites do validate redirect URLs, the validation is usually not sufficiently restrictive.

This attack can often be carried out by crafting a URL that redirects to the target web site's favicon image. For example:

::

    https://www.example.com/login?redirect=favicon.ico

This URL is then placed in an ``<img>`` tag on the attacker's web page. Attempting to load the image results in one of two JavaScript events: ``onload``, if the user is logged in, and ``onerror`` if they are not logged in.
