---
title:  "Dealing with SSH bruteforce attacks"
layout: post
author: Linus Probert
date:   2020-08-11 12:05
tags: iptables ssh bruteforce hashlimit
categories: 
comments: true
---

So, I have a new internet setup since I moved. This particular setup comes with
some antivirus/firewall features that I've enabled. Never verified how good
they are. But at least it sends notifications and seems to quarantine machines
when they misbehave (me practicing pentesting).

Anyhow, I was looking through the logs and noticed that there had been a lot of
bruteforce attempts on the only machine I own that has external SSH access. It
doesn't run under the default port and up until now I've been under the
impression that this reduces the bulk of the attempts. However not any more it
would seem.

I got to pondering (read: googling) in what way I could reduce these attacks
and still keep the machine available since it's my only access point from
outside and I sort of need that.

After some "thinking" I stumbled upon iptables *hashlimiting*.
[Here](https://poorlydocumented.com/2017/08/understanding-iptables-hashlimit-module/)
is a good article if you want more details.

What it does in short is to limit the amount of connections to a defined
destination within a given time unit. I never dug deep into the details but
this is what I did. Feel free to steal.

```bash
# firewall.sh

iptables -F

# Two connections per hour
iptables -A INPUT -p tcp -m tcp --dport 22 -m state \
        --state NEW -m hashlimit --hashlimit 1/hour \
        --hashlimit-burst 2 --hashlimit-mode srcip \
        --hashlimit-name SSH --hashlimit-htable-expire 60000 \
        -j ACCEPT

# Within 60 seconds
iptables -A INPUT -p tcp -m tcp --dport 22 \
        --tcp-flags SYN,RST,ACK SYN -j DROP
```

The first rule will limit connections to the server to two connections per
hour. After that limit is reached a connection will be accepted and a 60 second
hashlimit will start counting down. Any recurring connections within 60 seconds
will reset the 60 second counter and drop you to the second rule (which will
drop your connection). Connections occuring after 60 seconds will
accept the connection and start a new countdown.

So, in practice, an automated brute force attack will get one or two password
attempts after that it will be stuck in a hashlimit loop where recurring
connections only reset the counter. Possibly if there is a bruteforcer that can
hold of for 60 seconds between attempts this would bypass this limiting. But
that would also be an incredibly slow bruteforcer.

Hope this can help you!

// Liq
