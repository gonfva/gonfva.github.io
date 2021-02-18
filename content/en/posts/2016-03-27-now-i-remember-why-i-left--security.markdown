---
layout:	post
title:	"Now I remember why I left “security”"
date:	2016-03-27
categories: [Technology]
---

  Around the year 2000, I was doing some consulting work for the ISP branch of a Spanish bank. I was creating dynamic web pages (not many people in the IT industry will remember the word CGI)

On my own, I discovered that creating dynamic we pages was tricky. Dynamic web pages require interacting with the user. But interacting with the user meant risking trusting the user.

#### Some examples

I started taking a look to other dynamic web pages. Many web pages were susceptible to attacks. I remember being able to buy Real Madrid shirts with a 95% discount because the discount was sent as a value from the page. In one of the top online Spanish retailers.

Or being able to leak information of the users in an online forum that involved confidential information.

Or being able to login into a super known Spanish portal using the “recover my password” system, any account name and some specific code.


> ‘; or 1=1 used to be a magic codeFor some time “View page source” was compulsive while navigating, and asking “what if” was a second nature.

#### Notifications and responses

I always, always, always notified what I found to the relevant company. And believe me if I say that many times it’s easier to find a security bug than to find whom to notify.

The range of responses varied a lot.

* Some guys invited me to visit their office and gave me some tokens.
* Some would simply give thanks.
* The well known branch deleted the Real Madrid shirts order (after I notified them) and said nothing.
* Some others would reply and tell me the security breach was nothing because they would check all the orders.
#### Getting scared

However, at one point I got super-scared. The well known Spanish portal initially didn’t do anything.

At that moment there was a different piece of news on the same company. Dot-com bubble had burst and this was affecting stock price.

I had been in contact with good guys in the [security industry](http://web.archive.org/web/20010706055811/http://www.kriptopolis.com/boletin/0183.html). The security problem I had discovered was still open. I was scared of being linked to the piece of news it was already running. The running piece of know was (I was told by some sources) a disgruntled employee with a list of users. This was live data. Including access to payment details and email accounts (without resetting passwords) Only knowing the username.

I kept sending messages to the relevant people in the company. At one point I contacted a lawyer who advised me to go full frontal. So I contacted a journalist. A couple of days before the journalist was going to break the piece of news, they removed the “Recover my password” system, and they deleted my account.


> They said nothing and they deleted my accountYou receive a tip from someone (many messages). (S)he is skilled above average (believe me, not too much). And your only reply is to shut down his/her account. A free account. That can be recreated.

#### That was security in the year 2000

I decided to walk away, and leave “security”. I felt security practices were almost non-existent.

But the reply to disclosures was even worse.

I didn’t want to be part of an industry where bad guys go unnoticed and good guys are … punished.

Being a good guy and having been punished, I must say it worked for me.

I had my daily job. I tried to provide security in my daily job, but I (almost) never would “play” again.

#### All over again

Until a couple of weeks ago.

One of my daughters had been asked to do some online homework. She had accessed with her username and password. But for one specific assignment it kept asking for her password. And her password wouldn’t work. So she asked me for help.

I thought it was a bug in the specific page (others would work). So I looked at the HTML page source, and removed a hidden CSS in a “Start quiz” button. I posted a message in Twitter to the specific company notifying the problem. I promise I really thought it was a bug. She was using a password to login, but that page would ask for a password and her password wouldn’t work.

The following morning I received a reply, and a private message.

The reply said it wasn’t a bug. For some assignments they require an** additional **password.

And the private message detailing a bit more, and asking me to delete my message.


> …We would very much appreciate it if you did not tell people to remove the CSS rule that password protects the start quiz button. Please could you delete this post for the benefit of all schools and learners…I immediately deleted my Twitter post. But told them (via DM) that it wouldn’t prevent independent discovery.

They replied it was a calculated risk (they don’t think teenagers would figure out)

And told me they had notified explicitly my daughter’s school to *remind pupils that this is not a bug*.

Not change the page that asks for password so that is clear it is not asking for the user password.

Not change underlying security so that you cannot bypass it with trivial methods.

Not all schools.

#### But somebody will

Same tactics all over again. You have shitty security . You get told by good guys.

You bully them so that they don’t try again.

Don’t worry. My daughter is fine (her school didn’t do anything) So I won’t try again. I won’t verify if that thing that looks like an [open redirect](https://www.owasp.org/index.php/Open_redirect) in an education site is really an open redirect.

But somebody will.

