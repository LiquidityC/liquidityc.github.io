---
title:  "Switching id type from int to uuid in mysql"
layout: post
author: Linus Probert
date:   2019-08-28 14:21
tags: 
categories: 
comments: true
---

I ran into an issue the other day where a database I was using at work for a
micro service wasn't replication safe. The solution took some research so I
thought I'd share it for others to be able to make use of it.

First the setup. Imagine that you're running a microservice in a cluster (eg.
[kubernetes](https://kubernetes.io)). Your service is running in 2
instances with two separate databases. There is replication enabled between the
two databases to make sure that both databases reflect the same reality to your
microservice.

![]({{ "/assets/ms-db-diagram.png" | absolute_url }}){:style="margin-left: auto; margin-right: auto; width: 50%; display: block"}

Initially I set up my database table like this (MySQL):

```sql
create table information (
  id integer auto_increment,
  /* Additional irellevant columns */
  primary key (id)
);
```

A while later I was informed about issues that could appear during replication.
Eg if two services create different entries in their respective database there
is a risk that they auto_increment the same id on those separate entries. When
the replication does happen between the databases this conflicting id will
cause the replication to fail.

After reading up a bit on MySQL I discovered
[UUID()](https://dev.mysql.com/doc/refman/8.0/en/miscellaneous-functions.html#function_uuid).
I then decided that this was a better id to use for my table entries.

This is how I patched my database:
```sql
alter table information
    add column uuid varchar(36) default '' not null first;

update information
    set uuid = UUID();

alter table information
    drop primary key,
    drop column id,
    change uuid id varchar(36),
    add constraint primary key (id);
```

Since UUID() is documented to produce unique identifiers between tables,
databases and servers this should be a safer solution when faced with
replication setups.

If you don't like storing a lot of VARCHAR(36) values in your database then
you should be able to work out a solution similar to the below example.

```sql
create table information (
  id binary(16) not null,
  /* Additional irellevant columns */
  primary key (id)
);

/* On insert or read you can use the following utility functions */
UUID_TO_BIN(uuid)
BIN_TO_UUID(bin)
```

I'll leave it up to you how you'd want to work that out. Hope my solution for
changing after the fact helps someone.

Br,
Liq
