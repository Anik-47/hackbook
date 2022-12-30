---
description: Lets learn about some headers important for security
---

# Headers

{% hint style="info" %}
These are all response headers, these headers can be found in the HTTP response sent by the server.
{% endhint %}

## Strict-Transport-Security

```
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

This informs any visiting web browser that the site and all its subdomains use only SSL/TLS communication and that the browser should default to accessing it over HTTPS for the next two years (the `max-age` value in seconds).&#x20;

The _<mark style="color:blue;">`preload`</mark>_ directive indicates that the site is present on a global list of HTTPS-only sites. The purpose of preloading is to speed up page loads and eliminate the risk of man-in-the-middle (MITM) attacks when a site is visited for the first time.



## Content-Security-Policy

It lets you precisely control permitted content sources and many other content parameters and is recommended way to protect your websites and applications against XSS attacks. A basic _CSP_ header to allow only assets from the local origin is:&#x20;

```
Content-Security-Policy: default-src 'self'
```

Other directives include <mark style="color:blue;">`script-src`</mark>, <mark style="color:blue;">`style-src`</mark>, and `img-src` to specify permitted sources for scripts, CSS stylesheets, and images. For example, if you specify <mark style="color:blue;">`script-src 'self'`</mark>, you are restricting scripts, but not other content to the local origin.

## X-Frame-Options

This header was introduced way back in 2008 in Microsoft Internet Explorer to provide protection against cross-site scripting attacks involving HTML iframes. To completely prevent the current page from being loaded into iframes, you can specify:

```
X-Frame-Options: deny
```

Other supported values are <mark style="color:blue;">`sameorigin`</mark> to only allow loading into iframes with the same origin and <mark style="color:blue;">`allow-from`</mark> to indicate specific permitted URLs. Note that nowadays, this header can usually be replaced by suitable CSP directives.



## X-XSS-Protection

The <mark style="color:blue;">`X-XSS-Protection`</mark> header was introduced to protect against JavaScript injection attacks in the form of cross-site scripting. The usual syntax was:

```
X-XSS-Protection: 1; mode=block
```

In practice, it was relatively easy to bypass or abuse. Since modern browsers no longer use XSS filtering, this header is now deprecated.
