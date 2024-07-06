---
description: >-
  WordPress is the most popular open source Content Management System, powering
  nearly one-third of all websites in the world. It can be used for many
  purposes, such as hosting blogs, forums, e-commerce
---

# WordPress

## WordPress Structure

<pre class="language-shell-session"><code class="lang-shell-session"><strong>/var/www/html
</strong>.
├── index.php
├── license.txt
├── readme.html
├── wp-activate.php
├── wp-admin
├── wp-blog-header.php
├── wp-comments-post.php
├── wp-config.php
├── wp-config-sample.php
├── wp-content
├── wp-cron.php
├── wp-includes
├── wp-links-opml.php
├── wp-load.php
├── wp-login.php
├── wp-mail.php
├── wp-settings.php
├── wp-signup.php
├── wp-trackback.php
└── xmlrpc.php
</code></pre>

### Key WordPress Directories adn Files

#### wp-admin

_`wp-admin`_ folder contains the login page for administrator access and the backend dashboard. Once a user has logged in, they can make changes to the site based on their assigned permissions. The login page can be located at one of the following paths:

* `/wp-admin/login.php`
* `/wp-admin/wp-login.php`
* `/login.php`
* `/wp-login.php`

{% hint style="info" %}
This file can also be renamed to make it more challenging to find the login page.
{% endhint %}



#### wp-content

The `wp-content` folder is the main directory where plugins and themes are stored. The subdirectory `uploads/` is usually where any files uploaded to the platform are stored. These directories and files should be carefully enumerated as they may lead to contain sensitive data that could lead to remote code execution or exploitation of other vulnerabilities or misconfigurations.

```shell-session
$ tree -L 1 /var/www/html/wp-content
.
├── index.php
├── plugins
└── themes
```

#### wp-includes

`wp-includes` contains everything except for the administrative components and the themes that belong to the website. This is the directory where core files are stored, such as certificates, fonts, JavaScript files, and widgets.

```shell-session
$ tree -L 1 /var/www/html/wp-includes
.
├── <SNIP>
├── theme.php
├── update.php
├── user.php
├── vars.php
├── version.php
├── widgets
├── widgets.php
├── wlwmanifest.xml
├── wp-db.php
└── wp-diff.php
```

## Wordpress version enumeration

**WP Version - Source Code**

{% code overflow="wrap" %}
```html
<link rel='https://api.w.org/' href='http://blog.inlanefreight.com/index.php/wp-json/' />
<link rel="EditURI" type="application/rsd+xml" title="RSD" href="http://blog.inlanefreight.com/xmlrpc.php?rsd" />
<link rel="wlwmanifest" type="application/wlwmanifest+xml" href="http://blog.inlanefreight.com/wp-includes/wlwmanifest.xml" /> 
<meta name="generator" content="WordPress 5.3.3" />
```
{% endcode %}

Aside from version information, the source code may also contain comments that may be useful. Links to CSS (style sheets) and JS (JavaScript) can also provide hints about the version number.

**WP Version - CSS**

{% code overflow="wrap" %}
```css
<link rel='stylesheet' id='bootstrap-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/css/bootstrap.css?ver=5.3.3' type='text/css' media='all' />
<link rel='stylesheet' id='transportex-style-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/style.css?ver=5.3.3' type='text/css' media='all' />
<link rel='stylesheet' id='transportex_color-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/css/colors/default.css?ver=5.3.3' type='text/css' media='all' />
<link rel='stylesheet' id='smartmenus-css'  href='http://blog.inlanefreight.com/wp-content/themes/ben_theme/css/jquery.smartmenus.bootstrap.css?ver=5.3.3' type='text/css' media='all' />
```
{% endcode %}

**WP Version - JS**

{% code overflow="wrap" %}
```html
...SNIP...
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-includes/js/jquery/jquery.js?ver=1.12.4-wp'></script>
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-includes/js/jquery/jquery-migrate.min.js?ver=1.4.1'></script>
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-content/plugins/mail-masta/lib/subscriber.js?ver=5.3.3'></script>
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-content/plugins/mail-masta/lib/jquery.validationEngine-en.js?ver=5.3.3'></script>
<script type='text/javascript' src='http://blog.inlanefreight.com/wp-content/plugins/mail-masta/lib/jquery.validationEngine.js?ver=5.3.3'></script>
...SNIP...
```
{% endcode %}



{% hint style="info" %}
In older WordPress versions, another source for uncovering version information is the _`readme.html`_ file in WordPress's root directory.
{% endhint %}



## Plugins and Themes Enumeration

**Plugins**

{% code overflow="wrap" %}
```bash
curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
```
{% endcode %}

**Themes**

{% code overflow="wrap" %}
```bash
anik2002@htb[/htb]$ curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```
{% endcode %}

## User enumeration

```bash
curl -s -I -X GET http://blog.inlanefreight.com/?author=1
curl http://blog.inlanefreight.com/wp-json/wp/v2/users | jq
```

## Remote Code Execution (RCE) via the Theme Editor



#### Step 1

Log in to WordPress with the administrator credentials, which should redirect us to the admin panel. Click on _`Appearance`_ on the side panel and select _`Theme Editor`_. This page will allow us to edit the PHP source code directly.&#x20;

{% hint style="info" %}
We should select an inactive theme in order to avoid corrupting the main theme.
{% endhint %}

#### Step 2

Choose an unused theme from drop down menu and click on _`Select`_. Choose a non-critical file such as `404.php` to modify and add a PHP web shell.

#### Step 3

We can validate that we have achieved RCE by entering the URL into the web browser or issuing the curl request

```bash
curl -X GET "http://<target>/wp-content/themes/twentyseventeen/404.php?cmd=id"
```
