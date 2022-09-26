---
description: >-
  File upload vulnerabilities are when a web server allows users to upload files
  to its filesystem without sufficiently validating things like their name,
  type, contents, or size.
---

# File-Upload

From a security perspective, the worst possible scenario is when a website allows you to upload server-side scripts, such as PHP, Java, or Python files, and is also configured to execute them as code. This makes it trivial to create your own web shell on the server.

{% hint style="danger" %}
**Web shell**

A web shell is a malicious script that enables an attacker to execute arbitrary commands on a remote web server simply by sending HTTP requests to the right endpoint.

\
_`<?php echo system($_GET['command']); ?>`_
{% endhint %}

