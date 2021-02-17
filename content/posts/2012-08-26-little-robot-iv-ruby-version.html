---
layout: post
title: "Little robot (IV): Ruby version"
date: 2012-08-26 20:00:02 UTC
updated: 2012-08-26 20:00:02 UTC
comments: false
categories: Developer
---

Besides the <a href="http://gonfva.blogspot.com/2012/08/faking-console-in-ruby.html" target="_blank">console faking</a> and the <a href="http://gonfva.blogspot.com/2012/08/little-robot-iii-dependency-inversion.html" target="_blank">dependency inversion issue</a>, my code in Ruby was pretty rough. You can see here <a href="https://github.com/gonfva/assignments/blob/cc8d3664474c934a6e864ad285e5fb3855e49c2a/gfv_robot_ruby/robot.rb" target="_blank">the version I sent</a>.
<br /><br />
One obvious problem is the use of string instead of symbols. It was a bad bad mistake. So big if I had been the person testing, I would have thought very bad of myself.
<br /><br />
Anyway it was clear that Java version was considerably more verbose.
<br /><br />
So one obvious improvement is using symbols instead of string.&nbsp;But during the interview, I asked what other improvements people have done on the assignment. One tip I received is the use of <a href="http://www.ruby-doc.org/core-1.9.3/Array.html#method-i-rotate">Array.rotate</a>. I don't like that improvement, because it makes the code less understandable. But I didn't like the verbosity of the case, and I tested a different approach. This is the code I left in the end:
<br /><br />
<code>class Robot<br />&nbsp;&nbsp;VALID_X = 0..4<br />&nbsp;&nbsp;VALID_Y = 0..4<br />&nbsp;&nbsp;VALID_F = [:NORTH,:SOUTH,:EAST,:WEST]<br />&nbsp;&nbsp;TURN_RIGHT = {:SOUTH=&gt;:WEST,:NORTH=&gt;:EAST,:EAST=&gt;:SOUTH,:WEST=&gt;:NORTH}<br />&nbsp;&nbsp;TURN_LEFT = {:SOUTH=&gt;:EAST,:NORTH=&gt;:WEST,:EAST=&gt;:NORTH,:WEST=&gt;:SOUTH}<br />&nbsp;&nbsp;MOVE = {:SOUTH=&gt;[0,-1],:NORTH=&gt;[0,1],:EAST=&gt;[1,0],:WEST=&gt;[-1,0]}
<br /><br />
&nbsp;&nbsp;def initialize<br />&nbsp;&nbsp;&nbsp;&nbsp;@accepting=false<br />&nbsp;&nbsp;end
<br /><br />
&nbsp;&nbsp;def place(cols,rows,facing)<br />&nbsp;&nbsp;&nbsp;&nbsp;x=cols.to_i<br />&nbsp;&nbsp;&nbsp;&nbsp;y=rows.to_i<br />&nbsp;&nbsp;&nbsp;&nbsp;f=facing.to_sym<br />&nbsp;&nbsp;&nbsp;&nbsp;if is_valid?(x,y,f)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@x=x<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@y=y<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=f<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@accepting=true<br />&nbsp;&nbsp;&nbsp;&nbsp;end<br />&nbsp;&nbsp;end
<br /><br />
&nbsp;&nbsp;def left<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=TURN_LEFT[@f]<br />&nbsp;&nbsp;end
<br /><br />
&nbsp;&nbsp;def right<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@f=TURN_RIGHT[@f] <br />&nbsp;&nbsp;end
<br /><br />
&nbsp;&nbsp;def move<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return unless @accepting<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;delta=MOVE[@f]<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;x = @x + delta[0]<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;y = @y + delta[1]<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if is_valid?(x,y)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@x=x<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@y=y<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end<br />&nbsp;&nbsp;end
<br /><br />
&nbsp;&nbsp;def report<br />&nbsp;&nbsp;&nbsp;&nbsp;return nil unless @accepting<br />&nbsp;&nbsp;&nbsp;&nbsp;"#{@x.to_s},#{@y.to_s},#{@f}"<br />&nbsp;&nbsp;end
<br /><br />
protected<br />&nbsp;&nbsp;def is_valid?(x,y,f=@f)<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VALID_X.include?(x) &amp;&amp; VALID_Y.include?(y) &amp;&amp; VALID_F.include?(f)<br />&nbsp;&nbsp;end <br />end<br /> </code>
