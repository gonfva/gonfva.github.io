---
layout: post
title: "Faking the console in Ruby "
previous: https://gonfva.blogspot.com/2012/08/faking-console-in-ruby.html
date: 2012-08-20T12:31:38
comments: false
categories: Developer
tags:
    - Developer
---

A few days ago, I was preparing an assignment for a job process both in Java and in Ruby. [While doing in Java, I decided to test the engine](https://gonfva.blogspot.com/2012/08/little-robot-ii-java-version.html). But in Ruby I wanted to test the whole application, including the communication via standard input/output on the console.


And how do you mock standard input/output in Ruby?. Well. It turns out it is quite [easy to do it with Rspec](http://stackoverflow.com/questions/6335282/testing-with-stdin-and-stdout-in-rspec). But I wanted to do it without any additional requirement. How do you mock STDIN and STDOUT in plain Ruby.


It turns out it isn't difficult. Based on [this gist](https://gist.github.com/194554), I prepared my [own solution](https://github.com/gonfva/assignments/blob/master/gfv_robot_ruby/tc_robot_console.rb). First the input faker:
<blockquote class="tr_bq"><pre style="border: 0px; font-family: Consolas, 'Liberation Mono', Courier, monospace; font-size: 12px; line-height: 16px; padding: 0px;"><div class="line" id="LC6" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
<span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">class</span> <span class="nc" style="border: 0px; color: #445588; font-weight: bold; margin: 0px; padding: 0px;">InputFaker</span></div>
<div class="line" id="LC7" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">feedStrings</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">(</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">strings</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">)</span></div>
<div class="line" id="LC8" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@strings</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="n" style="border: 0px; margin: 0px; padding: 0px;">strings</span></div>
<div class="line" id="LC9" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC11" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">gets</span></div>
<div class="line" id="LC12" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="n" style="border: 0px; margin: 0px; padding: 0px;">next_string</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@strings</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">shift</span></div>
<div class="line" id="LC13" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
<span style="color: #333333;">    </span><span class="n" style="border: 0px; margin: 0px; padding: 0px;"><span style="color: #333333;">next_string </span><span style="color: #6aa84f;">#TODO: I guess next_string is superflous</span></span></div>
<div class="line" id="LC14" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC16" style="border: 0px; color: #333333; margin: 0px; padding: 0px 0px 0px 10px;">
<span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
</pre></blockquote>Second the output faker
<blockquote class="tr_bq"><pre style="border: 0px; color: #333333; font-family: Consolas, 'Liberation Mono', Courier, monospace; font-size: 12px; line-height: 16px; padding: 0px;"><div class="line" id="LC18" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
<span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">class</span> <span class="nc" style="border: 0px; color: #445588; font-weight: bold; margin: 0px; padding: 0px;">OutputFaker</span></div>
<div class="line" id="LC19" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">initialize</span></div>
<div class="line" id="LC20" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@result</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=[]</span></div>
<div class="line" id="LC21" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC22" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">write</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">(</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">str</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">)</span></div>
<div class="line" id="LC23" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@result</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;"><<</span> <span class="n" style="border: 0px; margin: 0px; padding: 0px;">str</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">chomp</span></div>
<div class="line" id="LC24" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC25" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">clear</span></div>
<div class="line" id="LC26" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@result</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=[]</span></div>
<div class="line" id="LC27" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC28" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">result</span></div>
<div class="line" id="LC29" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@result</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">join</span> </div>
<div class="line" id="LC30" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC31" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
<span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
</pre></blockquote>Third, setup and teardown methods
<blockquote class="tr_bq"><pre style="border: 0px; color: #333333; font-family: Consolas, 'Liberation Mono', Courier, monospace; font-size: 12px; line-height: 16px; padding: 0px;"><div class="line" id="LC34" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">setup</span></div>
<div class="line" id="LC35" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdin</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="no" style="border: 0px; color: teal; margin: 0px; padding: 0px;">InputFaker</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">new</span></div>
<div class="line" id="LC36" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdout</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="no" style="border: 0px; color: teal; margin: 0px; padding: 0px;">OutputFaker</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">new</span></div>
<div class="line" id="LC37" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@robotUI</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span><span class="no" style="border: 0px; color: teal; margin: 0px; padding: 0px;">RobotUI</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">new</span> <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">unless</span> <span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@robotUI</span></div>
<div class="line" id="LC38" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
<div class="line" id="LC39" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">teardown</span></div>
<div class="line" id="LC40" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdin</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="no" style="border: 0px; color: teal; margin: 0px; padding: 0px;">STDIN</span></div>
<div class="line" id="LC41" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdout</span> <span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">=</span> <span class="no" style="border: 0px; color: teal; margin: 0px; padding: 0px;">STDOUT</span> </div>
<div class="line" id="LC42" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
</pre></blockquote>And fourth, the actual use (in this case a test)



<blockquote class="tr_bq"><pre style="border: 0px; color: #333333; font-family: Consolas, 'Liberation Mono', Courier, monospace; font-size: 12px; line-height: 16px; padding: 0px;"><div class="line" id="LC43" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">def</span> <span class="nf" style="border: 0px; color: #990000; font-weight: bold; margin: 0px; padding: 0px;">actual_test</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">(</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">array_of_inputs</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">,</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">expected_output</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">)</span></div>
<div class="line" id="LC45" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
   <span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdin</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">feedStrings</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">(</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">array_of_inputs</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">)</span></div>
<div class="line" id="LC46" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="n" style="border: 0px; margin: 0px; padding: 0px;">array_of_inputs</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">size</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">times</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">{</span><span class="vi" style="border: 0px; color: teal; margin: 0px; padding: 0px;">@robotUI</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">parse</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">()}</span></div>
<div class="line" id="LC47" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
    <span class="n" style="border: 0px; margin: 0px; padding: 0px;">assert_equal</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">(</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">expected_output</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">,</span><span class="vg" style="border: 0px; color: teal; margin: 0px; padding: 0px;">$stdout</span><span class="o" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">.</span><span class="n" style="border: 0px; margin: 0px; padding: 0px;">result</span><span class="p" style="border: 0px; margin: 0px; padding: 0px;">)</span></div>
<div class="line" id="LC48" style="border: 0px; margin: 0px; padding: 0px 0px 0px 10px;">
  <span class="k" style="border: 0px; font-weight: bold; margin: 0px; padding: 0px;">end</span></div>
</pre></blockquote>
