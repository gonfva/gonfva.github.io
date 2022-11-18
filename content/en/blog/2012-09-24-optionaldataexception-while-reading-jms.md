---
layout: post
title: "OptionalDataException while reading JMS message"
previous: https://gonfva.blogspot.com/2012/09/optionaldataexception-while-reading-jms.html
date: 2012-09-24T19:33:00
comments: false
categories: [Developer, got ya]
tags:
  - Developer
---

We've been dealing with another bug in JMS code. The symptom was blocked queues and the following exception:

```
javax.jms.JMSException: getObject
          [...]
Caused by: java.io.OptionalDataException
      at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1325)
      at java.io.ObjectInputStream.readObject(ObjectInputStream.java:348)
      at java.util.HashMap.readObject(HashMap.java:1066)
```

In trying to reproduce the issue people in the team introduced a non-serializable object, but they got an exception sending the message `(Caused by: javax.jms.JMSException: setObject [...] java.io.NotSerializableException)`.

In a similar way, trying to send an int, they got a _NotSerializableException_.

And finally trying to send some class and then changing the _serialVersionUID_, they got an error at receiving, but a different error (_java.io.InvalidClassException_).


Everything in Internet pointed to a thread-unsafe error, so we looked at the code and found this


```
message = receiver.receive(timeOut*1000);
[...]
message.acknowledge();
```

with several lines of code between the receive and the acknowledge, and without any synchronization.

So the explanation for our case became clear.

Under very heavy load, it was easy to have a thread execute the receive, a different thread execute the receive and then the first thread acknowledge. But by then both threads have the same object.

And if the first thread changed its copy, the Exception would be triggered.


As a solution, we extracted a method for this code and synchronized the method.
