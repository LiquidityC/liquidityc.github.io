---
title:  "Switching id type from int to uuid in mysql"
layout: post
author: Linus Probert
date:   2019-08-28 14:21
tags: 
categories: 
comments: true
---

Explain how I went from this:
```mysql
/*
 * @TABLE pre_sale CREATE root
 * @UP
 */
create table pre_sale (
  id integer auto_increment,
  customer_id integer not null,
  first_name varchar(60) not null,
  last_name varchar(60) not null,
  street varchar(60) not null,
  zip varchar(10) not null,
  city varchar(30) not null,
  phone varchar(15) not null,
  primary key (id),
  index i1 (customer_id, id)
);

/*
 * @DOWN
 */
 DROP TABLE pre_sale;
```

to this:
```mysql
alter table pre_sale
    add column uuid varchar(36) default '' not null first;

update pre_sale
    set uuid = UUID();

alter table pre_sale
    drop primary key,
    drop column id,
    change uuid id varchar(36),
    add constraint primary key (id);
```
