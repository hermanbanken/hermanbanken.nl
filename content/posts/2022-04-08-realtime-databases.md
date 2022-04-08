---
title: Realtime SQL databases: Noria, Materialize and ReadySet
author: hbanken
type: post
date: 2022-04-08T11:00:00+02:00
url: /2022/04/08/realtime-databases/
categories:
  - databases

---

Sometimes the simplest of ideas are the most powerful. Almost all developers use databases now and then,
often Sqlite, PostgresSQL or MariaDB. These are the boring databases that power most applications.
While many people know SQL, less know about the internal systems inside these databases that are responsible
for quickly scanning, joining or otherwise reducing the stored data into the format that is queried.
These complex internals are what makes these databases do what we want them to do quickly, otherwise we
could just use plain files on disk. However, every database scales only so far. You can add more cpu & memory,
but at some point you hit the ceiling and need to migrate/scale further. Because databases store only the data
on disk/memory and not results each query is computed anew, the load scales almost linearly with amount of queries.

If your application has a read bottleneck (e.g. does a lot more reads than writes) then you might do one of two things:

- add read replicas
- cache results

Read replicas are fairly trivial to add (copy paste infrastructure) but are expensive to operate. Caching is complex
because it requires you to think about when to invalidate those caches and often requires a lot of custom code.
It would be great if we could optimize for those high read volumes without doing either.

Enter [Noria](https://github.com/mit-pdos/noria) and [Materialize](https://github.com/MaterializeInc/materialize), 
which are both [acquainted](https://news.ycombinator.com/item?id=22362301)
and related to the just announched [ReadySet](https://news.ycombinator.com/item?id=30922082).

[![Noria internally](/images/2022/noria.png)](https://jon.tsp.io/papers/osdi18-noria.pdf)

Noria stores not only the data, but also stores predeclared views that you will be requesting. It reverses those
complex internals of standard SQL databases, it flips them around to a dataflow model.

Materialize does something similar to [CQRS](https://microservices.io/patterns/data/cqrs.html) views, as it acts on
a pubsub source of change data and maintains views in-memory. While Noria is eventually consistent, Materialize is serializable.

All three are very interesting projects I would love to try out soon.
