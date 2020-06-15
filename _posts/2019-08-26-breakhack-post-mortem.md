---
title:  "BreakHack postmortem"
layout: post
author: Linus Probert
date:   2019-08-26 10:45
tags: gamedev breakhack postmortem
categories: gamedev
comments: true
---

So, I'm done adding features to
[BreakHack](https://store.steampowered.com/app/931040/BreakHack/).  I had a
blast making the game and updating it.  Since money was never the real goal
when releasing the game I was never really concerned with purchases and major
followings of the game. I just wanted to make it.

However, if I'd had plans on earning some money through my game then here is a
list of things that I could have done better. I'll give you the short list and
then explain each point in it's own time.

- [Patience and play testers](#patience)
- [Beta testers](#beta-testers)
- [General installation confirmation](#installation-confirmation)
- [Marketing](#marketing)
- [Final thoughts](#final-thoughts)

## <a name="patience"></a> Patience and play testers
I was pretty excited to get my game up on steam. I was also rather short on
time so I really wanted to get things done and uploaded during my summer
holiday when I had the time to work on those sorts of things.  Since I was a
single dev there was only me to test my game and critique my own game. This is
probably not something you want to do if you're working alone. Unless you are
certain that you are an authority on the type of game you are making you should
really get some experienced players with in your genre to try out your game.

They will tell you what they liked, what they hated and what could have been
better. It's hard to do this on your own since you'll most likely have project
blindness and won't really be thinking outside your own box.

Testers might also stumble upon bugs that you missed because they naturally
test things that you don't. However, finding testers isn't easy if you don't
have a pool of friends or a major following on any type of social media. I
don't have any suggestions on solving this. Perhaps google around a bit or
check what other indie games have done.

Once you do get a "Steam store page" or anything similar up, this will generate
some PR for your game. Perhaps this is a good source for beta testers?

In my case I really wanted to release my game so I pushed it out as it was.
This generated a lot of feedback on my game from people testing and playing it.
This feedback led to a rather quick but large patch to the game which changed a
lot of core features for the better and also added a massive amount of
additional content.

However, the biggest influx you will ever get to your games page is when it's
first posted on the steam store and on the day you make the official release.
If people don't get hooked then it doesn't matter how cool your game is 2
months later, those visitors have already come and gone. Most will never
return. We've seen this in many AAA titles in the past too.

Almost a year after I first released the game I created the page
[breakhack.net](https://breakhack.net). This page should have been done during
my initial release of the game. It's a good way of keeping people interested in
my game. This was an additional point where I should have just calmed down and
released the game when I and some play testers agreed that it was ready.

**TL;DR You're game has to be interesting and fun from day one**

## <a name="beta-testers"></a> Beta testers
This one is rather obvious. But you need beta testers. I had one which was
great but two or three more couldn't have hurt. Players who don't know the code
will try other things that you won't. If you're lucky they will break stuff so
that you can fix those issues before release.

I didn't get many issues in the bug department after release but I feel that I
might have been able to earn some additional feedback through some additional
testers. Different people different ideas so to speak.

## <a name="installation-confirmation"></a> General installation confirmation
This was my biggest error. If you're using steam or any other platform. Make
sure that your stuff works properly on all available platforms. My main mistake
was the linux release. It worked for me when I tested it but apparently I was
using a developer "build" for the linux release. The actual linux release
didn't contain any files to download.

Only one person posted an issue about this which made me aware of the issue and
allowed me to fix it. The other I guess just refunded the game and said
nothing.  And I can't blame them. It's not their job to make sure that my shit
works. I'm just really thankful to the guy who actually put in the extra time
to make me aware of the problem.

Also, if you are releasing a linux version. If you are using a "compile to
binary" language (eg. C/C++). Make sure that you build your release version on
a dist that is the default supported dist on the platform that you are
releasing on. This is easy to do through [vagrant](https://www.vagrantup.com)
or [virtualbox](https://www.virtualbox.org) if you're not running it as your
dev environment.

## <a name="marketing"></a> Marketing
I don't have much ideas here really since it was never a priority for me. I did
notice however that upon setting up my steam store page I did get a lot of
traffic to it.  If you are committed to your project I don't think it's a bad
idea setting this page up quite a lot earlier then you actually plan to release
the game. Once you do you need to be active on the page, updating at regular
intervals and posting the work you're doing to ensure people that progress is
being made and that there is stuff to look forward to. However I think it's a
fine balance. Don't set it up too early either. I think the waiting time to
release has to be somewhat reasonable to keep people from gaining an interest
and coming back for more information. There are people who are better at this
then I am. Just google a bit.

## <a name="final-thoughts"></a> Final thoughts
In my case I did earn my initial investment into steam back. Eg. I got my $100
back. This was my only goal really. However, I think I might have actually
earned some money if the game was first released in it's current state rather
then incrementally improving it over a years time.

I'm happy to have a game on steam which was always my goal. But I could have
had a slightly more popular game on steam if I hadn't been in such a hurry.

// Liq
