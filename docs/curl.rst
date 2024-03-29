Curl! For all your web needs
============================

In this chapter we will learn about a very special command, `curl`. 
It is used to trasfer data over network. Written by `Daniel Stenberg
<https://daniel.haxx.se/>`_, it is most probably one of the highest used
pieces of software in the world. You can find it in your servers, cars and also in
television sets.

In this chapter we will learn a few example use cases. In case you are new to
HTTP land, you can watch `this video
<https://www.youtube.com/watch?v=iYM2zFP3Zn0>`_ to learn more about the protocol.

Viewing a file
--------------

::

    $ curl https://kushaldas.in/test.html
    <html>
        <head>
            <title>Test page</title>
        </head>
        <body>
            This is a test page. You can view it via curl.
        </body>
    </html>


Here we are reading the content of the file located at URL
`https://kushaldas.in/test.html`. By default curl shows the output on STDOUT.

Downloading the file
---------------------

You can use the `-o` flag to download a file and save it to disk, with the given filename.

::

    $ curl https://kushaldas.in/test.html -o /tmp/download.html
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   125  100   125    0     0    295      0 --:--:-- --:--:-- --:--:--   296


Download with the same name
----------------------------

Use the `-O` flag to download and save the file with the same basename from the given URL.

::

    $ curl -O https://kushaldas.in/test.html
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   125  100   125    0     0    295      0 --:--:-- --:--:-- --:--:--   296
    $ ls -l test.html
    .rw-r--r--@ 125 kdas 14 Apr 20:45 test.html

Here the file is saved in the current directory, as `test.html`.

Making a POST request using curl
--------------------------------

We can make `HTTP POST <https://en.wikipedia.org/wiki/POST_(HTTP)>`_ requests
using curl in two different ways. 

1. Using the `-d` flag, for simple form submissions
(or using `application/x-www-form-urlencoded`), where each form name & its values
are marked with  `=` and separate by `&`.

2. You can also use `--form/-F` for `multipart/form-data` where we can upload
files or send in large amounts of binary data.

::

    $ curl -d "name=kushal&lang=Python" https://httpbin.org/post
    {
      "args": {}, 
      "data": "", 
      "files": {}, 
      "form": {
        "lang": "Python", 
        "name": "kushal"
      }, 
      "headers": {
        "Accept": "*/*", 
        "Content-Length": "23", 
        "Content-Type": "application/x-www-form-urlencoded", 
        "Host": "httpbin.org", 
        "User-Agent": "curl/7.79.1", 
        "X-Amzn-Trace-Id": "Root=1-625a7542-3994f1a24d276db65e59c88f"
      }, 
      "json": null, 
      "origin": "193.138.218.212", 
      "url": "https://httpbin.org/post"
    }

    $ curl --form name=kushal --form lang=Python https://httpbin.org/post
    {
      "args": {}, 
      "data": "", 
      "files": {}, 
      "form": {
        "lang": "Python", 
        "name": "kushal"
      }, 
      "headers": {
        "Accept": "*/*", 
        "Content-Length": "244", 
        "Content-Type": "multipart/form-data; boundary=------------------------870c3eede45c997d", 
        "Host": "httpbin.org", 
        "User-Agent": "curl/7.79.1", 
        "X-Amzn-Trace-Id": "Root=1-625a755e-2b91ece7042683285bd91332"
      }, 
      "json": null, 
      "origin": "193.138.218.212", 
      "url": "https://httpbin.org/post"
    }

In the second example above, we passed in each form field, using `--form` twice.

.. note:: You can read the `SPEC
   <https://www.w3.org/TR/html401/interact/forms.html#h-17.13.4>`_ to learn more 
   about the differences between the options and the reasoning behind each approach.

We can also put all the data into a file and post the contents of the file.

::

    $ cat data.txt 
    name=kushal&lang=Python
    $ curl -d @data.txt https://httpbin.org/post
    {
      "args": {}, 
      "data": "", 
      "files": {}, 
      "form": {
        "lang": "Python", 
        "name": "kushal"
      }, 
      "headers": {
        "Accept": "*/*", 
        "Content-Length": "23", 
        "Content-Type": "application/x-www-form-urlencoded", 
        "Host": "httpbin.org", 
        "User-Agent": "curl/7.79.1", 
        "X-Amzn-Trace-Id": "Root=1-62795cf0-2f1afd31178c28137be111d6"
      }, 
      "json": null, 
      "origin": "193.138.218.212", 
      "url": "https://httpbin.org/post"
    }


Following redirection
----------------------

