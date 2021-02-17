---
layout:	post
title:	"Deploying docker"
date:	2016-02-06
categories: [Developer]
---

  ![](/img/1*iZmlaEIAEtXIzM75chqMlg.jpeg)“Thou shall not pass” “docker exec, lets install wireshark”This is my account of some experiences deploying docker (or should I say messing/faffing with docker). The path has not yet finished, but I wanted to share our experience in case it can help others.

#### Linking Docker containers

Yeah, there are many other different technologies for deployment. But containers were “hot” and “the future”. Don’t get me wrong, I do think [containers are the new processes](https://medium.com/@HasuraHQ/for-app-developers-an-introduction-to-kubernetes-and-containers-9e104336356d).

Almost from day one we used docker to deploy our software both in development and “production” (more as in user testing) environments. Initially it was quite ad-hoc: two web applications, a database and a search index. The whole process was deployed using ansible. Containers were linked together.

It was painful at first. Particularly for those of us using docker in OSX. But [the pain was for a greater good](http://12factor.net/dev-prod-parity).

At one point we had several servers, but each with its own copy of the stack. And a load balancer. That worked because our stack was largely immutable. Yes including the data. As part of the deployment process we would recreate the database and search index.

#### Enter CoreOS

We then introduced authentication (don’t ask) And then, our infrastructure couldn’t be managed with a cluster of disposable independent stacks of containers. We needed to somehow manage a cluster. We did a quick research, and the consensus at that moment was [CoreOS](https://coreos.com/).

So we started introducing CoreOS using [fleet](https://coreos.com/fleet/docs/latest/launching-containers-fleet.html). We created feature services, we created presence services, we created discovery services, we used etcd, fleetctl, skydns, haproxy…

Believe me if I tell you this is painful.

Very painful.

The cluster was able to work as a cluster of containers. But the system was hacky: we changed the ports in the web applications to avoid collisions, we removed dependencies because we couldn’t bootstrap the cluster otherwise, we had some front-end stuff that couldn’t live in the same machine even if there were no other constraints because of ports, the scripts were super coupled, upgrading/deploying was not for the faint of heart…

#### Turning point

One day we discovered one of the nodes wasn’t working properly. We end up killing the node, and the autoscaling group (AWS) worked flawlessly creating a new node. The new node got added to the cluster, and fleet started adding containers to the new node. Everything worked fine.

But upon investigating the old node, we started to get a bit upset with CoreOS. On the logs there were some [rkt](https://github.com/coreos/rkt/) traces despite we were using docker exclusively. There were signs of a booting process without a record of it in the logs. There were also traces of auto-update connections despite [we had disabled auto-update](https://coreos.com/os/docs/latest/update-strategies.html). We had disabled auto-update because each update would render our client tools obsolete. We had done what the official docs say should be done to disable updates, but it hadn’t worked (probably a *Restart=always* in the service unit has something to say)

It was a turning point, and we stop trusting our system.

Everything was working. But suddenly everything looked too brittle. It was brittle before. It wasn’t really CoreOS/fleet fault.

#### Kubernetes???

I had discovered [this presentation](https://www.oreilly.com/events/velocity/devops-web-performance-2015/sessions/41818/new-ways-to-deploy-and-manage-applications-at-scale). If you’ve read this far, you’re interested in this topic. Do watch it. And I really mean it.

I knew there was some other way. So I started doing some research. And the number of available tools was overwhelming.

* [Kubernetes](http://kubernetes.io/)
* Hashicorp tools (Consul, serf, [nomad](https://www.nomadproject.io/intro/vs/index.html),vault, terraform)
* [Mesos/Marathon](https://mesosphere.github.io/marathon/)
* [Docker swarm](https://docs.docker.com/swarm/)
* [Amazon ECS](https://aws.amazon.com/ecs/details/)
This is the point were past and present start mixing one another.

We know fleet and etcdctl are very low level. And I was initially sold about Kubernetes. So I started focusing on Kubernetes. And what I’ve discovered is that

* It’s the tool that has all the community praise and all the push. It’s activity is brutal: fixes, contributors, new features. You watch the source code repo and the pipeline is absolutely automatized and frictionless. It gives you confidence that it’s Google backed, but it also has a huge and growing community behind.
* Has some very interesting concepts (pod, replication controller, service, namespace) and a declarative approach (infrastructure is code)
* Lacks some features I thought I was going to need. Like prerrequesites between containers. That was disappointing, but when something obvious is missing, you start thinking you should do things in the tool way not change the tool. In this case, pods are useful for that. But where do I define my environment configuration?
* And last but not least, I find it surprisingly *immature* (heretic !!!!). The default installation of a cluster has only one master node. Which means that if the master goes down, you’ve got a problem. Yes, I know that master down is not the same as cluster down (the cluster can keep working). And, yes, I know [you can configure it in high availability](http://kubernetes.io/v1.1/docs/admin/high-availability.html). But FFS, we’re talking about a tool that is supposed to be production ready. Same about secrets: compared to Vault from Hashicorp, secrets is a joke. Same with number of nodes.
After the research on Kubernetes, I was tempted to move to Mesos+Marathon, much more mature, quite used, but depending on a specific company. Then I discovered hashicorp tools, again, super mature but depending on a specific company. And I found both of them a bit too cumbersome to set up a quick cluster.

These decisions are difficult. Should I choose mature but risky or super active but not quite there?

#### Building cathedrals

Sometime ago I used heroku to deploy a Ruby on Rails to deploy a [side project](https://www.whendoigo.co.uk/) of my own. The project was stalled but a few weeks ago I planned to revamp it. So I decided to self-host it. But I didn’t want to configure all the infrastructure for hot deployments and so on.

So I decided to give a go to [dokku](http://dokku.viewdocs.io/dokku/).

It was really surprising to have something so painless to setup (3 hours, mostly fighting with my own SSL certificate) and update (git push) And yes, there are things like that [for a cluster](http://deis.io/get-deis/).

Not that we can use it at the office at this point.

But we’ve dedicated too much effort to deployment at a point when we had very little to deploy. Sometimes [we in the startup world build our own heroku](https://medium.com/@gonfva/devops-is-so-2010-f9425d06261f#.4h672thsh).

#### Final thoughts

There are many competing technologies. You need to bet. We haven’t decided yet, but we will probably go Kubernetes because it’s more active and there are some amazing features compared to what we have. We won’t use the default script that launches a cluster, but will pack the tools in master/minion images(vagrant/ami). So basically we will design your own cluster.

But I remember when mongodb was cool and then it stopped being cool.

Will we discover down the line that it wasn’t a good bet?

