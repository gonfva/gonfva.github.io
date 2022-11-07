---
previous: https://gonfva.medium.com/the-netflix-of-ebooks-937d6cb5c0a9
layout: post
title: "The Netflix of books"
date: 2015-12-12T18:00:00
tags:
  - Technology
categories: [Technology]
---

![](/img/1*PkZMt3V50bwmKwztcefDyg.jpeg)

Broken Kindle anyone?A couple of months ago, we were flying back to London after the summer. My 12 year old twin daughters, usually eager video watchers or passionate console players, were avid readers at that moment. To the point they were sharing one of their two physical Kindle reading devices.

A woman walking along the aisle, stopped next to my wife, started a conversation with her about my daughters’ reading habits, and asked my wife what model of e-book reader we thought is better (so that she could give it to their own children).

I like reading. My wife likes reading. My daughters love reading. In the last 6 months, I’ve ordered to Amazon (I just checked) 30 books, most of them fiction (I also buy technical books). But, despite having kindles, almost two thirds were printed books (19 vs 11).

So when I read this post about how [young people prefer printed books to ebooks](http://blog.bookbaby.com/2015/08/younger-readers-prefer-printed-books/), I asked my daughters if they prefer printed or electronic. They didn’t know. I don’t know either. And I started to think about how the ebook paradigm is still needing disruption.

Benedict Evans, summarized, some time ago, this feeling in a Tweet.

> [](https://twitter.com/BenedictEvans/status/640215664125341696)So I thought everything was said. But nonetheless, even if nobody reads it, lets write it.

### Pricing

#### Compare to physical

I can’t understand how an ebook can be more expensive than the same book in paper.

I can’t.

Yes. You can tell me about how the [publisher and the distributor disagrees](http://www.theguardian.com/books/2014/nov/13/amazon-hachette-end-dispute-ebooks) about what the price should be. Yes, I understand why you can put the same price (or even higher) for the electronic item. Because, yes, you fear that if it’s cheaper, you wouldn’t sell physical books.

I’ve bought many physical books because the cost was the same or higher than the electronic version (or even the electronic version didn’t exist). So yes, that argument has merit.

But market is not inelastic. Not only everybody has access to a humongous collection of pirated books, including the latest bestsellers. Not only public libraries in many western countries offer a good selection of books.

The main issue, is that [people read less books](http://www.theatlantic.com/business/archive/2014/01/the-decline-of-the-american-book-lover/283222/). Attention and dedication needed to read a book in a world of other competing stimuli is non-trivial. And if you read using a tablet, your social network is one notification away. And coming back to the ebook is difficult because you need will-power.

#### Paying without touching

Of the last four technical books I’ve read:

- One of them I paid for them, I loved it, but it was (legally) [freely available](https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html) (and I didn’t know). Had I know it, I wouldn’t have paid for it initially, but I would have liked to pay/contribute “afterwards”.
- Another one, [it was freely available. I loved](http://www.oreilly.com/programming/free/software-architecture-patterns.csp). I would like to pay/contribute back.
- The other two. I paid for them, and I was disappointed. They come from an editor that [refunds if I’m not happy](http://shop.oreilly.com/category/customer-service/oreilly-guarantee.do). But it doesn’t feel right for me.
  Yeah, in a way is what I’m asking for. But I feel I cannot when the thing I buy is fine but it’s just I don’t like it. In a way it’s a barrier Amazon has broken, but not for me.

#### From blog post to ebook

A few weeks ago I started a blog post about AWS (Amazon cloud technologies) Soon it was clear a blog post wasn’t the correct format for what I have in mind: it was going to be too long.

A set of blog posts maybe?

Why not a short ebook?

I’m reading a [set of posts](https://www.nginx.com/blog/introduction-to-microservices/) that could clearly be a short ebook.

The whole “from blog post to ebook” approach is being used for content marketing. And the short fiction ebook is also being used by Kindle publishers.

But apart from Amazon, it seems that either we’ve got free “take this book and give me your data” or “10–40 bucks”

There should be other possibilities.

#### Subscriptions

I’ve considered subscribing to reading programs, even if I don’t feel they’re quite there (probably because disoverability)

[Kindle unlimited](https://www.amazon.co.uk/gp/kindle/ku/sign-up) subscription price is not tempting enough compared to its book collection in the UK and restrictions.

[Safari](https://www.safaribooksonline.com/) has a great technical collection but with a pricing point that cannot justify on a personal level (on the other hand, I think some kind of company subscription should be default in the enterprise).

#### DRM

Many, many books are protected to try and avoid copying. Tends to annoy users and it usually doesn’t deterrent piracy. Yes, people won’t piracy the book you’re selling. They probably won’t read it. And if they read it, somebody will manage to remove the DRM.

#### Other

But apart from pay per book and pay per subscription, for me it’s clear pricing in ebooks is not fully explored.

I can envision something like a better implementation of [Google Contributor](https://www.google.com/contributor/welcome/) but for ebooks. Imagine you could pay some amount and you wanted to give back to writers [depending on different characteristics](http://www.theguardian.com/technology/2015/jul/02/amazon-pay-self-published-authors-per-page-read-kindle).

Anyway, pricing is not the worst thing.

### Experience

There are good things and bad things about the experience of reading ebooks.

#### Volume

Probably the first good one is the sheer number/space needed. Matt Cutts sums up pretty well

> [](https://twitter.com/mattcutts/status/669917008792698880)Past are days when you would go on holidays and you have to decide which two/three books you should pick. Or what book you would buy in the airport because you had to kill a couple of hours but you wouldn’t have anything.

#### Functionalities

In terms of experience, the first kindles with physical buttons on each side, would make it more convenient reading a sequential ebook than an actual book. Newest kindles (and other reading devices like tablets), make it a adequate (but not great) experience.

Being able to change the size of the font (or the lighting/contrast) is great. Both for different people (I read with bigger font than my daughters and smaller than my mum) and different reading conditions.

Search should be a bonus point, but it’s usually purely implemented using a full text index with no tweaking (synonyms, shingles, low/high frequency stop words…)

Being able to interact with the document has pros and cons. I love looking for the definition of something (I’m not native English speaker) I sometimes make notes, but note taking is not quite well implemented. I tend to bookmark, but it’s also not implemented to my liking. When the book has links it’s interesting to get more information, but sometimes takes you away from the book and coming back is difficult (not in terms of a button but in terms of distraction)

Somehow related, in a physical book you’ve got the possibility to change the page while skimming, and start rereading in a previous page. This is not quite solved in ebooks. The usual way is going to the index and changing there. Some reading apps (books in iOS) allow to select the page from a thumbnail in a navigation bar at the bottom of the page. But I find it poor from an usability perspective.

#### Different modes

In fact may devices/reading apps make it difficult to separate the different modes. Reading tends to be page turn. When there is a specific button, for page turning, it’s fine. In other cases, the metaphor tends to be swiping the page. But sometimes I feel I have to change my hands to turn the page. The new kindles are clearly inferior in this sense, because the swipe metaphor applies only for certain areas: it’s not clear if it’s a swipe or a click on an area.

When you go into interaction, I feel many times the app doesn’t do what I expect. Is a click a highlighting, is it a careless click, a look for a word?

#### Full page books

Another space where ebooks are clearly inferior are regarding full page books.

![](/img/1*HuFt4Sf4940fqvvxJL8-3g.png)_xkcd’s “All things explained” book. Example of one page-book. (source [http://xkcd.com](http://xkcd.com/)/)_

So usually books can be read one page at a time. Even one paragraph at time. So changing the font doesn’t affect.

It’s adequately solved when you’ve got pictures or tables.

When you get to full page reading book, the only tactic is either having a big enough device or zoom. Which for an ebook makes it a mess.

And there are books that are thought to read [two pages at a time](http://www.headfirstlabs.com/). It’s a horrible format.

#### Other formats

In a way, this whole page/two page metaphor reminds me the whole desktop/mobile webpages. Sites were built for desktop (books for paper), until there was something that changed the thinking. Today in some cases, mobile is the full experience and desktop is a crippled one (maybe in the future will be the same for ebooks)

What about comics/graphic novels? Can’t we build new formats?

I’m one of those guys fascinated by [Choose your own adventure](https://en.wikipedia.org/wiki/Choose_Your_Own_Adventure) books. Where are they now? In this ebook revolution, these kinds of books were supposed to be primary citizens. But they are missing. And no, it’s not the web, nor is a game. It’s reading. And no format supports it.

#### Specifications

That takes us to a different kind of formats: specification. You’ve got text, you’ve got HTML. You’ve got PDF which is bad for reading (but perfect for page based books) And then you’ve got mobi, basically used by Amazon and ePub, used by everybody else. And an array of converters to help in the process.

### Discovery

[Project Gutenberg](https://www.gutenberg.org/) has over 50,000 free ebooks, many of them classics. [Amazon.com kindle store](http://www.amazon.com/s/ref=lp_133140011_nr_n_3?fst=as%3Aoff&rh=n%3A133140011%2Cn%3A%21133141011%2Cn%3A154606011&bbn=133141011&ie=UTF8&qid=1449921766&rnid=133141011) has over 4 million books with more than 95 thousand new books in the last 30 months.

What to read? How do I discover what books I should read next?

This is probably the main issue. And one that’s not really solved.

Naive approach is search and categorization. You search for a book. Which implies that discovery is off-line (word of mouth basically, but also ads or brick and mortar libraries). Or you look for fiction -> crime & suspense -> crime.

Amazon has the start system, which aggregates scores among people who bought (and presumably read) the book. Amazon also has the “Other Items Do Customers Buy After Viewing This Item”. But it’s amazing that having their own devices (Kindle) and amazing collection of data, they couldn’t make it better. They probably now when a user stops reading. When it reads faster than usual. When they’re hooked into the book. When they start reading, leave, come back.

They’ve got the data. And they do nothing.

To the point they bought a company that tries to solve the discovery problem called [Goodreads](http://www.goodreads.com/).

Yes,[ Rotten Tomatoes](http://www.rottentomatoes.com/) and [IMDb](http://www.imdb.com/) provide value. But imagine if after watching a movie or an episode in Netflix you would need to get to them to vote and get a recommendation about what to watch next.

[Crazy, isn’t it](https://medium.com/@ianpatrickhines/why-google-play-music-won-my-time-attention-and-money-7de9c6511456#.5sysls3cj)?

### In summary

There are a whole array of issues awaiting a new approach to really crack the system. Amazon is superbly positioned (devices, pricing, partial discovery), but apparently they’re not interested, or unwilling/unable to unify. In fact, they’ve gone the wrong way (making their latest devices a dumb tablet instead of improving they original design)

A new traditional incumbent would probably need negotiating power which possibly means capital.

It’s probably time for innovative disruption. Coming from undeserved markets. Not a Netflix for books. Hopefully not a PopcornTime either.
