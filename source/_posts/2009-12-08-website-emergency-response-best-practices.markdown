--- 
layout: post
title: Website Emergency Response - Best Practices For Controlling Downtime
published: true
meta: 
  _edit_last: "2"
  _wp_old_slug: website-emergency-response-how-to-control-down-time-best-practices
tags: 
- downtime
- Management
- response management
- website
- work
type: post
status: publish
---
If you work for any website that receives a lot of traffic, you know how aggravating it can be when you get woken up in the middle of the night because the website is down or payments aren't getting processed.  People call this many things -- Seg-1, P5, it doesn't matter -- it's "the shit has hit the fan".  Working at <a href="http://www.backcountry.com/">Backcountry.com</a>, I know I've seen my fair share of experiences.  When you have a group of 5 or more people trying to work on the same problem, chaos can ensue.  People will work on the same problem, stepping on each others toes, change something without letting others know, withhold vital information for the sake of being the "rockstar" who fixes the problem, or various other problems.  The ultimate problem is that the company loses money, and it's an embarrassment to have the downtime.
<div class="mceTemp"><dl class="wp-caption alignright" style="width: 186px;"> <dt class="wp-caption-dt"> </dt> <dl id="attachment_28" class="alignright" style="width: 186px;"> <dt class="wp-caption-dt">{% img /images/uploads/emergency_response1.jpg 176 118 Emergency Response %}</dt> <dd class="wp-caption-dd">Emergency Response</dd> </dl> </dl></div>
The first practice I recommend you, and the one thing I hope you keep from the article, is this:  keep track of what you do.  Track everything.  Track changes you make.  Track decisions you made.  Track data you have gathered, no-matter how irrelevant.  Recently at Backcountry, we were troubleshooting a problem that involved nearly all aspects of our architecture -- high load on databases, high load on webapps, traffic stays the same, 500 errors increased, people were losing sessions, traffic through the load balancer was inconsistent... but we couldn't pinpoint the problem.  Searching through the logs, there were no errors, only timeouts.  No query locks in the database.  We kept record of all that information, and tried to make correlations.  We eventually came to a solution by putting the pieces (or what we tracked) together until it made sense.  Then everything else falls into place.  The end result was that, coincidentally, there were 2 major problems at once -- Varnish was passing through a 500 error which happened to be an RSS feed (i.e. high traffic), and session databases were intermittently not allowing connections (for various reasons).  If we didn't record all the data, we couldn't have made the connections.

The second practice I would recommend is to elect a "Call Leader" when responding to an emergency.  This coordinator has a few roles: communicate with business owners periodically, keep track of what tasks people are working on, and make recommendations, in some some cases dictating, what actions are going to be taken.  A side-effect is that communication patterns within the team become explicit -- techs looking into the problem know they need to communicate to the Call Lead, and the Call Lead needs to work with the techs.   This leave the rest of the team to concentrate on the problem at hand, and only their specific silo.  An example conversation among the team might go like this:
<blockquote><strong>Tech 1</strong>: "I'm seeing some weird stuff in our Apache error logs, something about an error with connecting to session dbs.  I'd like to take a look at it."

<strong>Call Lead</strong>: Ok, go ahead.</blockquote>
That's all you need to communicate effectively.  But you'd be surprised at how many companies and teams don't do this.  Having a Call Lead helps ease this transition.

Let me move on to another subject -- the stages of an emergency.  One thing I've seen a lot of teams do is circle around a problem, jumping from one observation to the next, without ever remediating anything.  I've attempted to layout these stages so you know where you're at in solving the problem so you know where you need to go next to get the problem fixed.  The five stages are: Reaction/Response, Collection/State What You Know, Discovery, Remediation, and Verification.
<h2>Reaction/Response</h2>
<ul>
	<li>Once you hear about the problem, whether it be nagios or the guy sitting next to you, make sure the problem is documented however way to document these things (Bugzilla, Jira, Google Doc, whatever).</li>
	<li>Dial into a phone conference, or join a chatroom, or do whatever you need to communicate with the other team members.</li>
	<li>Validate there is actually a problem.  You could waste expensive, valuable time by assuming the problem is larger than it actually is.</li>
	<li>Communicate outwardly that you are taking care of the problem</li>
	<li>Get ahold of ANYONE that should be there.  Don't be afraid to call the CEO of the company if you need, the fact is that if the problem says more than X amount of time, the company will go under.</li>
	<li>Elect a Call Leader (discussed above)</li>
</ul>
This should take a maximum about 5 minutes.
<h2><span> Collection/State What You Know (SWYK)</span></h2>
<ul>
	<li><span>Have everyone on the call state what they know, documenting thoroughly.  Make sure you state what YOU know.</span></li>
	<li><span>Set a schedule or a plan of attack.
</span></li>
</ul>
<h2><span>Discovery</span></h2>
<ul>
	<li><span>Call leader makes assignments (dependent on what people say, of course)</span></li>
	<li><span>Get a list of options/suggestions from techs working on the issue.</span></li>
	<li><span>Weigh options, and avoid "Analysis Paralysis"
</span></li>
</ul>
<span>Should take 5-30 minutes, sometimes more, sometimes less.
</span>
<h2><span>Remediation</span></h2>
<ul>
	<li><span>Call leader makes decision to do an option, and you execute on it.</span></li>
</ul>
<span>Some notes about this one:</span>

<span>You can easily get yourself into a bind by making changes too rashly, and not thinking about the consequences.  I try to use the following principles:</span>
<ul>
	<li><span>Rollback should always be an option.  Too many people are afraid to remove new functionality because of pride or whatever reason.  After a really, most often it's the best solution to just roll back.</span></li>
	<li><span>When changing live-site behavior immediately, try to do it in a rolling fashion.  Restart servers one at a time when in a clustered environment.  When code changes are necessary, roll them to one server if possible to verify changes fix the problem.
</span></li>
</ul>
<h2><span>Verification</span></h2>
<span>Another often over-looked step in the process.  This is when the business verifies the problem as fixed -- or there is no longer any customer impact.
</span>

There's much more, so much that I could write a book on the subject, but I hope this is enough information to be helpful.  I may dive deeper into the different roles and practices in another post, so keep checking back.  As always, I would love to hear feedback on the subject.
