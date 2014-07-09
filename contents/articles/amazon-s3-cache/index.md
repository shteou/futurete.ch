---
title: Amazon S3, Cloudfront and caching issues
author: Stewart Platt
date: 2013-03-20
template: article.jade
---

I recently hacked together a project prototype using Amazon Web Services.  
I store various files using Amazon S3 and access them via Amazon's Cloudfront service.

<span class="more"></span>

In my case, Cloudfront adds very little to the solution other than leaving me free from worrying about catering to multiple regions in the prototype. This is because in ideal operation of the system, each file should be fetched once and only once, barring exceptional circumstances. All other requests for the content would be expected to return a 304 (Not modified) response.

I currently use an If-Not-Modified header to perform cache control, which for the most part works well. However, every now and then Cloudfront insists on returning a full 200 response, despite the ETag matching exactly.  
This happens maybe once in 100 times on smaller files. Although I haven't been able to test this in more depth, I suspect it happens more frequently with larger files.

    stew@raspberrypi ~ $ grep -c "not modified" Update.log # Number of 304s
    4445
    stew@raspberrypi ~ $ grep -c saved Update.log # Number of 200s
    50
    stew@raspberrypi ~ $ echo 'scale=8; (50/4535)*100' | bc
    1.10253500



The questions are:

1. Why does this happen? I suspect the file is deleted from the edge node cache and this forces it to return upstream to the origin server. Rather than checking the ETag at this point, the edge node blindly returns the full response.
1. Does the [HTTP RFC](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html) prohibit this?

I'm currently stumped on this one, but can't warrant spending much more time investigating it. If you know the answer, give me a shout! I'd be incredibly grateful :)