One can use `-L` option to tell curl to follow any **3xx** redirect from the
server. To see this, first we call `curl` with `-I` to `http://kushaldas.in`.
This will return a *302* redirection to the `https://kushaldas.in` site. In the
second run, we will additionally provide a `-L` flag, so that curl will follow the
redirection. `-I` allows curl to do a `HEAD` request to the server.

::

    $ curl -I http://kushaldas.in
    HTTP/1.1 302 Moved Temporarily
    Server: nginx/1.18.0
    Date: Sat, 16 Apr 2022 15:03:02 GMT
    Content-Type: text/html
    Content-Length: 145
    Connection: keep-alive
    Location: https://kushaldas.in/


    $ curl -LI http://kushaldas.in
    HTTP/1.1 302 Moved Temporarily
    Server: nginx/1.18.0
    Date: Sat, 16 Apr 2022 15:03:06 GMT
    Content-Type: text/html
    Content-Length: 145
    Connection: keep-alive
    Location: https://kushaldas.in/

    HTTP/2 200 
    server: nginx/1.18.0
    date: Sat, 16 Apr 2022 15:03:06 GMT
    content-type: text/html; charset=utf-8
    content-length: 27890
    last-modified: Fri, 01 Apr 2022 13:35:38 GMT
    etag: "6246ffaa-6cf2"
    strict-transport-security: max-age=31536000
    onion-location: https://kushal76uaid62oup5774umh654scnu5dwzh4u2534qxhcbi4wbab3ad.onion
    permissions-policy: interest-cohort=()
    x-frame-options: DENY
    x-content-type-options: nosniff
    referrer-policy: strict-origin
    accept-ranges: bytes


Example: To view github's pull request patch
---------------------------------------------

We can use the options we already learned to get any patch from github. When I
started writing this chapter, I did an `initial PR
<https://github.com/kushaldas/lym/pull/58>`_. Let us first see what happens
when we just try to get the page.

::

    $ curl https://github.com/kushaldas/lym/pull/58 | less

You will notice a lot of HTML/JS, but we want to see the actual code diff. 
We can try to do that by appending `.diff` to the end of the URL.

::

    $ curl https://github.com/kushaldas/lym/pull/58.diff
    <html><body>You are being <a href="https://patch-diff.githubusercontent.com/raw/kushaldas/lym/pull/58.diff">redirected</a>.</body></html>

We see that it is a redirect.
Now we can use `-LO` flag to follow the redirect, and also save the patch in `58.diff`.

::

    $ curl -LO https://github.com/kushaldas/lym/pull/58.diff


Viewing more details about the transfer
---------------------------------------

We can use `--write-out` flag to get more details about the transfer. It prints
them after the main output, based on the variable we pass in. For example, we can
check the `HTTP status code` in both the calls.

::

    $ curl -s --write-out '%{http_code}' http://kushaldas.in -o /dev/null
    302
    $ curl -s --write-out '%{http_code}' https://kushaldas.in -o /dev/null
    200

You can also pass `--write-out '%{json}'` to see the all the different details as
JSON. Read `curl` man page for more details.


Making multiple requests at once
--------------------------------

We can use `--next` flag to make multiple requests one after another (as totally separate
operations). Note that, it resets all of the settings/command line options used before.


::

    $ curl --user-agent "ACAB/1.0" http://httpbin.org/get --next  https://httpbin.org/get
    {
      "args": {}, 
      "headers": {
        "Accept": "*/*", 
        "Host": "httpbin.org", 
        "User-Agent": "ACAB/1.0", 
        "X-Amzn-Trace-Id": "Root=1-625b0986-39eae16e7144c2ec7601b697"
      }, 
      "origin": "193.138.218.212", 
      "url": "http://httpbin.org/get"
    }
    {
      "args": {}, 
      "headers": {
        "Accept": "*/*", 
        "Host": "httpbin.org", 
        "User-Agent": "curl/7.79.1", 
        "X-Amzn-Trace-Id": "Root=1-625b0987-6bc8f2a30c2fef0037c7d629"
      }, 
      "origin": "193.138.218.212", 
      "url": "https://httpbin.org/get"
    }


In the above example you can see the `User-Agent` value taking effect, only in the
first operation, but not in the second one.

Inspecting HTTP headers
-----------------------

You can use `-v` flag to inspect the HTTP headers in a request/response.

