---
layout: post
title: WSGI in 3 Minutes
---

[WSGI](https://www.python.org/dev/peps/pep-3333/)
is a low-level interface between web servers (also called a gateway)
and web applications (also called a framework).

WSGI is very simple:
It defines the interface of just one callable object
`app(environ, start_response)`
that a web application provides to a web server.
(You may observe some relics of CGI here,
such as the `environ` variable.)

Of course,
you should use higher level frameworks like
[Flask](http://flask.pocoo.org/)
or
[Django](https://www.djangoproject.com/)
to develop your WSGI application.
Or use
[Werkzeug](http://werkzeug.pocoo.org/)
or [wsgiref](https://docs.python.org/3.4/library/wsgiref.html)
if you really want to go low level.
See
[here](http://wsgi.readthedocs.org/en/latest/)
and [here](http://docs.python-guide.org/en/latest/scenarios/web/)
for a brief survey of WSGI-compliant frameworks, servers, etc.

Middleware is merely a callable object
that is called by a web server (or another middleware) and
calls a web application (or another middleware).
You could think of layers of middleware as an onion or a Unix pipeline.

WSGI interface is designed to accommodate both
buffering and streaming mode of transmission.
