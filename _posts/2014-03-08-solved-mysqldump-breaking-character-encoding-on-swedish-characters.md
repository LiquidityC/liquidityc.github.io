---
layout: post
title: "[SOLVED] mysqldump breaking character encoding on Swedish characters..."
date: 2014-03-08 11:42:09.000000000 +01:00
comments: true
author: Linus Probert
category: mysql
type: post
---
<p>So... I was up for 4 hours too long last night fiddling with the stupid backups of a webshop that I wrote, launched and maintain for my wifes shop (http://www.houseofme.se/webshop).</p>
<p>As I was getting a fresh backup for some local development I noticed that all the å, ä and ö characters where distorted in my copy of the database but they weren't in the production environment. I fiddled with it alot and tore my hair for hours resulting in a very late night with no solution.</p>
<p>The next day I solved the whole thing with some hands on editing in less then an hour.</p>
<p><strong>The cause:</strong> Production environment had latin1 character encoding for all the tables and columns in my database but was running utf8 in the console environment. This caused a problem that I have yet to understand. In short, the dump file was utf8 but was being read as latin1 even though the dump was actually utf8.</p>
<p><strong>The solution:</strong></p>
<ul>
<li> Dump the database with:

	{% highlight bash %}
	mysqldump -u <user> -p <databasename> --default-character-set=latin1 -r <dumpfile.sql>
	{% endhighlight %}

</li>
<li>Edit the dump file replacing the occurence <em>"SET NAMES latin1"</em> with <em>"SET NAMES utf8"</em></li>
<li>Restore the dump to database with:

	{% highlight bash %}
	mysql -u <user> -p <databasename> < <dumpfile.sql>
	{% endhighlight %}

</li>
<li style="text-align:left;">Voila! The å, ä and ö were looking just fine in my local copy of the db</li>
</ul>
<p>I hope this helps someone as much as it relieved me when I found the solution. Because I was really pissed and really nervous when I relaized the issue since it might have meant that all the backups of the production environment were more or less useless.</p>
<p>I'm not a big writer so I'm going to end here. Good luck to anyone else who has this problem.</p>
<p>//Liq</p>
