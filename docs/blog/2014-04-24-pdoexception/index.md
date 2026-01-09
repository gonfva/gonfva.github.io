---title: PDOException
date: 2014-04-24T15:49:30Z
categories: [got ya]
tags: [Developer]
---

Small and uninteresting post. But I've been dealing with an issue for hours and maybe somebody can benefit from this in the future.

While trying to index using Sphinx, I was getting the following error

```
[PDOException]
  SQLSTATE[HY000] [2002] No such file or directory
```

The issue was not related to Sphinx, but to a related connection between PHP/Symphony2 and MySQL. Unfortunately the PHP web application and MySQL were working together and that made the issue more difficult to track.

Most of the replies around the Internet involve file sockets and the like.

The real solution is not to connect to "localhost", but to 127.0.0.1.

Yeah. I know localhost and 127.0.0.1 should be the same. Apparently it is a Mac weirdness. Beats me!!!
