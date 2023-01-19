---
description: >-
  JSON WEB TOKEN attacks involve a user sending modified JWTs to the server in
  order to bypass authentication and access controls by impersonating other
  users and taking control of their accounts
---

# JWT

JSON web tokens (JWTs) are used for sending data between systems. They can theoretically contain any kind of data but are most commonly used to send information ("claims") about users as part of authentication, session handling, and access control mechanisms.

### JWT format <a href="#jwt-format" id="jwt-format"></a>

A JWT consists of 3 parts:

* &#x20;<mark style="color:red;">header</mark>&#x20;
* <mark style="color:blue;">payload</mark>
* <mark style="color:green;">signature</mark>

These are each separated by a dot. For example:

<mark style="color:red;">eyJraWQiOiI5MTM2ZGRiMy1jYjBhLTRhMTktYTA3ZS1lYWRmNWE0NGM4YjUiLCJhbGciOiJSUzI1NiJ9</mark>.<mark style="color:blue;">eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTY0ODAzNzE2NCwibmFtZSI6IkNhcmxvcyBNb250b3lhIiwic3ViIjoiY2FybG9zIiwicm9sZSI6ImJsb2dfYXV0aG9yIiwiZW1haWwiOiJjYXJsb3NAY2FybG9zLW1vbnRveWEubmV0IiwiaWF0IjoxNTE2MjM5MDIyfQ</mark>.<mark style="color:green;">SYZBPIBg2CRjXAJ8vCER0LA\_ENjII1JakvNQoP-Hw6GG1zfl4JyngsZReIfqRvIAEi5L4HV0q7\_9qGhQZvy9ZdxEJbwTxRs\_6Lb-fZTDpW6lKYNdMyjw45\_alSCZ1fypsMWz\_2mTpQzil0lOtps5Ei\_z7mM7M8gCwe\_AGpI53JxduQOaB5HkT5gVrv9cKu9CsW5MS6ZbqYXpGyOG5ehoxqm8DL5tFYaW3lB50ELxi0KsuTKEbD0t5BCl0aCR2MBJWAbN-xeLwEenaqBiwPVvKixYleeDQiBEIylFdNNIMviKRgXiYuAvMziVPbwSgkZVHeEdF5MQP1Oe2Spac-6IfA</mark>

The <mark style="color:red;">header</mark> and <mark style="color:blue;">payload</mark> parts of a JWT are just _base64url-encoded_ JSON objects. The header contains metadata about the token itself, while the payload contains the actual "claims" about the user.

The server that issues the token typically generates the <mark style="color:green;">signature</mark> by hashing the <mark style="color:red;">header</mark> and <mark style="color:blue;">payload</mark>. In some cases, they also encrypt the resulting hash. Either way, this process involves a secret signing key. This mechanism provides a way for servers to verify that none of the data within the token has been tampered with since it was issued.

{% hint style="danger" %}
Even if the signature is robustly verified, whether it can truly be trusted relies heavily on the server's secret key remaining a secret. If this key is leaked in some way, or can be guessed or brute-forced, an attacker can generate a valid signature for any arbitrary token, compromising the entire mechanism.
{% endhint %}

