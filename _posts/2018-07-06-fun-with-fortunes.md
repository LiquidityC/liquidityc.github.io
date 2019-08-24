---
title:  "Fun with cows and fortunes"
layout: post
date:   2018-07-06 09:45
categories: programming
tags: python fortune cowsay fun
comments: true
---

So, thought I'd write a post...

I had some time over at work the other day and thought I'd make something fun.
I've always enjoyed browsing the
[/r/showerthoughts](https://www.reddit.com/r/Showerthoughts/) subreddit on
reddit. Generally the top voted posts on that page can be quite amusing.

Now, like many linux terminal junkies I've always used the old

```bash
fortune | cowsay
```

when my terminal opens up.

I got the idea that it would be nice to get the top showerthoughts posts merged
into fortune for this purpose.

Let the hacking begin
---------------------

First I needed to get the top posts. Python has of late become my goto language
when you need to script up some quick internet parsing stuff. After roughly 30
seconds of googling I learned that reddit provides rss to almost all their
subreddits by just appending ".rss" to the url. This knowledge provided this
url:
[https://www.reddit.com/r/Showerthoughts/top/.rss](https://www.reddit.com/r/Showerthoughts/top/.rss).

And building upon that I got this:
```python
#!/usr/bin/python

import feedparser
import re
import urllib.request
import os.path
import sys

def read_list_from_file(infile):
    list = []

    if not os.path.isfile(infile):
        return list

    with open(infile, "r") as file:
        for line in iter(file.readline, ""):
            line = line.strip()
            if line == "%" or line == "":
                continue
            list.append(line)

    return list

def save_list_to_file(list, outfile):
    with open(outfile, "w") as file:
        file.write("\n%\n".join(list))

if len(sys.argv) < 2:
    print("You need to provide a storage file")
    sys.exit(0)

fname = sys.argv[1]

quotes = read_list_from_file(fname)

rss = feedparser.parse("https://www.reddit.com/r/Showerthoughts/top/.rss")
for post in rss.entries:
    quotes.append(post.title)

quotes = [quote.strip() for quote in sorted(set(quotes))]

save_list_to_file(quotes, fname)
```

I won't go into detail, but essentially this is the gist of it:
- Open a file and read possible existing "showerthoughts"
- Get new "showerthoughts" from reddit rss
- Merge them together
- Remove possible duplicates
- Write it all back to file in "fortune format"

Googling some more gave me the following command to create .dat files from these
text files:
```bash
strfile -c % /usr/share/fortune/showerthoughts /usr/share/fortune/showerthoughts.dat
```

So, now I had a script that concats current top showerthoughts to ones I've
fetched earlier and a command that converts the output of that command to a .dat
file that "fortune" can read.

Placing the output under "/usr/share/fortune" (on my machine) gets me the result
I want.

I'm not going to boor you with details of cron. Just google how it works if
you're new to this. But that's what I did to fetch new showerthoughts into my
fortune db every hour.

// Liq
