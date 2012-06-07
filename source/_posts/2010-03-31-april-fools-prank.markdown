--- 
layout: post
title: April Fools Prank -- Changing Images With Squid
published: true
meta: 
  _edit_last: "2"
  _wp_old_slug: april-fools-prank-changing-all-product-images-in-offic
tags: 
- fun
- image
- prank
- proxy
- squid
type: post
status: publish
---
Well, that time is upon us... time for April Fools.  I'm not very good at coming up with good pranks, but a friend of mine, Chris Alef, gave me a great idea: swap all the product images on Backcountry.com for pictures of our management, provided at http://www.backcountrycorp.com/corporate/section/0/aboutus_team.html.

Pretty simple, and funny idea, but how do you make it work?  It was a lot easier than I thought.  The first idea was to use a proxy, and route all traffic from the office through the proxy.  That seemed less than ideal -- if the proxy went down, so would everyones web access.  Orders would stop shipping, buyers would stop buying, and I would be in a lot of trouble.  There had to be a less risky way to do it.  Well, our images (all static content I guess) are hosted on content.backcountry.com.  Limited to a single domain, eh?  We have an internal DNS, so if we could get content.backcountry.com to just be a proxy, changing only a few images, we could just update the dns to use the proxy.  Brilliant!

I started up an EC2 instance of Ubuntu, installed squid, and started configuring it.  I followed relatively the same pattern as the <a href="http://www.ex-parrot.com/pete/upside-down-ternet.html">Upside-Down-Ternet</a>, using a redirector.  In all, these are the configs I changed/used in /etc/squid/squid.conf:
<pre lang="conf">redirect_children 20
http_access2 allow all
http_port 80 accel defaultsite=content.backcountry.com vport=80
cache_peer content.backcountry.com parent 80 0 no-query originserver</pre>
where <strong>content.backcountry.com </strong>is the origin server (what to proxy), and /tmp/test.pl is the redirector.  Without the redirector, this would be a completely transparent proxy to static content.  Now, the redirector code:
<pre lang="perl">#!/usr/bin/perl

$|=1;

@images = (
  'http://www.backcountrycorp.com/images/newsletter/2131.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2132.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2133.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2135.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2129.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2134.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2130.jpg',
  'http://www.backcountrycorp.com/images/newsletter/2231.jpg',
);

while (&lt;&gt;) {
  my $tmp = $_;
  if ($tmp =~ /images\/items\/medium/) {
    print $images[int rand(scalar @images )] . "\n";
  }
  else {
    print $tmp;
  }
}</pre>
Restart squid, and done!  Now just test it by updating your /etc/hosts to add the ip of the proxy and content.backcountry.com.  Behold, the result:

<a href="/images/uploads/00000019.png">{% img /images/uploads/00000019-300x197.png 300 197 "Jill looks good, doesn't she?" %}</a>

I ended up not doing it.  I really didn't feel it was worth losing my job by messing up.  If I could have found a better way to do it than updating the DNS, even the internal one, I would have done it.  But it was just too risky at my company.  Oh well, now you all know how to do it :)
