# Expression Language Injection

Expression Language (EL) Injection is a type of injection attack where an attacker is able to inject and execute arbitrary expressions in the context of the server-side expression language. This type of attack can lead to various security issues, including information disclosure, remote code execution, and full system compromise.

## **Understanding Expression Language (EL)**

Expression Language is a scripting language used in various web technologies to dynamically access and manipulate application data. It is commonly used in Java-based web applications (e.g., JavaServer Pages (JSP), JavaServer Faces (JSF)), and other frameworks like Spring and Thymeleaf.

## **How EL Injection Works**

EL Injection occurs when user-controlled input is directly embedded into an EL expression without proper validation or sanitization. This can allow an attacker to manipulate the expression to execute unintended operations.

## **Example Scenario**

Consider a web application that uses JSP and allows users to input their name, which is then displayed back on a web page. The JSP code might look like this:

```javadoc
<p>Welcome, ${param.username}</p>
```

If the `username` parameter is not properly sanitized, an attacker could provide a malicious input, such as `${7*7}`, which would be evaluated and displayed as `49` instead of the intended username.

## **Potential Impacts of EL Injection**

The severity of EL Injection can vary depending on the context and capabilities of the EL implementation. Some potential impacts include:

1. **Information Disclosure**: An attacker can read sensitive data from server-side objects.
   * Example: `${applicationScope}` can reveal application-scope variables.
2. **Remote Code Execution**: In some cases, an attacker can execute arbitrary code on the server.
   * Example: `${Runtime.getRuntime().exec('cat /etc/passwd')}`.
3. **Bypass Authentication and Authorization**: Manipulating EL expressions to bypass security checks.
4. **Denial of Service (DoS)**: Crafting expressions that consume excessive resources, leading to a denial of service.

## **Mitigation**

1. **Input Validation and Sanitization**:
   * Always validate and sanitize user input before embedding it into EL expressions.
   * Use a whitelist approach to allow only safe characters and patterns.
2. **Escape User Input**:
   * Escape special characters in user input to prevent it from being interpreted as an EL expression.
   * Use library functions or frameworks that provide automatic escaping.
3. **Use Security Libraries**:
   * Leverage security libraries and frameworks that offer built-in protections against EL Injection.
4. **Context-Specific Protection**:
   * Ensure that user input is appropriately isolated in different contexts.
   * Avoid using untrusted input directly in EL expressions.
5. **Least Privilege Principle**:
   * Restrict the capabilities of the EL environment to the minimum necessary.
   * Disable access to sensitive classes and methods that are not required.
6. **Regular Security Audits**:
   * Perform regular code reviews and security audits to identify and fix potential EL Injection vulnerabilities.

## **Real-World Examples**

**Spring Framework CVE-2017-4971**: A critical EL Injection vulnerability in the Spring Framework allowed attackers to execute arbitrary code by crafting malicious EL expressions.

**Apache Struts2 S2-045**: An EL Injection vulnerability in Apache Struts2 could be exploited to execute arbitrary commands on the server.

## **Conclusion**

Expression Language Injection is a critical security vulnerability that can lead to severe consequences, including remote code execution and information disclosure. To protect against EL Injection, developers should rigorously validate and sanitize user inputs, employ escaping mechanisms, and adhere to security best practices. Regular security assessments and the use of secure frameworks can further mitigate the risks associated with EL Injection.
