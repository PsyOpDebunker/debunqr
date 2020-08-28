---
layout: post
title:  "The Service Providers: VanWa"
date:   2020-08-25 12:08:02 -0700
tags: isp stack
---
After 8chan got booted off of CloudFlare for providing safe harbor for terrorists, we all hoped they would go away forever.  I assumed 
for some reason that they moved their servers to Russia like many other terrorism-adjacent sites (stormer, gab, etc...). Someone posted 
on [twitter][8kun-qmap-same-ip-tweet] yesterday that 8kun and qmap domains resolve to the same IP address.  Really?  What kind of psyop does this?

So I decided to see for myself:
{% highlight bash %}
for fqdn in qmap.pub 8ch.net 8kun.top; do
  echo "Domain ${fqdn} has IP $(dig -t A +short ${fqdn})"
done

Domain qmap.pub has IP 203.28.246.1
Domain 8ch.net has IP 203.28.246.1
Domain 8kun.top has IP 203.28.246.1
{% endhighlight %}

Well isn't that interesting?  This removes any plausible deniability of the connection between Qmap and 8kun.  The domain names resolve to the same IPv4 address.

Where does this IP address live?  Let's take a look at [ARIN][arin-vanwa].

![ARIN lookup for 203.28.246.1](/debunqr/assets/vanwa-arin.jpg)

What are the important bits to glean?
- This IP is part of a /24 CIDR block, which indicates VanWa owns the IP address range 203.28.246.0 - 203.28.246.255.
- The IP address block is owned by [VanwaTech][vanwa-marketing].
- The IP address range ownership was last changed Tue, 31 Mar 2020 23:58:31 GMT.

Their [marketing website][vanwa-marketing] looks like a somewhat legitimate operation.  Many illegitimate operations have professional looking websites too.

What other domains are hosted on this IP?  [BuiltWith][vanwa-builtwith] can answer. Let's take a look:

![Fucking yikes](/debunqr/assets/vanwa-connected-domains.jpg)

The very first connected domain in the list is an explicit reference to the illegal content that 4chan/8chan never seems to distance itself from. 
Again, why in the hell is a Trump-stanning SuperSpy&copy; leaking classified intelligence on a message board for fans of the type of sexual exploitation
that Trump is supposedly at war against?

In addition to this domain there are 24 other domains recently connected to that IP.

In a nutshell, Q posts as well as Qmap posts are served from a server located in Vancouver, Washington, USA.  Qmap posts are further processed by
[Heroku]({% post_url debunqr/2020-08-25-the-service-providers:-heroku %}).

[arin-vanwa]: https://search.arin.net/rdap/?query=203.28.246.1
[vanwa-marketing]: https://vanwatech.com/
[vanwa-builtwith]: https://builtwith.com/relationships/ip-address/203.28.246.1
[8kun-qmap-same-ip-tweet]: https://twitter.com/tobypinder/status/1297629492865896451
