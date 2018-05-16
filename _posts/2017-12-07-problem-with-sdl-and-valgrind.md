---
title:  "Problems with valgrind and SDL"
layout: post
date:   2017-12-07 22:56
categories: sdl2 sdl valgrind c c++
---

So, I'm working on my new
[game](https://github.com/liquidityc/breakhack). I want to be thorough
and make sure I didn't accidentally introduce any nasty leaks or memory
errors. I startup the old trusty valgrind and it spits out over nine
thousand different errors regarding leaks and bad jumps.
I figure, I'm not the god of code, but I'm sure as hell not that bad. Especially
not bad enough to make roughly 1000+ loc cause that many errors.

Now, if you have any experience with SDL 1 or 2 and valgrind. This problem
shouldn't come as a surprise to you. Different graphics drivers and
various other libraries that SDL makes use of sometimes can contain errors
in them that don't actually harm the execution of your game but can still
make valgrind [freak out](https://www.youtube.com/watch?v=EVZh4WcdC3s).

Now up until this point the errors have been so few that I've happily been able
to ignore them and easily visually filter out the problems that are cause by my
code. However my new fancy laptop with intel HiDPI fancy graphics wasn't being
as nice to me. There was a literal wall of crap spewing from valgrind just from
the initSDL call at the beginning of my game.

So this needed some looking into. It took me about 2 minutes of googling to find
a solution to my problem. "Valgrind suppression's", you should google that too if
you're having similar issues or just generally want to clean up your valgrind
output.

I essentially did the following:

{% highlight bash %}
valgrind --leak-check=full --gen-suppressions=all --log-file=somelog.out
./your_program
{% endhighlight %}

This will produce a logfile of the regular valgrind error output but after each
"problem" there will also be a structure similar to json which is a format for
supressing that particular error.

Based on that data you should be able to create something like this:
{% highlight conf %}
{
	<initSDLLeak>
		Memcheck:Leak
		...
		fun:initSDL
		...
}
{
	<initSDLAddr4>
		Memcheck:Addr4
		...
		fun:initSDL
		...
}
{
	<initSDLAddr8>
		Memcheck:Addr8
		...
		fun:initSDL
		...
}
{
	<libSDLAddr4>
		Memcheck:Addr4
		...
		obj:/usr/lib/libSDL2-2.0.so.0.7.0
		...
}
{
	<libSDLAddr8>
		Memcheck:Addr8
		...
		obj:/usr/lib/libSDL2-2.0.so.0.7.0
		...
}
{
	<libSDLAddr1>
		Memcheck:Addr1
		...
		obj:/usr/lib/libSDL2-2.0.so.0.7.0
		...
}
{
	<libSDLLeak>
		Memcheck:Leak
		...
		obj:/usr/lib/libSDL2-2.0.so.0.7.0
		...
}
{% endhighlight %}

Which should significantly reduce the amount of spam from your valgrind tests.

You might even store this data in files somewhere useful and create a
'.valgrindrc' file in your $HOME directory such as this:

{% highlight conf %}
--memcheck:leak-check=full
--show-reachable=yes
--suppressions=/home/linus/.valgrind/sdl2.supp
--suppressions=/home/linus/.valgrind/i965.supp
--suppressions=/home/linus/.valgrind/x11.supp
--suppressions=/home/linus/.valgrind/glx.supp
{% endhighlight %}

And then everything should look nice in the future. :)

Now, my files might not be perfect. Google around if something seems wonky and
perhaps even get back to me with feedback. I didn't become a valgrind expert in
order to make this post. I just stumbled upon it because I was having a problem
and I thought I'd share it with... the internet and the google spiders.

// Liq

<div id="disqus_thread"></div>
<script>
    var disqus_config = function () {
        this.page.url = '{{ site.url }}';
        this.page.identifier = '{{ page.url }}';
    };
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        
        s.src = '//linuxcodehacks.disqus.com/embed.js';
        
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
