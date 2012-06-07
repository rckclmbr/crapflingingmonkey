--- 
layout: post
title: S3 At A Real-world Company
published: true
meta: 
  _edit_last: "2"
tags: 
- aws
- backcountry.com
- Management
- s3
- work
type: post
status: publish
---
Let's face it, most bigger companies nowdays are afraid of trying something new.  That happens with good reason -- most new ideas tend to fall by the wayside, as trends normally do, and companies like to play it as safe as possible.  I see new ideas and frameworks popping up all over the Twittersphere every day, and I wouldn't consider using any of them in a production environment.
<h2>Amazon Web Services Isn't Just a Pie-In-The-Sky</h2>
The reason I bring this up is this -- <a href="http://aws.amazon.com">Amazon Web Services</a> in the business (not startup) world is *still* considered a new, unproven technology.  And with all the marketing hype around clouds, infinitely scalable services, etc, etc, I honestly don't blame them.  It hard to believe a pie-in-the-sky promise.  That's just the point -- AWS is not pie in the sky, and people that think it is need to dig deeper and understand what it is and what it offers.  The fact is that Amazon Web Services has been around since 2002, and has uptime that is most likely better than your data center.  Coincidentally, Amazon also knows this and is trying to eliminate the false perception that IT IS GOOD FOR YOUR COMPANY TO USE IT TOO.  They published <a href="http://aws.typepad.com/aws/2009/12/the-economics-of-aws.html" target="_blank">this article</a>, along with an updated cost calculator and an Excel spreadsheet to compare your datacenter with using AWS.
<h2>Backcountry.com and S3</h2>
<a href="/images/uploads/900x900_screenshot1.png">{% img /images/uploads/900x900_screenshot1-300x150.png 300 150 %}</a>
Ok, so the real reason for this article.  At <a href="http://www.backcountry.com/">Backcountry.com</a>, we try hard to stay as close as we can to the bleeding edge, but going into "the cloud" has always received serious backlash.  That is, until recently.  Earlier this month we took advantage of the cloud for the first time in a production environment: by using S3 for our "Jumbo" product images.

First, let me explain the reasons we decided to use S3.  Our webapp tier, consisting of a few boxes, hosts the Interchange e-commerce framework, and also contains all our static content.  The trouble was, the 900x900 images consumed about 100gb disk space, but each box only had less than 20gb left.  That left us with one of two traditional options: put new hard disks in each webapp, or use our NetApp to host the images from a single location.  Neither seemed ideal, since putting in new hard disks would be pricey and could take some time, and we were already short on NetApp space given the current budget.  I had done some side-work using S3, and mentioned it.   <a href="http://www.crickertech.com">Chris Alef</a> was able to push the decision as a great idea and it was agreed to do it.

Flash forward 1 week, and we were ready to go live.  We were able to convert and upload the 900x900 images to S3 over the weekend, and get the UI in place in no-time flat.  We have Akamai hosting edge cache in front of S3, and we had zero problems since launch last month.  I asked our operations team what they thought the bill for the month would be, and they guessed $4,000.  The actual bill?  Under $50.  Granted, Akamai probably took most of the traffic, but that's still mighty impressive.

There's so much more we can do with AWS, and I hope this is just the start.  I hope to be able to take advantage of other AWS services such as EC2 and SQS in the future, and I think S3 helped build confidence.  AWS is a service that can be relied on for both startups and established internet businesses alike.
