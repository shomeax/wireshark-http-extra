Introduction
============

This is a Wireshark Lua dissector that adds few useful properties to the existing HTTP dissector.

 1. It connects HTTP responses to their respective HTTP requests, displaying original request' URI, Host, Method/Version.

 2. It tries to calculate full URL from the request and displays it in the separate properties tree.

Installation
============
Linux
-----
Copy or hardlink http_extra directory to your config directory, e.g. ~/.wireshark/

Create init.lua there if it does not exists.

Add a dofile("http_extra/init.lua") to your wireshark init.lua at ~/.wireshark/init.lua

Windows
-------
Copy http_extra contents to user settings directory. That depends on your Windows version.
 * Vista - C:\Users\[username]AppData\Roaming\Wireshark
 * XP/2000 - C:\Documents and Settings\[username]\Application Data\Wireshark

Create init.lua file there if it does not exists

Sometimes for Windows installation you must edit system wide init.lua to enable it and disable superuser check.
That apply because frequently Windows users run as root.

To accomplish this:
 * open C:\Program Files\Wireshark\init.lua
 * find string 'disable_lua = true' and replace it with 'disable_lua = false'.
 * replace 'run_user_scripts_when_superuser = false' with 'run_user_scripts_when_superuser = true'
 * replace 'if running_superuser then' with 'if 0 and running_superuser'

Otherwise you can use Lua/Evaluate menu to run arbitrary dofile("C:\\Progra~1\\Wireshark\\plugins\\1.4.6\\http_response_patcher.lua") command.

Usage
=====
Capture some HTTP traffic. Clicking on HTTP packet (e.g. with 'HTTP' in Protocol column) should reveal tree
'Advanced HTTP data' with 'Request URL' property in the Packet Details list. If the packet you've clicked is HTTP response packet 
(e.g. Info columnt reads 'HTTP/1.1 200 OK' or similar), then 'Upstream HTTP Request' with 'Request URI', 'Request Version', 
'Request Method' and 'Host' properties within it).

You can check out sample screenshots in the 

Known issues and limitations
============================
It consumes memory creating a copy of fields for each HTTP request. Generally it should not be a problem, 
but for very big capture dump or live capture this may result in slow processing.

It does not handle packets residing in single TCP packet. This is original Wireshark' dissector issue and had been reported.

Tested with Wireshark 1.4.6 at Ubuntu 11.04, Windows Vista Pro

References
==========
1. http://www.wireshark.org/Lua
2. http://gplus.to/shomeax
   mailto:ShomeaX@gmail.com

