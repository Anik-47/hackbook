# Host Header Injection

The purpose of the HTTP Host header is to help identify which back-end component the client wants to communicate with. If requests didn't contain Host headers, or if the Host header was malformed in some way, this could lead to issues when routing incoming requests to the intended application.\
\
Nowadays, it is common for multiple websites and applications to be accessible at the same IP address due to **virtual hosting**, **reverse proxy** or a **load balancer**.



## What is an HTTP Host header attack? <a href="#what-is-an-http-host-header-attack" id="what-is-an-http-host-header-attack"></a>

HTTP Host header attacks exploit vulnerable websites that handle the value of the Host header in an unsafe way. If the server  trusts the Host header, and fails to validate it properly, an attacker may be able to use this input to inject harmful payloads.&#x20;

{% hint style="info" %}
Web applications typically don't know what domain they are deployed on unless it is manually specified in a configuration. When they need to know the current domain to generate an absolute URL included in an email, they may refer to the Host header:

`<a href="https://_SERVER['HOST']/support">Contact support</a>`
{% endhint %}

