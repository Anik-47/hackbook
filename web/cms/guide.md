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

Note that this endpoint only exposes users that have made a post. **Only information about the users that has this feature enable will be provided**.

Also note that **/wp-json/wp/v2/pages** could leak IP addresses.



**Login username enumeration**

When login in **`/wp-login.php`** the **message** is **different** is the indicated **username exists or not**.



