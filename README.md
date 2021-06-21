# Support
To submit the solutions to the next three tasks, fork this repository to your own github account, push changes to it and send me the link to the github repo which should be reviewed.


Task 1
======

The customer reports a weird Varnish behaviour and s/he can't explain what is going on. To the support case is attached a varnishgather, which can be found here: https://filebin.varnish-software.com/equnbmapw2t23bbt-Tsupport_case

1. Review the gather to understand what is happening with Varnish.
2. If/when you have understood what is wrong with the Varnish installation, describe the problem and a possible workaround/solution.

Hints:
1. varnishgahter script is open source and lives here: https://github.com/varnish/varnishgather
2. Chapter 7 of the book is good starting point to understand what to look at when debugging


While looking over file 187_varnishadm_--_ panic_show from the included gather varnishgather.dad-2021-06-11.tar.gz indicates a Panic at: Fri, 11 Jun 2021 08:48:31 GMT.

Also runing varnishlog -r varnishlog.raw, the time stamp for Expires: Thu, 01 Jan 1970 00:00:00 GMT doesn't seem right.

My assumption would be to check/correct what's causing the incorrect time stamp as well to check the CMOS or system battery.

Reference
https://info.varnish-software.com/blog/how-to-debug-and-fix-varnish


Task 2
======

A customer sends a support e-mail

__Subject

Varnish Brotli Support

__Body

Hi Varnish Support,

I recently read in your changelog that it was now possible for varnish to support brotli. How would I go about enabling this, my backend cannot do brotli compression on its own which is why having varnish do it would be a huge help.

Thanks in advance,
Christopher Taylor
Spa and Pools LLC 


__Resources

None

__Objective

In a Pull Request write a response to the customer. The response for this should include the solution and not be an escalation to tier 3.


Hello Christopher,

Brotli support is enabled via the API below.

import brotli;

sub vcl_init {
  brotli.init(encoding = BOTH);
}

sub vcl_recv {
  if (req.http.accept-encoding ~ "br") {
     set req.http.x-ae = "br";
  } else {
     set req.http.x-ae = "gzip";
  }
}

sub vcl_backend_response {
  set beresp.http.vary = "x-ae";
}

sub vcl_deliver {
  unset resp.http.vary;
}

For further explanation see here: https://docs.varnish-software.com/varnish-cache-plus/vmods/brotli/

Let us know if you have further questions.

Kind Regards,
Joseph
Varnish Support 


Task 3
======

![CDN architecture](https://user-images.githubusercontent.com/6757531/121661483-86cd9780-caa4-11eb-8081-d6ebc6da2800.png)

This is a CDN architecture, please describe the role of each component.


GeoDNS /traffic router - Determines which is the best route from one location to another, Routing traffic based on specific Geo location to a given requesting IP address.

Clients - Termination of client connections on the edge.

PoP - Points of presence, data center sites where they host a number of caching nodes.

Edge node - Stores hot content in memory and routes cache misses to the storage tier via consistent hashing.

Storage node - Responsible for storing most of the content catalog.

Shield - Helps protect your origin servers and offload some of the request.

Origin - Represents the original web servers.

References
https://dzone.com/articles/build-your-own-cdn-in-5-steps
Varnish 6 by Example book, ch 9

