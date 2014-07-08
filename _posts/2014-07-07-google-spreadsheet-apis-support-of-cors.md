---
layout: post
title: Google Spreadsheet API's Support of CORS
---

TL;DR it is broken as of July 7, 2014.

[CORS](http://www.w3.org/TR/cors/), Cross Origin Resource Sharing,
is a mechanism for sharing resources across domains.
It is a modern alternative to JSONP, which only supports `GET` method.

I tested two `GET` API calls with the Javascript snippet below:

{% highlight javascript %}
xhr = new XMLHttpRequest();
xhr.open('GET', URL, true);
xhr.setRequestHeader('Authorization',
  'Bearer ' + OAuth2AccessToken);
xhr.send();
{% endhighlight %}

The
[list-of-spreadsheets](https://developers.google.com/google-apps/spreadsheets/#retrieving_a_list_of_spreadsheets)
call supported CORS, but the
[list-of-worksheets](https://developers.google.com/google-apps/spreadsheets/#retrieving_information_about_worksheets)
call didn't; I encountered this error:

> XMLHttpRequest cannot load https://spreadsheets.google.com/feeds/worksheets/KEY/private/full. No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://localhost:8000' is therefore not allowed access.

I tested list-of-worksheets with `curl` (you may find Chrome's 'Copy as cURL' handy here).
I simplified the parameters to `curl` in this post,
but the result was the same even if I used the full parameters from Chrome.

{% highlight bash %}
curl -v \
    -H "Accept: */*" \
    -H "Authorization: Bearer ${ACCESS_TOKEN}" \
    -H "Origin: http://localhost:8000" \
    "https://spreadsheets.google.com/feeds/worksheets/${KEY}/private/full"
{% endhighlight %}

Google server did actually respond the requested contents.
It was just that Google server did not respond CORS headers.
Excerpt from `curl` output:

    > GET /feeds/worksheets/.../private/full HTTP/1.1
    > User-Agent: curl/7.29.0
    > Host: spreadsheets.google.com
    > Accept: */*
    > Authorization: Bearer ...
    > Origin: http://localhost:8000
    >
    < HTTP/1.1 200 OK
    < Content-Type: application/atom+xml; charset=UTF-8
    < X-Robots-Tag: noindex, nofollow, nosnippet
    < Expires: Tue, 08 Jul 2014 02:08:28 GMT
    < Date: Tue, 08 Jul 2014 02:08:28 GMT
    < Cache-Control: private, max-age=0, must-revalidate, no-transform
    < Vary: Accept, X-GData-Authorization, GData-Version
    < GData-Version: 1.0
    < Last-Modified: Sun, 06 Jul 2014 21:14:33 GMT
    < Transfer-Encoding: chunked
    < Set-Cookie: ...
    < P3P: ...
    < X-Content-Type-Options: nosniff
    < X-Frame-Options: SAMEORIGIN
    < X-XSS-Protection: 1; mode=block
    < Server: GSE
    < Alternate-Protocol: 443:quic

I found a
[report](http://stackoverflow.com/questions/19887737/pushing-data-to-google-spreadsheet-through-javascript-running-in-browser)
of the same problem on November 13, 2013.  This
[report](http://stackoverflow.com/questions/20169829/cors-preflight-fails-when-writing-to-google-spreadsheet-api)
on November 24, 2013 and this
[one](https://groups.google.com/forum/#!searchin/google-spreadsheets-api/cors$20preflight/google-spreadsheets-api/f8LLOa5JpiU/vuyreJ5BeGUJ)
on January 9, 2014
said that the `OPTIONS` preflight of a `POST` request received incorrect CORS header in response.

CORS is ratified by W3C on January 16, 2014;
it is reasonable that Google has not fully implemented this standard into their services,
or viewed supporting CORS as a low priority task
(but weird enough, Google has fully implemented CORS in their Google Chrome browser
even before CORS was ratified).

Another explanation is that it is a Google policy that it will not support CORS for certain API calls.
I find this explanation less likely
because which API call supports CORS is not stated in their
[API documentation](https://developers.google.com/google-apps/spreadsheets/),
and the way it fails looks arbitrary to me.

The bottom line is that Google Spreadsheet API is not usable as of today.
