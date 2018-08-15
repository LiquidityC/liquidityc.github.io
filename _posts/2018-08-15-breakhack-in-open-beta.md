---
title:  "BreakHack is in open beta"
layout: post
date:   2018-08-15 18:19
author: Linus Probert
categories: breakhack beta game gamedev
tags: breakhack beta game gamedev
---
![]({{ "/assets/bh-icon.png" | absolute_url }}){:style="float: right" width="80px"}
Ok, so my roguelike "BreakHack" has finally reached open beta. If you are
reading this you have probably been directed here from one of my links since
nobody accidentally stumbles onto my blog.

About the game
--------------
As explained in the
[README](https://github.com/LiquidityC/breakhack/releases/tag/beta1).
BreakHack is a roguelike(lite?) based on my minor experience with "NetHack".
It's intended to keep a player occupied for 10-20 minutes when everything else
has become boring. If you die, you restart. No save games, no particular story.
Just enter the dungeon, kill the monsters and leave with the loot.

You will die, ***a lot***. It's not simple at first. But there are tricks to get
further into the depths of the dungeon. The game won't hold your hand, you'll
have to work out the mechanics as you move along.

To give the readers of this a slight advantage:
- You navigate with arrows, wasd or hjkl (because vim/unix!)
- There are skills shown in the bottom bar marked with numbers. Press the
corresponding number to activate
- You get more skills as you gain levels

Beta testers needed!
-------------------
If you wish to be a beta tester this is the link you need:
[releases](https://github.com/liquidityc/breakhack/releases)

If using Windows
-------------
I'd suggest downloading the zip file. You can run the game
straight from the executable inside the archive.

**(Note: Windows will warn about the zip seeming unsafe in some cases
and will almost certainly bring up a smartscreen alert when running
the executable. These are false positive warnings. I don't have a code
signing cert because they are expensive)**

If using Mac/Linux
------------------
You are probably best off compiling the source code. Good Luck! :o)
(In all seriousness it shouldn't be that hard. Just check the [project
 page](https://github.com/liquidityc/breakhack))

**(Note: Don't compile using the DEBUG flag because that is not how the
game is intended to be played)**

After testing
-------------
If you find any issues or have any ideas during your hopefully slightly
enjoyable time with the game you can contact me directly if you know how to, 
otherwise you can submit an issue in the git
[repo](https://github.com/liquidityc/breakhack/issues) (I'd prefer this).

If you have no issues and enjoyed the minor pastime that the game offers.
Please feel free to tell me as much. It will keep me motivated to continue with
the other classes that are the obvious next step for the game.

If you have ideas and would like to add them yourself. Just fork the repo and go
nuts. I can't promise that I'll accept all pull requests though. So perhaps
create an issue before you add 17 new source files to the code base.

Keep in mind
------------
This is a beta. Everything isn't final, especially not the type of monsters that
spawn after dungeon level 5, there is only one boss so far and the game doesn't
end. Depending on how deep people seem to get there should be a win state after
level 15ish.

Thanks //
    Linus

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
