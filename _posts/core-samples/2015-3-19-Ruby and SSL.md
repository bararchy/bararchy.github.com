---
layout: post
category : experiences
tags : [ruby, development, ssl]
---

# SSL, what is that ?

First of all before trying to explain the usage of SSL in Ruby or even in other cases, we first must understand what SSL is and why we should be using it.<br>

SSL stands for 'Secure Socket Layer' and it does exactly what it sounds like, allowing you to pass information from one socket to the other using a secured layer that 'wraps' the data passed.<br>

The specific SSL protocol is old, how old you ask ?, Version 2 of the SSL protocol was released in February 1995 by Netscape, we are now at 2015 (as of writing), this means its 20 years old.<br>

SSL has 3 versions:<br>

*  Version 1 (SSLv1) - Wasn't even released to the public because it had serious security flaws in the protocol itself.
*  Version 2 (SSLv2) - Released in February 1995, and contained a number of security flaws which ultimately led to the design of SSL version 3.0.
*  Version 3 (SSLv3) - Released in 1996, represented a complete redesign of the protocol, Newer versions of SSL/TLS are based on SSL 3.0.

<br>
Current SSL State:<br>
As of 2014 the 3.0 version of SSL is considered insecure as it is vulnerable to the POODLE attack that affects all block ciphers in SSL; and RC4, the only non-block cipher supported by SSL 3.0, is also feasibly broken as used in SSL 3.0.
<br>
Now, you must be wondering, why would I even suggest we use a broken and old protocol to secure our data in transport ? <br>

# TLS, SSL's younger brother

TLS stands for 'Transport Layer Security' and it was developed in January 1999 as an upgrade of SSL Version 3.0.<br>
TLS like SSL has 3 versions (one more will be released soon):<br>

*  Version 1 (TLSv1.0) - The differences between this protocol and SSL 3.0 are not dramatic, but they are significant enough to preclude interoperability between TLS 1.0 and SSL 3.0.
*  Version 2 (TLSv1.1) - TLS 1.1 was defined in RFC 4346 in April 2006, it has more security features and some security issues were addressed.
*  Version 3 (TLSv1.2) - TLS 1.2 was defined in RFC 5246 in August 2008, it too has more security focused implementation.

<br>
TLS is currently more or less secured, most of the issues that affect it are more related to specific software implementation then the protocol it self.<br>

So from now on, when I say SSL I actually mean 'TLS'
-----------------------------------------------

# How do we use SSL in Ruby ?

I'm glad you asked, Ruby is usually very simple and straight forward language, but, SSL is not, Ruby doesn't have any pure-ruby SSL library and is using the C-Extension of 'OpenSSL' the Open-source SSL Linux-Unix library.<br>
This means, it will act faster, but will be a little less convenient to work with.<br>

First thing First - require OpenSSL<br>

~~~
require 'openssl'
~~~
{: .language-ruby}

The 'openssl' ruby library holds all we need in order to use OpenSSL for our code in regards to SSL, but doesn't SSL stands for secured socket layer ? well yes it does !<br>

~~~
require 'socket'
~~~
{: .language-ruby}

Now, after requiring the 'openssl' and 'socket' libraries we can create a basic client side connection to a secured site.<br>

~~~
require 'socket'
require 'openssl'

# Basic TCP Socket to connect to our test server (sorry Google)
tcp_socket = TCPSocket.new('google.com', 443)
# Lets upgrade this to a SSL socket
ssl_socket = OpenSSL::SSL::SSLSocket.new(tcp_socket, ssl_context)
~~~
{: .language-ruby}

Ok, stop everything, what is this ssl_context !?<br>
I'm so glad you asked, the ssl_context object (or what ever you want to call it) holds our SSL socket configurations, here we can decide what Ciphers we want to allow for our handshakes, what SSL\TLS protocols we want to support, etc..<br>
How do we configure it:<br>

~~~
# First we initialize the object
ssl_context = OpenSSL::SSL::SSLContext.new
# looking at our class using 'p' we can see the following:

# Those are our configurations for the SSL Socket class
@ca_file=nil,
@ca_path=nil,
@cert=nil,
@cert_store=nil,
@client_ca=nil,
@client_cert_cb=nil,
@extra_chain_cert=nil,
@key=nil,
@npn_protocols=nil,
@npn_select_cb=nil,
@options=nil,
@renegotiation_cb=nil,
@servername_cb=nil,
@session_get_cb=nil,
@session_id_context=nil,
@session_new_cb=nil,
@session_remove_cb=nil,
@timeout=nil,
@tmp_dh_callback=nil,
@verify_callback=nil,
@verify_depth=nil,
@verify_mode=nil>

