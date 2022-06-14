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


So one obvious improvement is using symbols instead of string. But during the interview, I asked what other improvements people have done on the assignment. One tip I received is the use of [Array.rotate](http://www.ruby-doc.org/core-1.9.3/Array.html#method-i-rotate). I don't like that improvement, because it makes the code less understandable. But I didn't like the verbosity of the case, and I tested a different approach. This is the code I left in the end:

```
class Robot
  VALID_X = 0..4
  VALID_Y = 0..4
  VALID_F = [:NORTH,:SOUTH,:EAST,:WEST]
  TURN_RIGHT = {:SOUTH=>:WEST,:NORTH=>:EAST,:EAST=>:SOUTH,:WEST=>:NORTH}
  TURN_LEFT = {:SOUTH=>:EAST,:NORTH=>:WEST,:EAST=>:NORTH,:WEST=>:SOUTH}
  MOVE = {:SOUTH=>[0,-1],:NORTH=>[0,1],:EAST=>[1,0],:WEST=>[-1,0]}


  def initialize
    @accepting=false
  end


  def place(cols,rows,facing)
    x=cols.to_i
    y=rows.to_i
    f=facing.to_sym
    if is_valid?(x,y,f)
      @x=x
      @y=y
      @f=f
      @accepting=true
    end
  end


  def left
      return unless @accepting
      @f=TURN_LEFT[@f]
  end


  def right
      return unless @accepting
      @f=TURN_RIGHT[@f]
  end


  def move
      return unless @accepting
      delta=MOVE[@f]
      x = @x + delta[0]
      y = @y + delta[1]
      if is_valid?(x,y)
        @x=x
        @y=y
      end
  end


  def report
    return nil unless @accepting
    "#{@x.to_s},#{@y.to_s},#{@f}"
  end


protected
  def is_valid?(x,y,f=@f)
      VALID_X.include?(x) && VALID_Y.include?(y) && VALID_F.include?(f)
  end
end
```
