# Oauth

Oauth is a way to provide websites and applications access to information present in other platforms such as Google, Amazon and Microsoft

## &#x20;How OAuth Works

* **Client application** - The website or web application that wants to access the user's data.
* **Resource owner** - The user whose data the client application wants to access.
* **OAuth service provider** - The website or application that controls the user's data and access to it. They support OAuth by providing an API for interacting with both an authorization server and a resource server.

{% hint style="info" %}
There are numerous different ways that the actual OAuth process can be implemented. These are known as OAuth "_<mark style="color:blue;">flows</mark>_" or "_<mark style="color:blue;">grant types</mark>_".
{% endhint %}

1. The client application requests access to user's data, specifying which grant type they want to use and what kind of access they want.
2. The user is prompted to log in to the OAuth service and give their consent for the requested access.
3. The client application receives a unique access token that proves they have permission from the user to access the requested data. Exactly how this happens varies significantly depending on the grant type.
4. The client application uses this access token to make API calls fetching the relevant data from the resource server.
