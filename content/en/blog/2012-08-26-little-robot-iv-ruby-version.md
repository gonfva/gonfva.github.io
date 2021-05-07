---
layout: post
title: "Little robot (IV): Ruby version"
previous: https://gonfva.blogspot.com/2012/08/little-robot-iv-ruby-version.html
date: 2012-08-26T20:00:02
comments: false
categories: Developer
tags:
    - Developer
---

Besides the [console faking](https://gonfva.blogspot.com/2012/08/faking-console-in-ruby.html) and the [dependency inversion issue](https://gonfva.blogspot.com/2012/08/little-robot-iii-dependency-inversion.html), my code in Ruby was pretty rough. You can see here [the version I sent](https://github.com/gonfva/assignments/blob/cc8d3664474c934a6e864ad285e5fb3855e49c2a/gfv_robot_ruby/robot.rb).


One obvious problem is the use of string instead of symbols. It was a bad bad mistake. So big if I had been the person testing, I would have thought very bad of myself.


Anyway it was clear that Java version was considerably more verbose.


So one obvious improvement is using symbols instead of string.&nbsp;But during the interview, I asked what other improvements people have done on the assignment. One tip I received is the use of [Array.rotate](http://www.ruby-doc.org/core-1.9.3/Array.html#method-i-rotate). I don't like that improvement, because it makes the code less understandable. But I didn't like the verbosity of the case, and I tested a different approach. This is the code I left in the end:


<code>class Robot
&nbsp;&nbsp;VALID_X = 0..4
&nbsp;&nbsp;VALID_Y = 0..4
&nbsp;&nbsp;VALID_F = [:NORTH,:SOUTH,:EAST,:WEST]
&nbsp;&nbsp;TURN_RIGHT = {:SOUTH=&gt;:WEST,:NORTH=&gt;:EAST,:EAST=&gt;:SOUTH,:WEST=&gt;:NORTH}
&nbsp;&nbsp;TURN_LEFT = {:SOUTH=&gt;:EAST,:NORTH=&gt;:WEST,:EAST=&gt;:NORTH,:WEST=&gt;:SOUTH}
&nbsp;&nbsp;MOVE = {:SOUTH=&gt;[0,-1],:NORTH=&gt;[0,1],:EAST=&gt;[1,0],:WEST=&gt;[-1,0]}


&nbsp;&nbsp;def initialize
&nbsp;&nbsp;&nbsp;&nbsp;@accepting=false
&nbsp;&nbsp;end


&nbsp;&nbsp;def place(cols,rows,facing)
&nbsp;&nbsp;&nbsp;&nbsp;x=cols.to_i
&nbsp;&nbsp;&nbsp;&nbsp;y=rows.to_i
&nbsp;&nbsp;&nbsp;&nbsp;f=facing.to_sym
&nbsp;&nbsp;&nbsp;&nbsp;if is_valid?(x,y,f)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@x=x
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@y=y
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=f
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@accepting=true
&nbsp;&nbsp;&nbsp;&nbsp;end
&nbsp;&nbsp;end


&nbsp;&nbsp;def left
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=TURN_LEFT[@f]
&nbsp;&nbsp;end


&nbsp;&nbsp;def right
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=TURN_RIGHT[@f]
&nbsp;&nbsp;end


&nbsp;&nbsp;def move
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delta=MOVE[@f]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x = @x + delta[0]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;y = @y + delta[1]
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if is_valid?(x,y)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@x=x
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@y=y
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end
&nbsp;&nbsp;end


&nbsp;&nbsp;def report
&nbsp;&nbsp;&nbsp;&nbsp;return nil unless @accepting
&nbsp;&nbsp;&nbsp;&nbsp;"#{@x.to_s},#{@y.to_s},#{@f}"
&nbsp;&nbsp;end


protected
&nbsp;&nbsp;def is_valid?(x,y,f=@f)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VALID_X.include?(x) &amp;&amp; VALID_Y.include?(y) &amp;&amp; VALID_F.include?(f)
&nbsp;&nbsp;end
end
 </code>
