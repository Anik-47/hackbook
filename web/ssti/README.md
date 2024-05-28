---
description: Server Side Template Injection
---

# SSTI

Server-side template injection is a vulnerability that occurs when an attacker can inject malicious code into a template that is executed on the server.

## Detection <a href="#detection" id="detection"></a>

To detect Server-Side Template Injection (SSTI), initially, **fuzzing the template** is a straightforward approach. This involves injecting a sequence of special characters (**`${{<%[%'"}}%\`**) into the template and analyzing the differences in the server's response to regular data versus this special payload. Vulnerability indicators include:

* Thrown errors, revealing the vulnerability and potentially the template engine.
* Absence of the payload in the reflection, or parts of it missing, implying the server processes it differently than regular data.
* **Plaintext Context**: Distinguish from XSS by checking if the server evaluates template expressions (e.g., `{{7*7}}`, `${7*7}`).
* **Code Context**: Confirm vulnerability by altering input parameters. For instance, changing `greeting` in `http://vulnerable-website.com/?greeting=data.username` to see if the server's output is dynamic or fixed, like in `greeting=data.username}}hello` returning the username.



## Impact

* Remote Code Execution
* Data Leakage
* DOS
* Cross Site Scripting

## Web Template Engines:

* FreeMarker - Java-based template engine
* Velocity - Java-based template engine
* Smarty - PHP Template engine
* Twig - PHP Template engine
* Jade - Node.js Template engine
* Jinja2 - Python/Flask Template engine

## Mitigation

* Input Validation
* Use Safe Templates
* Sandboxing

## [Expression Language (EL) Injection](expression-language-injection.md)

EL injection is a specific type of template injection where the vulnerability arises from the improper handling of expression language input. EL is used to evaluate expressions within templates, and if user input is unsafely embedded within these expressions, it can lead to similar security issues as template injection.

### **Example of EL Injection**

Consider a JavaServer Pages (JSP) application using Java Unified Expression Language (UEL):

```java
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%
  String userInput = request.getParameter("input");
%>
<p>Result: ${userInput}</p>
```

If `userInput` is not sanitized, an attacker could inject an expression like `${7*7}` which would be evaluated by the UEL engine, resulting in "Result: 49".



