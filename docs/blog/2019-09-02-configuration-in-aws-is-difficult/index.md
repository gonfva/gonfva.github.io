---title: Configuration in AWS is difficult
date: 2019-09-02T18:00:00Z
subtitle: Least privilege vs complexity explosion
categories: [Developer got ya]
tags: [Developer]
---

I’ve been doing some research on credentials compromise and abuse in AWS. In the process, it was obvious that the main mitigation would be (is) to minimise the permissions assigned to the EC2 instance profile, applying the “least privilege principle”.

![It shows two children on a cobbled road that disappears into the sea. It shows how a road can be difficult to follow.](/img/1*zYkG2Iq7RzTH5CyjYWrPZA.jpeg)_When the path is difficult to follow_

However, sometimes assigning permissions is tricky and it is not easy to understand what a specific permission means.

What follows is just one example I found on the Internet (i.e. not original research)

Imagine you get non-privileged access to an instance that has a role with _ec2:StopInstances_, _ec2:StarInstances_ and _ec2:ModifyInstanceAttribute_ (or much commonly, ec2:\*). With that premise, apparently it is not difficult to make your non-privileged user can escalate into a full “own”.

As I came to think while reading the following tweet … “do I know the implications of 5985 different privileges?”

{{< tweet user="0xdabbad00" id="1161117748119781376" >}}

Do you know what are the implications of those 5985 privileges?

It turns out that I don’t, and below you can find what it could happen if you don’t fully understand either the implications.

{{< tweet user="Darkarnium" id="1065600704134475776" >}}

This is what can happen when you don’t understand implications for one of those 5985 privileges

I haven’t tested the steps detailed in those tweets, but it seems you can take control of a machine just by changing some configuration (*ec2:ModifyInstanceAttribute) *and restarting the instance.

You may need _ec2:ModifyInstanceAttribute_ for a different reason and not be aware of the risk*.*

### Conclusion

Beyond the specific compromise the important part is the tension between “**least privilege principle**” and the **complexity explosion** of understanding and assigning proper permissions.

I used to think that cloud computing, having a saner set of defaults than on-premises, would lead to a more secure setup by default.

But now… well … in the cloud there are ways to make things more secure. But the configuration is more complicated.

Stay tuned for part two.
