---
layout: post
title: "OptionalDataException while reading JMS message"
date: 2012-09-24T19:33:00
comments: false
categories: [Developer, got ya]
---

We've been dealing with another bug in JMS code. The symptom was blocked queues and the following exception:
<br /><br />

<blockquote class="tr_bq"><blockquote class="tr_bq">javax.jms.JMSException: getObject</blockquote><blockquote class="tr_bq">&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [...]&nbsp;</blockquote><blockquote class="tr_bq">Caused by: java.io.OptionalDataException</blockquote><blockquote class="tr_bq">&nbsp; &nbsp; &nbsp; at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1325)</blockquote><blockquote class="tr_bq">&nbsp; &nbsp; &nbsp; at java.io.ObjectInputStream.readObject(ObjectInputStream.java:348)</blockquote><blockquote class="tr_bq">&nbsp; &nbsp; &nbsp; at java.util.HashMap.readObject(HashMap.java:1066)</blockquote></blockquote><br />In trying to reproduce people in the team introduced a non-serializable object, but he got an exception at sending the message (Caused by: javax.jms.JMSException: setObject [...] java.io.NotSerializableException). In a similar way, trying to send an int, he got a NotSerializableException. And trying to send some class and then changing the&nbsp;serialVersionUID, he got an error at receiving, but a different error (java.io.InvalidClassException).
<br /><br />
Everything in Internet pointed to a thread-unsafe error, so we looked at the code and found this
<br /><br />
<br />&nbsp; &nbsp; &nbsp; &nbsp; mensaje = receptorMensaje.receive(timeOut*1000);<br />&nbsp; &nbsp; &nbsp; &nbsp; [...]<br />&nbsp; &nbsp; &nbsp; &nbsp; mensaje.acknowledge();
<br /><br />
<br />with several lines of code between the receive and the acknowledge, and without synchronization
<br /><br />
Under very heavy load, it was easy to have a thread execute the receive, a different thread execute the receive and then the first thread acknowledge. But by then both threads have the same object and if the first thread changed its copy, the Exception would be triggered.
<br /><br />
We extract a method this code and synchronized.
<br /><br />
