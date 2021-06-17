Task 1 - answer
======

I noticed one problem that might be causing weird behavior from Varnish. 
Expire header is holding the date very far back in the past: Thu, 01 Jan 1970 00:00:00 GMT. In addition the Cache-Control is set to no-cache, no-store, must-revalidate.

This causes the data object to expire immediately which results in them not being cached which leads to origin server being constantly pulled for data which renders the Varnish solution obsolete.

There are three possible options that might solve this problem. 

One is to fix the Expires header to show date in the future, that would be suitable for the customer.

Second possibility is to delete Expires header and set the s-maxage in Cache-Control header for a suitable amount of time in seconds (3600 for an hour, 864000 for a day etc.)

Third possibility is to use extended grace period using below command:
vcl 4.1;
import std;
sub vcl_recv {
 if (std.healthy(req.backend_hint)) {
 set req.ttl = 2h;
 set req.grace = 2h;
 }
}

This will serve object for 2 hours even if the backend is healthy.



PS. Just as a fun sidenote. I was able to download, unpack and open the varnishlog.raw file on Linux on VM using varnishlog -r varnishlog.raw. Unfortunately opening varnishlog.raw on my VM was not very helpful, because Linux installed on VM does not support scrolling up or down the terminal in any way, so I could only work with last 14 lines of the log in there, which would not give my any information. I managed to open the varnishlog.raw file using Notepad++ on Windows, but the text was extremely scrambled. Example below:
![varnishlog](https://user-images.githubusercontent.com/86019690/122354873-b58ab880-cf51-11eb-9001-ebfcb1d42ae7.png)

I don't try to make any excuses if the above troubleshooting and solution are not correct. I just wanted to share some of my adventures I encountered while trying to analyze the problem.

Task 2 - answer
======

Answer in Pull Request under https://github.com/aondio/support in file Task 2 - Brotli support


Task 3 - answer
======

![CDN architecture](https://user-images.githubusercontent.com/6757531/121661483-86cd9780-caa4-11eb-8081-d6ebc6da2800.png)

GeoDNS or traffic router - technology splitting the load between many different PoPs (or Super PoPs). In case of GeoDNS - it's using Geotagging/Geotargetting to split devices into categories and redirect the traffic to the closest PoP/Super PoP. It's not only ensures the best latnecy when handling web requests, but also protects any single PoP/Super PoP from being overloaded as it's also acting as a load balancer redirecting traffic from overloaded servers to the next best PoP.

Clients - recipients of the data - PCs, laptops, servers, mobile devices that are receiving requested (or pushed in case of updates or software installations) data.
PoP - Point of Presence; datacenter strategically placed to deliver content with the best possible latency to as many clients/endpoints as possible. It is consisent of multiple cache servers responsible for content delivery. They are acting as well as a protection for the origin server to ensure it's not being hit by too many data requests and cause overload

Super PoP - bigger version od PoP. It can act as a data supplier for other PoPs, but also as a direct PoP for client if no regular PoP is available. 

Edge node - servers delivering content to the end users as they are the closest end of the network to the client. They contain caches running one or more delivery applications. Content stored here obeys cache expiration rules. Content can be pushed manually to them or pulled into them by requests (depending on the Content Delivery Network configuration). 

Storage node - servers storing copy of the data that is being distributed from the origin server. They are standing behind Edge nodes as commonly they have larger, but slower storage media.

Shield - storage tier servers (or PoP) protecting the origin serves from incoming requests to avoid overload and reduce slowness/latency. Sheild collapses the same requests to avoid hitting the server as well as define chaching rules for the data.

Origin - servers being the main source of the content that is being distributed across the infrastructure by the Content Delivery Network

