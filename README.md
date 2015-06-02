# lab4

Introduction
A Web proxy is a program that acts as a middleman between a Web browser and an end server. Instead of
contacting the end server directly to get a Web page, the browser contacts the proxy, which forwards the
request on to the end server. When the end server replies to the proxy, the proxy sends the reply on to the
browser.
Proxies are used for many purposes. Sometimes proxies are used in firewalls, such that the proxy is the only
way for a browser inside the firewall to contact an end server outside. The proxy may do translation on the
page, for instance, to make it viewable on a Web-enabled cell phone. Proxies are also used as anonymizers.
By stripping a request of all identifying information, a proxy can make the browser anonymous to the end
server. Proxies can even be used to cache Web objects, by storing a copy of, say, an image when a request
for it is first made, and then serving that image in response to future requests rather than going to the end
server.
In this lab, you will write a Web proxy that logs requests. You will write a simple sequential proxy that
repeatedly waits for a request, forwards the request to the end server, and returns the result back to the
browser, keeping a log of such requests in a disk file. This will help you understand basics about network
programming and the HTTP protocol.

Logistics
As always, you must work in a group of two or more people. You will need to upload the source code and
have a TA checkoff your code.

Hand Out Instructions
You code template is available for download from the course webpage. Go to the course webpage and
download the proxy.c file.
Start by copying proxy.c to a directory in which you plan to do your work. When compiling proxy.c be
sure to link it with csapp.c and be sure that csapp.h is in the same directory.

Implementing a Sequential Web Proxy
In this part you will implement a sequential logging proxy. Your proxy should open a socket and listen
for a connection request. When it receives a connection request, it should accept the connection, read the
HTTP request, and parse it to determine the name of the end server. It should then open a connection to the
end server, send it the request, receive the reply, and forward the reply to the browser if the request is not
blocked.
Since your proxy is a middleman between client and end server, it will have elements of both. It will act as
a server to the web browser, and as a client to the end server. Thus you will get experience with both client
and server programming.

Logging
Your proxy should keep track of all requests in a log file named proxy.log. Each log file entry should be
a file of the form:
Date: browserIP URL size
where browserIP is the IP address of the browser, URL is the URL asked for, size is the size in bytes
of the object that was returned. For instance:
Sun 27 Oct 2002 02:51:02 EST: 128.2.111.38 http://www.cs.cmu.edu/ 34314
Note that size is essentially the number of bytes received from the end server, from the time the connection
is opened to the time it is closed. Only requests that are met by a response from an end server should be
logged. We have provided the function format log entry in csapp.c to create a log entry in the
required format

Port Numbers
You proxy should listen for its connection requests on the port number passed in on the command line:
unix> ./proxy 15213
You may use any port number p, where 1024 ≤ p ≤ 65536, and where p is not currently being used by any
other system or user services (including other students’ proxies). See /etc/services for a list of the
port numbers reserved by other system services.
Your sequential proxy should correctly accept connections, forward the requests to the end server, and pass
the response back to the browser, making a log entry for each request. Your program should be able to proxy
browser requests to the following Web sites and correctly log the requests:
• http://www.yahoo.com
• http://www.aol.com
• http://www.nfl.com

Hints
• Initially, you should debug your proxy using telnet as the client.
• Later, test your proxy with a real browser. Explore the browser settings until you find “proxies”, then
enter the host and port where you’re running yours. With Netscape, choose Edit, then Preferences,
then Advanced, then Proxies, then Manual Proxy Configuration. In Internet Explorer, choose Tools,
then Options, then Connections, then LAN Settings. Check ’Use proxy server,’ and click Advanced.
Just set your HTTP proxy, because that’s all your code is going to be able to handle.
• Since we want you to focus on network programming issues for this lab, we have provided you with
two additional helper routines: parse uri, which extracts the hostname, path, and port components
from a URI, and format log entry, which constructs an entry for the log file in the proper format