Curl for all your web
======================

In this chapter we will learn about a very special command, `curl`. It is used
to trasfer data over network, it is written by `Daniel Stenberg
<https://daniel.haxx.se/>`_. It is most probably one of the highest used
software in the world, you can find it in your servers, cars and also in
television sets.

In this chapter we will learn a few example use cases.

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
`https://kushaldas.in/test.html`, by default curl shows the output on STDOUT.

Downloading the file
---------------------

You can use `-o` flag to download a file and save it with the given filename.

::

    $ curl https://kushaldas.in/test.html -o /tmp/download.html
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   125  100   125    0     0    295      0 --:--:-- --:--:-- --:--:--   296


Download with the same name
----------------------------

Use `-O` flag to download the save the file with the same basename from the given URL.

::

    $ curl -O https://kushaldas.in/test.html
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   125  100   125    0     0    295      0 --:--:-- --:--:-- --:--:--   296
    $ ls -l test.html
    .rw-r--r--@ 125 kdas 14 Apr 20:45 test.htm

Here the file is saved in the current directory as `test.html`.

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


Here the lines with `>` at starting showing the headers in the request, and `<`
shows the headers in the response.

For the rest of the chapter we will keep using `httpbin.org <https://httpbin.org>`_,
which is a service run by `Kenneth Reitz <https://twitter.com/kennethreitz42>`_.
The service returns JSON as output.

Doing POST request using curl
-----------------------------

We can do `HTTP POST <https://en.wikipedia.org/wiki/POST_(HTTP)>`_ requests
using curl in two different ways. Using `-d` flag for simple form submission
style using `application/x-www-form-urlencoded` where each form names & values
are marked with  `=` and separate by `&`.

You can also use `--form/-F` for `multipart/form-data` where we can upload
files or send in large amount of binary data.

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

Above we had to pass both the form fields using `--form` twice.

.. note:: You can read the `SPEC
   <https://www.w3.org/TR/html401/interact/forms.html#h-17.13.4>`_ to learn
   about the difference.


