---title: Multithreading: Warning lights
date: 2014-01-21T12:17:35Z
categories: [Developer Jobs]
tags: [Developer Jobs]
---

A few days ago I went to an interview which included a pair programming session. The session was a great experience, but the result wasn't quite as good as it should have been, because of some difficulties understanding on what was being asked. It didn't help it was my first experience working with a Mac.


At one point we agreed on creating a new constructor so that we could pass a service in the constructor instead of getting directly in the method under test.


So I had something like (fictitious code)


```java
 class GreatObject {
    public void calculateAndAddAmount(int length,
          boolean cheapService) {
      int cost;
      if (cheapService) {
        cost=length*3;
      } else {
        cost=length*8;
      }

      Service supaService= MegaFactory.getService();

      supaService.addCost(cost);
    }
  }
```

and we had agreed on doing something like

```java
  class GreatObject {
    private Service supaService;
    public GreatObject () {
      this.supaService= MegaFactory.getService();
    }
    public GreatObject (Service supaService) {
      this.supaService=supaService;
    }

    public void calculateAndAddAmount(int length,
          boolean cheapService) {
      int cost;
      if (cheapService) {
        cost=length*3;
      } else {
        cost=length*8;
      }

      supaService.addCost(cost);
    }
  }
```

When I was going to do it, something in the back of my mind started to blink. It was a bit subtle and it took me a while to be able to express it. So as an exercise for the future, I'm going to try and express it in writing.


The problem with the change is that we have transformed a local variable into an instance variable. Local variables are local to the thread. Instance variables are shared between threads. So until now we had a thread safe class, but now the class is not thread safe. As an example: What if each instance of the service can only be invoked once and we have [two threads](https://twitter.com/nedbat/status/194452404794691584) running?


I was asked to tell what to do to be protected. I replied I didn't know. The typical answer involves the reserved word **synchronized**. But as [I've told in the past]({{< ref "2012-03-23-synchronized-silver-bullet">}}), that is not usually a good answer and almost never the best answer.
