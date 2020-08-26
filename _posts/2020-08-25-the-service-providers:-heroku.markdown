---
layout: post
title:  "The Service Providers: Heroku"
date:   2020-08-25 14:08:02 -0700
---
We already know that Qmap is hosted on the same Vancouver, Washington IP as 8kun.  What can we deduce about the rest of their stack?

{% highlight bash %}
debunqr@antiqhq> curl -I https://qmap.pub/

HTTP/2 200
server: nginx
date: Tue, 24 Aug 2020 17:38:43 GMT
content-type: text/html; charset=utf-8
content-length: 285066
vary: Accept-Encoding
set-cookie: heroku-session-affinity=not_sharing_my_session_cookie; Version=1; Expires=Wed, 25-Aug-2020 17:38:43 GMT; Max-Age=86400; Domain=mapqapp.herokuapp.com; Path=/
...
{% endhighlight %}

Look at the domain  **set-cookie** header.  The cookie domain is **mapqapp.herokuapp.com**.

Try it yourself: [mapqapp.herokuapp.com][qmap-direct-link].  Console commands reveal the Heroku router in [Virginia][aws-az]:

{% highlight bash %}
debunqr@antiqhq> dig +short qmappub.herokuapp.com

us-east-1-a.route.herokuapp.com.
{% endhighlight %}

What does this mean?  It means that requests to qmap.pub are processed by San Francisco based [Heroku][heroku-home], a PAAS (Platform As A Service) host.  VanWa merely serves
as an SSL termination endpoint.  This seems like an unnecessary architectural complication, since Heroku is a top-tier platform hosted directly on top of
Amazon Web Services.  Many [Heroku customers][heroku-customers] are large companies with much more traffic than this shitty PsyOp.

Why are they doing this?  This is the difficult question.  PsyOps by default are not intelligent.  How about the people who run them?

8kun and QAnon-related sites obviously [struggle with retaining service providers][columbian-8chan-back-article], so this may be a thinly veiled attempt
to circumvent Heroku's [Acceptable Use Policy][heroku-aup].  I'm just speculating, but here is a list of other potential benefits of this setup:
1. Stability.  If Heroku shuts down this account due to [customer complaints][heroku-contact-abuse], the operator can still use VanWa.
1. Ease of deployment.  Heroku is famous for its simple git-based deployment.  Perhaps the qmap developers do not have strong deployment chops.
1. Logging.  This does not make much sense because Heroku easily integrates with a large number of [logging providers][heroku-addons].  Perhaps it is less
costly to process them on VanWa.  Perhaps the operators are paranoid?
1. Verifying legitimate Q posts.  We already know that "tripcodes" are a weak phenomenon, something that sounds secure to people who do not understand
security.  Because tripcodes can be [easily cracked][tweet-bcrypt-tripcodes], Qmap needs an second verification method, such as a special [request header][wiki-request-header].
With this architecture, VanWa could identify requests with the matching special header.
1. Intrusion detection.  They are playing a dangerous game, so they need early warning of attempts by law enforcement, hackers, and intelligence services
to attack their infrastructure.  Heroku's router does not provide intrusion detection, so VanWa's server could merely log attempts while forwarding the request
to Heroku for processing.

None of these really seem that clever, and I feel stupid for elaborating on them.

The debunking community is already working on [deplatforming][tweet-deplatform-8kun].  Let's add Heroku to the list of former Q service providers.

[qmap-direct-link]: https://mapqapp.herokuapp.com/
[heroku-customers]: https://www.heroku.com/customers/case-studies
[heroku-aup]: https://www.heroku.com/policy/aup#
[columbian-8chan-back-article]: https://www.columbian.com/news/2019/nov/07/8chan-back-online-under-the-name-8kun/
[heroku-contact-abuse]: mailto:heroku-abuse@salesforce.com
[heroku-addons]: https://elements.heroku.com/addons#logging
[tweet-bcrypt-tripcodes]: https://twitter.com/bcrypt/status/1297970470214483968
[wiki-request-header]: https://en.wikipedia.org/wiki/List_of_HTTP_header_fields
[heroku-home]: https://www.heroku.com/home
[tweet-deplatform-8kun]: https://twitter.com/HW_BEAT_THAT/status/1297680313787662342?s=20
[aws-az]: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html