::

    $ curl -v http://httpbin.org/get
    *   Trying 54.91.120.77:80...
    * Connected to httpbin.org (54.91.120.77) port 80 (#0)
    > GET /get HTTP/1.1
    > Host: httpbin.org
    > User-Agent: curl/7.79.1
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 200 OK
    < Date: Fri, 15 Apr 2022 10:03:05 GMT
    < Content-Type: application/json
    < Content-Length: 256
    < Connection: keep-alive
    < Server: gunicorn/19.9.0
    < Access-Control-Allow-Origin: *
    < Access-Control-Allow-Credentials: true
    < 
    {
      "args": {}, 
      "headers": {
        "Accept": "*/*", 
        "Host": "httpbin.org", 
        "User-Agent": "curl/7.79.1", 
        "X-Amzn-Trace-Id": "Root=1-625942d9-163a40480c9aea0470fd9c2e"
      }, 
      "origin": "185.195.233.166", 
      "url": "http://httpbin.org/get"
    }
    * Connection #0 to host httpbin.org left intact


Here the lines with `>` at the start of the line, show the headers in the request, while 
those with `<` show the headers in the response.

For the rest of this chapter, we will keep using `httpbin.org <https://httpbin.org>`_,
which is a service run by `Kenneth Reitz <https://twitter.com/kennethreitz42>`_.
The service returns JSON as output.

Let's say you want to only view the headers, and not the actual
file/URL content. You can use `-s` and `-o /dev/null` as flags to do so

::


    $ curl -s -v http://httpbin.org/get -o /dev/null
    *   Trying 52.7.224.181:80...
    * Connected to httpbin.org (52.7.224.181) port 80 (#0)
    > GET /get HTTP/1.1
    > Host: httpbin.org
    > User-Agent: curl/7.79.1
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 200 OK
    < Date: Sat, 16 Apr 2022 09:18:46 GMT
    < Content-Type: application/json
    < Content-Length: 256
    < Connection: keep-alive
    < Server: gunicorn/19.9.0
    < Access-Control-Allow-Origin: *
    < Access-Control-Allow-Credentials: true
    < 
    { [256 bytes data]
    * Connection #0 to host httpbin.org left intact


Adding new HTTP headers
-----------------------

To learn about this feature of `curl` first we will try to access a URL with a `GET` request. We will inspect the status code returned by the server,
and also the headers.

::

    $ curl -s -v http://httpbin.org/bearer -o /dev/null
    *   Trying 54.90.70.44:80...
    * Connected to httpbin.org (54.90.70.44) port 80 (#0)
    > GET /bearer HTTP/1.1
    > Host: httpbin.org
    > User-Agent: curl/7.79.1
    > Accept: */*
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 401 UNAUTHORIZED
    < Date: Wed, 20 Apr 2022 07:41:25 GMT
    < Content-Type: text/html; charset=utf-8
    < Content-Length: 0
    < Connection: keep-alive
    < Server: gunicorn/19.9.0
    < WWW-Authenticate: Bearer
    < Access-Control-Allow-Origin: *
    < Access-Control-Allow-Credentials: true
    < 
    * Connection #0 to host httpbin.org left intact

It says `401 UNAUTHORIZED`. Now, if we check `the documentation
<http://httpbin.org/#/Auth/get_bearer>`_, it tells us to send in an `Authorization`
header with a bearer token. Which generally, is a random value depending on the
server implementation (random, but only for actual authenticated users). We
will try to send in `123456` as token using the `-H` flag. You can pass
multiple such headers by using the `-H` multiple times.

::

    $ curl -H "Authorization: Bearer 123456" -s -v http://httpbin.org/bearer -o /dev/null
    *   Trying 35.169.55.235:80...
    * Connected to httpbin.org (35.169.55.235) port 80 (#0)
    > GET /bearer HTTP/1.1
    > Host: httpbin.org
    > User-Agent: curl/7.79.1
    > Accept: */*
    > Authorization: Bearer 123456
    > 
    * Mark bundle as not supporting multiuse
    < HTTP/1.1 200 OK
    < Date: Wed, 20 Apr 2022 07:46:09 GMT
    < Content-Type: application/json
    < Content-Length: 50
    < Connection: keep-alive
    < Server: gunicorn/19.9.0
    < Access-Control-Allow-Origin: *
    < Access-Control-Allow-Credentials: true
    < 
    { [50 bytes data]
    * Connection #0 to host httpbin.org left intact

Curl video/talk
----------------

In `this video <https://www.youtube.com/watch?v=V5vZWHP-RqU>`_ Daniel Stenberg himself
explained various features of curl, the talk is from August, 2023.

Curl book
----------

If you want to know more, there is an `amazing online book
<https://everything.curl.dev/project>`_ to read. The man page of `curl` also
has a lot of details.

