---
layout: post
title: PayPal's (quick)sandbox
date: 2014-04-27 10:15:52
comments: true
author: Linus Probert
type: post
---
<p>So.... I'm implementing PayPal for a customer application that I've been working on for some time. I found the API pretty straight forward and I got the initial request hacked up pretty quickly.</p>
<p>PayPal offers a neat solution called the "sandbox" (generic term for test environment) that developers can use to verify that their applications are functioning correctly before sending them up on the production environment. This however was where I literally hit a wall.</p>
<p>The first request I tested against this environment had a response time of what seemed like 10 minutes, I didn't clock it because I didn't stay by the computer waiting for the response. "This can't be good" I thought and proceeded to do what one always does in these situations. Google it.</p>
<p>The first hit I got on Google was <a href="http://pleasantonwebdesignblog.com/2008/08/paypal-sandbox-so-slow-its-unusable.html">this </a>page, note that the post is from 2008. This means that PayPal has had an unbelievably slow testing environment going on 4 years now.</p>
<p>I'm mostly writing this post so that other Googlers upon reading it will know that they aren't the only ones with this problem. It's crap but I don't think there is a solution.</p>
