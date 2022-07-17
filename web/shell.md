# shell

## Reverse and Bind Shell

_**Reverse shells**_ are when the target is forced to execute code that connects back to your computer. On your own computer, you would use one of the tools mentioned in the previous task to set up a listener which would be used to receive the connection. Reverse shells are a good way to bypass firewall rules that may prevent you from connecting to arbitrary ports on the target; however, the drawback is that, when receiving a shell from a machine across the internet, you would need to configure your own network to accept the shell.

_**Bind shells**_ are when the code executed on the target is used to start a listener attached to a shell directly on the target. This would then be opened up to the internet, meaning you can connect to the port that the code has opened and obtain remote code execution that way. This has the advantage of not requiring any configuration on your own network, but may be prevented by firewalls protecting the target.

## Webshell

"Webshell" is a colloquial term for a script that runs inside a webserver (usually in a language such as PHP or ASP) and executes code on the server. Essentially, commands are entered into a webpage -- either through a HTML form, or directly as arguments in the URL -- which are then executed by the script, with the results returned and written to the page. This can be extremely useful if there are firewalls in place, or even just as a stepping stone into a fully-fledged reverse or bind shell.

As PHP is still the most common server-side scripting language, let's have a look at some simple code for this.

In a very basic one-line format:

_<mark style="color:blue;">`<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>`</mark>_

##
