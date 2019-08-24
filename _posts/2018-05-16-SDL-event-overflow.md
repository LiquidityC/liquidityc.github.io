---
title:  "SDL event overflow"
layout: post
date:   2018-05-16 19:19
categories: SDL SDL2 gamedev c c++
tags: SDL SDL2 gamedev c c++
comments: true
---

Sooo, I was made aware of an exploit today that was new to me.

Consider the following:
{% highlight c %}
static SDL_Event event;
bool quit = false;

while (SDL_PollEvent(&event) != 0) {
    if (event.type == SDL_QUIT) {
        quit = true;
        continue;
    }

    if (handle_main_events(&event))
        continue;

    if (gGameState == PLAYING) {
        if (currentTurn == PLAYER)
            gPlayer->handle_event(gPlayer,
                                  gRoomMatrix,
                                  &event);
        camera_follow_position(gCamera, &gPlayer->sprite->pos);
        map_set_current_room(gMap, &gPlayer->sprite->pos);
        roommatrix_handle_event(gRoomMatrix, &event);
        skillbar_handle_event(gSkillBar, &event);
    } else if (gGameState == MENU) {
        menu_handle_event(mainMenu, &event);
    } else if (gGameState == IN_GAME_MENU) {
        menu_handle_event(inGameMenu, &event);
    }
    pointer_handle_event(gPointer, &event);
}

return quit;
{% endhighlight %}

If you're comfortable with SDL you'll recognize this as a basic event polling
loop in any given SDL tutorial/game/whatever. This is how I've always handled
input in all of my games.

Well, as mentioned in previous posts I'm back to making a game on a surprisingly
regular schedule for the last 6 months. It's reached a far enough state that I
let a colleague at work try the game out since he's a big fan of roguelikes.

He was playing and I was peeking over his shoulder and micromanaging to my
hearts content. Now naturally as I think is the case for all game devs, my
colleague was not playing as I normally do and how I've imagined players of my
game would play it. Instead of furiously typing in input as a man on a mission
to write a really crappy essay using just the letters *h, j, k and l* he would
instead hold a directional key down to cover distance faster. Now, I've done
this too. I do it all the time when testing and it's never been a problem. But
in this particular case the rendering would lock and skip erratically, sounds
however played out as normal. Disregarding the limitation of the SDL_mixer
libraries 2 effects channels.

At first I thought with was unfortunate and pinned it on his computer being
*wonky*. However, combining this with attacking enemies gave his character in
the game massive XP boosts and the log would sprout out information suggesting
he just killed one enemy 50 times over.

It took me about 3 minutes to figure out what was going on. Going back to my
code above, check out this line in particular.

{% highlight c %}
    // ...
    if (gGameState == PLAYING) {
        if (currentTurn == PLAYER)
            gPlayer->handle_event(gPlayer,
                                  gRoomMatrix,
                                  &event);
        camera_follow_position(gCamera, &gPlayer->sprite->pos);
        map_set_current_room(gMap, &gPlayer->sprite->pos);
        roommatrix_handle_event(gRoomMatrix, &event);
        skillbar_handle_event(gSkillBar, &event);
    } // ...
{% endhighlight %}

From what I had seen I figured that he was able to spam events so fast that the
poll loop was never exiting, allowing the above code to fire multiple times even
if the players turn was actually over. Tracing the code down supported this
theory where XP would be added to the players stats if the monster that was just
hit had *health <= 0*.

I was unable to replicate this on my windows computer when I got home, even if I
did max out the keyboards repeat speed in the OS settings. But I still feel
confident that this is what was happening.

This is how I solved the problem. In particular, the following steps:

- Check so that the players turn hasn't already ended before handling events.
- Check if input seems to be *spammy*, in that case clean up, break and move on.

The full block of code with the fix:
{% highlight c %}
static SDL_Event event;
bool quit = false;
int handleCount = 0;

while (SDL_PollEvent(&event) != 0) {
    if (event.type == SDL_QUIT) {
        quit = true;
        continue;
    }

    if (handle_main_events(&event))
        continue;

    if (gGameState == PLAYING) {
        if (currentTurn == PLAYER && !player_turn_over(gPlayer))
            gPlayer->handle_event(gPlayer,
                                  gRoomMatrix,
                                  &event);
        camera_follow_position(gCamera, &gPlayer->sprite->pos);
        map_set_current_room(gMap, &gPlayer->sprite->pos);
        roommatrix_handle_event(gRoomMatrix, &event);
        skillbar_handle_event(gSkillBar, &event);
    } else if (gGameState == MENU) {
        menu_handle_event(mainMenu, &event);
    } else if (gGameState == IN_GAME_MENU) {
        menu_handle_event(inGameMenu, &event);
    }
    pointer_handle_event(gPointer, &event);

    handleCount++;
    if (handleCount >= 5) {
        debug("Flushing event queue");
        SDL_PumpEvents();
        SDL_FlushEvents(SDL_FIRSTEVENT, SDL_LASTEVENT);
        break;
    }
}

return quit;
{% endhighlight %}

Now, I just implemented this fix so I have no solid test data on it right now.
But from the tests I was able to perform it seemed to prevent the issue.

If you have similar problems I hope this helps. If you haven't then perhaps it
should be something you fix before anyone figures it out. In larger games I
figure it could have an even bigger impact.

// Liq
