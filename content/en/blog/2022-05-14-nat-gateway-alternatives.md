---
date: 2022-05-14T14:00:00
title: NAT Gateway is expensive
subtitle: Heresy and a VPC without NAT GW
tags:
  - Developer
categories: [Developer, ctoclick]
---
## NAT Gateway

### A quick intro in AWS networking

In AWS, it is very common to create your own isolated network, in what is called a VPC (from [virtual private cloud](https://aws.amazon.com/vpc/)). The _official_ (more like general perception) way of doing it, involves having multiple public and private subnets. 

* Public subnets are subnets that have a route to and from the Internet (using what is called [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)). 
* Private subnets don't have access _from_ the Internet, but they can have access _to_ the Internet.

Continuing with that general perception, in the public subnets you may have a Load Balancer or even public web or proxy servers. 

And in the private subnets you may have your backend servers and databases. By placing your backend servers in the private subnets, you are trying to limit the access to those servers and databases for security reasons. And leave the interaction with the Internet only to services running in the public subnets

However, your backend services may still need to access _to_ the Internet. For example to download security updates, or docker images. Or to connect to AWS services. This access would be **outbound** or **egress-only**. Initiated from your servers in the private subnet. 

And there would be a traditional piece in that model: the [NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html). 

NAT gateway is a service, managed by AWS that allows egress-only connectivity to private subnets, doing NAT or Network Address Translation for instances in the private subnets and being a gateway for them. 

If you have a server on the Internet and receive a request coming through the NAT gateway, you will see the IP of the NAT gateway, not the IP of the machine that connected through the NAT gateway.

![An example of the usual architecture](/img/Screenshot_20220508-183557.png "Only two availability zones!!!")

It sits within the public subnets (or at least that's my mental model)

NAT gateway is a relatively simple concept. A Linux machine doing ip forwarding, launched in the public subnet and a default route in the private subnet to that machine, would be all you need. Things start being tricky, when you think you need to do hardening, updates, you need to start considering replication and their IP changing.

Hmm. Yeah. Maybe it is not such a simple service.

### The problems with NAT gateway

Not being a simple service, if it were free, I wouldn't be writing about it. I would just use it.

Heck, I didn't even think about NAT gateway at all. It is something you just have in AWS. Like a VPC, three public subnets and three private subnets. You need to download updates. You need to connect to S3 or DynamoDB. And doing so from your private subnets equals NAT gateway in your public subnets. And the sun rises in the East. It's the model that is taught in many courses, and the model that helped me get my [SAA certification](https://aws.amazon.com/certification/certified-solutions-architect-associate/). 

However, when you dig deeper, you find a very significant issue.

The [cost](https://aws.amazon.com/vpc/pricing/). If you go with the model, you have three private subnets, and three NAT gateways. Just by that simple, no brain, traditional approach, you will be paying more than 108$/month per VPC. For a service you don't even think you are using. Because it is a must.

But wait. It gets worse. Traffic is billed in addition. All traffic. Are you downloading/uploading stuff to S3 from your backend instances? Most likely you are using this service and paying for that traffic. At 5 cents per GB (by comparison, that GB stored in S3 for a month will cost you 2.5 cents)

This was the problem I came to realize just a month ago. My current **side project** will be hosting users' applications in AWS (the details are more exciting and I cannot wait to tell them, but they don't matter for now). 

If I was going to provide a hosting service on top of AWS, I needed to at least cover costs. And having a "by the book" setup, was 125$/month only in this item. 

You may think. "Heck, let's have only one NAT and give service to the three subnets with a single NAT GW". OK. Two points there. If that Availability Zone (AZ) goes down, you have also taken down your instances in the other AZs. Because they can no longer connect to the internet, nor to S3 or DynamoDB. And also, because then your inter-AZ traffic costs are going to grow (traffic inside AWS costs if you are not in the same AZ)

So yes, mainly on cost, that is something I couldn't afford. I knew I couldn't use NAT gateway.

At that point I must confess I felt a bit dirty. NAT gateway and the whole private/public subnet was so ingrained in my mind, that parting with it was really difficult. 

It wasn't until I started thinking about alternatives, that I discovered, that, yes NAT gateway costs are little secret for people much more capable than me. And that parting with the private/public subnet model, is not crazy. For a sample, and an inspiration to this post, [Corey Quinn's post on NAT GW](https://www.lastweekinaws.com/blog/the-aws-managed-nat-gateway-is-unpleasant-and-not-recommended/) is illuminating.

> “We’re putting a petabyte of data through that a month, but you don’t understand: We’ve gotta get that data to and from S3.” I get it; I’m not suggesting you change your data flow! But if you add a (completely free) S3 gateway endpoint to your private subnet, suddenly that petabyte of traffic to and from S3 stops costing you $45,000 a month and becomes absolutely free. The fact that this isn’t set up by default is a rant for another time.

Surprisingly cost is not the only issue. Security is another one. In some companies it is common the "only private subnets" and VPN/Direct Connect to on-premise. Installing a NAT gateway seems obvious when you want to download updates, until you start thinking about how you would limit egress traffic, proxy whitelisted domains and limit the possibilities for exfiltration. 

From a security perspective, yes NAT Gateway provides a decent solution, but it won't cover those with stringent requirements.

Please still consider using NAT gateway. But be well aware of the these two issues.

### The alternatives

Now I'm going to give some alternatives to NAT gateway. 

First things first. If your backend is sending/receiving data to/from AWS managed services like S3 and DynamoDB **create an endpoint for them**. [S3 and DynamoDB gateway endpoints have no cost at all!](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-ddb.html). They are more secure (since they can have policies applied to them and you don't need to go to the Internet to reach them). This should be a must in every default configuration. Courses should be teaching them instead of putting a NAT gateway everywhere.

If you use other AWS managed services you may consider an interface endpoint as well. Are you pulling or pushing images to ECR. Use an [ECR endpoint](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html). Are you using SSM? Use and endpoint. The traffic to those endpoint is billed. But it is billed 5 times cheaper!!!

Second. If you still need egress traffic to the Internet, and you already have IPv6 on your VPC, you may want to consider [egress-only-internet-gateway](https://docs.aws.amazon.com/vpc/latest/userguide/egress-only-internet-gateway.html). Traffic costs applied

Third. You still need updates. But you may consider that having golden images with updates already applied and still no internet access can be a very nice alternative to NAT Gateway.

Fourth. You might consider moving your backend services to a public subnet, were they have Internet access, and clamp down that access with security groups and NACLs. This is _heresy_, and I wouldn't even have thought about suggesting it, have it not Corey Quinn suggested it first

>“We need to move a petabyte a month to and from the internet, and _we can’t move the EC2 instances doing that into a public subnet_ due to Compliance.”

In the end, there is nothing magic about private subnet. There is not a flag or something. Just being able to route things to the Internet.

Fifth. If you only need some gateway to get some updates, but you don't need full redundancy, you may consider creating your own instance. AWS [dedicates a page to it and it will tell you how to do it](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html), so it is not like you are doing anything crazy. You might as well, instead of a plain NAT instance, create a proxy with squid so that you limit the exfiltration options.

### Conclusion

NAT gateway is a reasonable service. But the cost is prohibitive. And a cost that affects both very small companies (with a few instances and a database, and where the **EC2-other** item masks this) but also big ones that rely on AWS services (S3 and DynamoDB)
