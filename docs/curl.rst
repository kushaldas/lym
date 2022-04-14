Curl for all your web
======================

In this chapter we will learn about a very special command, `curl`. It is used
to trasfer data over network, it is written by `Daniel Stenberg
<https://daniel.haxx.se/>_`. It is most probably one of the highest used
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


Downloading the file
---------------------

You can use `-o` flag to download a file and save it with the given filename.

::

    $ curl https://kushaldas.in/test.html -o /tmp/test.html
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   125  100   125    0     0    295      0 --:--:-- --:--:-- --:--:--   296

