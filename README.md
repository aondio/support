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


Task 3
======

![CDN architecture](https://user-images.githubusercontent.com/6757531/121661483-86cd9780-caa4-11eb-8081-d6ebc6da2800.png)

This is a CDN architecture, please describe the role of each component.

