# Grant Types

The OAuth grant type determines the steps that are involved in the OAuth process. The grant type also affects how the client application communicates with the OAuth service at each stage, including how the access token itself is sent. For this reason, grant types are often referred to as "OAuth flows".

An OAuth service must be configured to support a particular grant type before a client application can initiate the corresponding flow. The client application specifies which grant type it wants to use in the initial authorization request it sends to the OAuth service.

There are several different grant types, each with varying levels of complexity and security considerations. By far the most common Grant types are "authorization code" and "implicit".



{% hint style="warning" %}


_**Authorization Code Grant**: Typically used for server-side applications. It involves the client obtaining an authorization code from the authorization server and exchanging it for an access token._

_**Implicit Grant**: Designed for client-side applications like JavaScript apps running in a web browser. It returns the access token directly to the client without an authorization code exchange._
{% endhint %}



