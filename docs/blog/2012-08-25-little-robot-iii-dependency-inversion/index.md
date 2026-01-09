---title: Little robot (III): Dependency inversion
date: 2012-08-25T22:11:00Z
categories: Developer
tags: [Developer]
---

As I wrote some posts ago, [I received an assignment]({{< ref "2012-08-17-little-robot-i">}}), and I decided to do both in Java and in Ruby. When doing in Ruby I wanted to make sure I was able to test the UI, and not only the engine. For doing that, I chose to [fake the console interaction]({{< ref "2012-08-20-faking-console-in-ruby">}}) (both stdin and stdout). I didn't consider dependency inversion.


Sometimes a higher level class A has to invoke methods in a lower level class. Let's say this lower level class tends to be class B. Typical approach (non DI) is to make class A depend on class B. Dependency inversion is to make class B implement some contract associated with class A (the dependency is inverted).


I think I'm still in the process of converting from Java(where DI and interfaces is compulsory) to Ruby. With Ruby is much easier to fake classes B and don't even think about inversion while developing class A.  Maybe I should have done my class more testable. But I didn't realize it until in a different selection process I was asked when to use Dependency Inversion in Ruby.
