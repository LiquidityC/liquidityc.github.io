---
title:  "Terminals and telnet"
layout: post
author: Linus Probert
date:   2020-06-14 18:05
tags: telnet mud mud-client client vt102
categories: mud
comments: true
---

So... A pandemic entered society and we all got told to stay indoors and keep
distance from one another. "No problem" said all the Swedes. "No problem!" said
all the programmers. "Living the dream!!!" shouted the Swedish programmers.

Just before this all started I had been spending a lot of time practicing
[rust](https://rust-lang.org), I was just a day or two away from firing up my
*mandatory slightly larger practice project*. Now if you've never tried rust I
suggest you give it a whirl. It has a great online book which takes you through
everything you need to get started in just a short few steps and once you're
through that you realize how intelligently it is made. Especially if you come
from a C/C++ background.

But I digress. Rust also comes with a really nice package and build manager
named *cargo*. It's really handy and it just works like you want it to work. So
I was browsing through different
[crates](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html)
looking for inspiration when I stumbled upon a really nice looking
[tui](https://en.wikipedia.org/wiki/Text-based_user_interface) library.

Now, ever since I spent all my spare time playing
[muds](https://en.wikipedia.org/wiki/MUD) back in my university days I really
appreciated a client known as [Tinyfugue](http://tinyfugue.sourceforge.net/),
even back then it was somewhat unmaintained. But I added to my todo-list that
one day I was going to make something similar myself. Call it a passion
project.

In the past I have made attempts at building something basic like that to set
the foundation for something that I could then expand upon. My first try I got
caught up in terminal escape sequence problems. In particular *scrolling
region*. That time I lost interest and other side projects took up my time.
The second attempt was a few years back, this time I got the TUI working nicely
but the handling of the socket connection gave me problems. In particular being
able to read persistently from the socket as well as being able to send as soon
as the user provided some input. I realized that some sort of threading was in
order. Couldn't be bothered to read up on it and then lost interest.

This third time I had all the building blocks I needed to get it to work. A nice
TUI library, a rock solid, memory safe language with excellent threading
capabilities and an entire pandemic worth of time on my hands (ignoring the
        fact that I have three kids below the age of 8 and a wife that is).
This time it was bound to get done!

I found two libraries that covered my base needs
[termion](https://github.com/redox-os/termion) and
[libtelnet-rs](https://github.com/envis10n/libtelnet-rs). Now, since many rust
libraries are rather new one library didn't cover all my needs but my elite
understanding of vt102 terminals from previous attempts at this project allowed
me to work around and extend the behavior I needed with little hassle. The
other library I found some bugs in. Which I dutifully debugged, located,
      patched and submitted pull requests for like a good little OSS dev.

After two weeks I had a fully functional and usable mud client. After some
boasting on [/r/muds](https://www.reddit.com/r/muds) I came to the
understanding that my
[MVP](https://en.wikipedia.org/wiki/Minimum_viable_product) was indeed *MV*.
But thanks to some dedicated testers and contributors we extended and adapted
the software into version 0.2.

So I hereby present ***[Blightmud](/blightmud/)***. A terminal mud-client that
covers all my needs as a mud player who doesn't play much with bells and
whistles in my opinion. Feel free to try it out, check it out, scrutinize and
improve upon if you have the time, dedication and interest. Compared to
alternatives like [tintin++](https://tintin.mudhalla.net) or
[mudlet](https://www.mudlet.org) it might be a meager contender. Perhaps we'll
catch up one day, who knows.

That's about it.

// Liq
