---title: Daily lessons from London trains
date: 2018-07-07T18:00:00Z
subtitle: A bunch of ideas while commuting
categories: [Personal Technology]
tags: [Personal Technology]
toc: true
---

For many people not living in the UK, it will come as a surprise that the railway system in London is quite expensive and provides a really bad service.

![](/img/1*1ejopJRU6tyb99iYCyI14w.jpeg)_Train service is not a theme park. It affects people’s lives_

Some of my Spanish readers will think “surely that cannot be true”. After all, railway was born in the UK. And punctuality is probably one of the words when a Spaniard thinks of British people.

It turns out train delays and cancellations, packed and dirty coaches, and little information are surprisingly common. And tickets are really expensive.

For example, a major shake up of the services last May, ended up as a major screwup including [the resignation of the president of one of the companies](https://www.bbc.co.uk/news/uk-england-44497080).

These problems affect people’s life (even I’ve been told some randoms guys dedicate a a few hours to write articles like this). And there should be solutions for it.

But in the meantime you can also look those issues with a different view and perceive patterns that, maybe with no such an impact, happen in daily life. Here are some ideas (and a little bit of venting on my part)

### Sorry is not enough

**Every single morning** my train stops in the middle of nowhere, because **every single morning** there is a train stopped at the next station (London Bridge).

I think apologising is usually a good first step when there is a problem. But, does it help if every single morning the driver apologises for the same problem? Maybe it is me, but I don’t like it. It may be good that she explain the issue on the PA system, but don’t apologise every morning.

That remembers me of a story a couple years ago. We have a ping-pong table in our office and we used to play doubles from time to time. I remember one specific day. A colleague (hi, George) was a member of the rival team. Many times the ball would go just in the middle of the table, without me or my partner hitting it. After a couple of misses, he said something like: “it’s funny you both apologise for not hitting the ball but then you do the exactly the same in the next point”.

I totally understand it is not the driver fault. It may not even be the franchise fault. It may be a timetable issue. It doesn’t matter.

> Apologising is not enough.**Lesson**: When there is a recurring problem, you need to fix the underlying cause.

### Don’t blame, solve it

UK railway system is quite complicated. Simplifying a lot, trains, drivers and sales are privatised to private franchises, [railway is managed by a government-owned company](https://www.networkrail.co.uk/), [timetables managed by a different government-owned company](http://www.nationalrail.co.uk/) and I haven’t even started.

![](/img/1*JHRUu8mPBrsq0fO6fldY4Q.png)_Franchise shifting the blame_

Being such a mess, it is tempting to look for blame.

People tends to blame the private franchises. I myself has fantasied with launching a [mobile app](#Its%20title%20%27Yet%20another%20Southeastern%20excuse%27).

But in truth, if we want to discuss that question, blame should be much more spread. At least [half of the delays are caused by “Engineering problem”](https://www.economist.com/britain/2016/07/28/going-south) (buy your own “Engineering problem” T-shirt [with a discount for London commuters]({{< ref "#I%27m%20joking%20but%20I%20could%20probably%20get%20rich">}})). Engineering problems are dealt by a government-owned company. And probably that’s part of the problem. In fact, the [new Joint Teams that are being introduced](https://www.gov.uk/government/news/better-journeys-for-south-eastern-rail-passengers) go in that direction.

**Parallels**: I remember a bug [I discovered in IE9]({{< ref "2012-09-22-weird-ie9-bug" >}}). App not mine, but VIP user (and a bit obnoxious). It turns out the user was wrong at blaming the developers, the developers were wrong at blaming the user.

**Lesson 1:** Sometimes in the corporate world, (and to a lesser extent in the startup world), when there’s a problem “It’s not my fault” is not an answer. Let’s first try to solve the problem and then let’s see why it happened and how to avoid happening in the future.

**Lesson 2**: Easy solutions don’t work. Understand the problem first.

### Don’t be too stingy

More than delays and cancellations, packed trains affect my mood.

I totally understand franchises need to maximise profit. And that sometimes, the resources are limited. But to me, trains with a reasonable amount of cars are part of the service (and it should be one of the conditions to a concession). I don’t care about delay as much as simply being able to breathe.

It seems there is one station in my line that don’t accept trains with twelve coaches. It would be fine as I’ve seen in other places, some doors can be left closed in that station. But in the last couple of years I haven’t boarded a 10 coaches train. Despite feeling cattle transport day-in, day-out, trains have more than 8 coaches in my line. And in some occasions I’ve had 6-car trains (that I decided not to board)

{{< tweet user="mtpennycook" id="785894110779572224" >}}

Yes. This is a Member of Parliament complaining on Twitter.

**Parallels**: Ryanair [introducing a new policy and then complaining](https://www.independent.co.uk/travel/news-and-advice/ryanair-luggage-rules-baggage-check-in-bags-carry-on-gate-flights-a8365546.html).

**Lesson**: Some things are beyond your control. But there are others that are under your control even if it’s more expensive.

### Prepare for the unplanned

Cancellations and delays come in different flavours. It doesn’t matter if it is a train fault. Or a crew member that couldn’t attend. Or a line blocked by an engineering problem. Not to mention specific timetables in London because snow, rain, hot weather or leaves in Autumn (I swear I’m not lying, those have been excuses in different occasions. not for a specific problem one evening but to change the timetable for week)

![](/img/1*Im1NuH_8hrl-ABMPMKdG6Q.jpeg)_Delayed because late departure due to missing crew member_

Failures happen. You can let it to affect you. Or you can be resilient to those problems, and anticipate.

You can have spare crew in some key stations in your network, ready to deploy to wherever they are needed.

You can have spare trains in those key stations, ready to provide a solution for those train faults.

A line blocked can be a problem for that line, but it shouldn’t be a problem for the rest of the lines if you’re agile and you can divert trains through other lines.

Regular maintenance of the tracks and especially in critical periods is a must.

The perception is that the system is operating at its edge and a simple issue triggers a catastrophic problem through the net.

**Parallels**: That reminds me in a previous job, with systems running usually on high load. And how a small problem would trigger chaos.

**Lesson**: Failures occur. Be ready for the unplanned.

### The blind leading the blind

Imagine your train is scheduled for 8:37. You arrive at the station at 8:35. You look at the billboard and … you know your commute today is going to be more painful than usual. Even if you see your train expected as 8:42, a “mere 5 minutes delay”, you know that’s probably not true.

Your experience tells you that it will be a bigger delay.

![](/img/1*K0wffkNTba1fFXEdvbrW6Q.png)_Not yet departed. Schrodinger’s train_

It seems the announced delayed is calculated in terms of the current delay for that train. But when a train is delayed in the rush hour, more people will be at the platform waiting for the usually packed train, which will make more difficult for those people to get into the train (already crowded). Which will increase the delay on the following station. And so on and so forth.

I know this.

The companies know it too.

And the train finally arrived at 8:50.

Part of the problem is that the main source of information (even for the companies themselves) is a [system called Darwin](http://www.nationalrail.co.uk/100296.aspx). Darwin is not very good at predicting, neither quite reliable. But [other information systems](https://datafeeds.networkrail.co.uk/ntrod/login) are [not very reliable either](https://groups.google.com/forum/#!searchin/openraildata-talk/td$20issues%7Csort:date). The external feeling is one of a 15 year old system that has gone into firefighting mode.

And then you get to actually punctuality. It should be easy to know what actually happened and get a few metrics. If you look at the [official numbers](http://www.networkrail.co.uk/about/performance/) on punctuality, they don’t look terrible (well except for [that company that in the last month has cancelled or significant delay of 23% of their trains](https://cdn.networkrail.co.uk/wp-content/uploads/2018/06/Sub-operator-PPM-figures-for-Period-03-201819.pdf)). [One in six trains delayed](https://www.networkrail.co.uk/who-we-are/how-we-work/performance/public-performance-measure/), is not good by any measure. But even that it is not my experience. The reason is there aree many: Public Performance Measure (PPM), Right Time(RT) and others. If the train is delayed less than 5 minutes at the final station, then the train is not delayed. If there is a problem in the line and you need to cancel all the trains for the following day, there are “officially” no cancellations.

In summary, my perception is that in addition to stingy franchises, and old physical infrastructure, there is a problem with technology too.

**Parallels**: A few weeks ago I was trying to reserve a flight. I couldn’t fill the details because the page required to fill all the details and there was a bug on one of the details required.

![](/img/0*miUaOt_S3buSrlJb)_I bought the ticket with a different company_

**Lesson**: If you only know that your system has crashed because your customer complains, you have a problem with your system.

### Final words

The railway system in the UK (or at least in the London area) is a mess. There should be a solution for it.

But even if your company is not a monopoly, it doesn’t mean you shouldn’t pay attention to your customers.