# Using ssl_context.ciphers we can see what ciphers our local openssl lib supports
[["ECDHE-RSA-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["ECDHE-ECDSA-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["ECDHE-RSA-AES256-SHA384", "TLSv1/SSLv3", 256, 256],
 ["ECDHE-ECDSA-AES256-SHA384", "TLSv1/SSLv3", 256, 256],
 ["ECDHE-RSA-AES256-SHA", "TLSv1/SSLv3", 256, 256],
 ["ECDHE-ECDSA-AES256-SHA", "TLSv1/SSLv3", 256, 256],
 ["SRP-DSS-AES-256-CBC-SHA", "TLSv1/SSLv3", 256, 256],
 ["SRP-RSA-AES-256-CBC-SHA", "TLSv1/SSLv3", 256, 256],
 ["SRP-AES-256-CBC-SHA", "TLSv1/SSLv3", 256, 256],
 ["DH-DSS-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["DHE-DSS-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["DH-RSA-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["DHE-RSA-AES256-GCM-SHA384", "TLSv1/SSLv3", 256, 256],
 ["DHE-RSA-AES256-SHA256", "TLSv1/SSLv3", 256, 256],
 ["DHE-DSS-AES256-SHA256", "TLSv1/SSLv3", 256, 256],
 ["DH-RSA-AES256-SHA256", "TLSv1/SSLv3", 256, 256],
 ["DH-DSS-AES256-SHA256", "TLSv1/SSLv3", 256, 256],
 ["DHE-RSA-AES256-SHA", "TLSv1/SSLv3", 256, 256],
 ["DHE-DSS-AES256-SHA", "TLSv1/SSLv3", 256, 256]]
~~~
{: .language-ruby}

So, now we have an SSL Context object, lets use it to connect to Google already !

~~~
require 'socket'
require 'openssl'

tcp_socket = TCPSocket.new('google.com', 443)
ssl_context = OpenSSL::SSL::SSLContext.new
ssl_socket = OpenSSL::SSL::SSLSocket.new(tcp_socket, ssl_context)
ssl_socket.connect # This little bit tells it to initiate the SSL handshake over the TCP socket

# we can now use the ssl_socket as we would use the regular TCP socket

ssl_socket.write "GET /index.htm HTTP/1.1\r\n\r\n"

# we use 16384 as this is the smallest SSL block we can get, getting less bits will result in corrupted package
a = ssl_socket.read_nonblock(16384)

puts a

HTTP/1.1 404 Not Found
Content-Type: text/html; charset=UTF-8
Content-Length: 1434
Date: Thu, 19 Mar 2015 10:17:07 GMT
Server: GFE/2.0

<!DOCTYPE html>
<html lang=en>
  <meta charset=utf-8>
  <meta name=viewport content="initial-scale=1, minimum-scale=1, width=device-width">
  <title>Error 404 (Not Found)!!1</title>
  <style>
</style>
</html>
...


~~~
{: .language-ruby}

Cool! right ? we connected to Google and got the index page :) but wait, was this connection secured ?<br>
Well... it depends, on Google, and why is that ? because we configured a weak client, we used the default parameters and allowed ourselves to use whatever we get, do we like it ? NO ! No we don't ! <br>

# Hardening the SSL Context
As I showed before, the SSL Context is the Object that allows us to configure and control how the ssl_socket will behave.<br>
So first, lets allow only strong ciphers to be used:<br>

~~~
ciphers = '!ADH:!RC4:!aNULL:!MD5:!EXPORT:!SSLv2:HIGH'
~~~
{: .language-ruby}

What do we have here... you may think this looks nothing like ruby... and you will be right.<br>
Those are direct configurations using the C-Extension of the OpenSSL lib (I warned you didn't I ?)<br>
What those basically say is this:<br>
the '!' is for no, as in don't use this.

*  !ADH - Dont use anonymous Diffie-Hellman communications (in which neither party is authenticated. Note that this mode is vulnerable to man-in-the-middle attacks.)
*  !RC4 - Don't use the RC4 stream cipher, as it is known to be a weak cipher.
*  !aNULL - Don't use the cipher suites offering no authentication. This is currently the anonymous DH algorithms and anonymous ECDH algorithms. These cipher suites are vulnerable to a "man in the middle" attack and so their use is normally discouraged.
*  !MD5 - Don't use the MD5 stream cipher.
*  !EXPORT - Don't use any export based ciphers (those are vulnerable to the resent FREAK attack).
*  !SSLv2 - Don't use SSL Version 2 protocol ciphers
*  High - Use only ciphers which regarded as high security (high bit number)

<br>
Now after we know what ciphers we want, lets configure them into the ssl_context

~~~
ciphers = '!ADH:!RC4:!aNULL:!MD5:!EXPORT:!SSLv2:HIGH'
ssl_context.ciphers = ciphers
~~~
{: .language-ruby}

Yeha, as simple as that.<br>
Now, lets get rid of the pesky SSLv3 protocol:<br>

~~~
# Don't use SSLv3
no_ssl_3 = OpenSSL::SSL::OP_NO_SSLv3
# Don't use SSLv2
no_ssl_2 = OpenSSL::SSL::OP_NO_SSLv2
# Don't use compression (CRIME CVE-2012-4929)
no_ssl_compression = OpenSSL::SSL::OP_NO_COMPRESSION

ssl_options = no_ssl_2 + no_ssl_3 + no_ssl_compression

ssl_context.options = ssl_options
~~~
{: .language-ruby}

Did you see what I did there ? this + that + this one too, why did I added all those object together ? well, because they are integers, each with it's own specific number so the OpenSSL lib knows what we want and don't want to use.<br>

The next step is to configure the Certificate options, those options are critical, why ? so we can defend ourselves from MITM attacks, the certificate part of SSL is the core logic behind the peer-verification process, yes, my connection is encrypted but who is receiving the data ?<br>
What we want is to configure our system's certificate store for the ssl_context.<br>

~~~
# Initialize the Store object
cert_store = OpenSSL::X509::Store.new
# Tell the store to configure the default OS store path
cert_store.set_default_paths
# Add the store to the ssl_context object
ssl_context.cert_store = cert_store
~~~
{: .language-ruby}

*  Important !

Don't forget to change the ssl_context.verify_mode <br>

~~~
ssl_context.verify_mode = OpenSSL::SSL::VERIFY_PEER
~~~
{: .language-ruby}

So, what we have now ? we have a ssl_context object with strong ciphers and even stronger protocol specific options, and also a check for invalid certificates.<br>
This is all nice and dandy, but it's just a client, which means we cannot receive SSL connections with it, only to initiate (we are not the server)<br>

And thats kind of what you need to know on the basic usage of SSL and Ruby, if I will get requests from people, I'll add a server side example and explain the differences between the two. <br>

Thanks for reading !<br>

