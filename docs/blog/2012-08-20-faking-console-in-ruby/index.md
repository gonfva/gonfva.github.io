---title: Faking the console in Ruby 
date: 2012-08-20T12:31:38Z
categories: Developer
tags: [Developer]
---

A few days ago, I was preparing an assignment for a job process both in Java and in Ruby. [While doing in Java, I decided to test the engine]({{< ref "2012-08-19-little-robot-ii-java-version">}}). But in Ruby I wanted to test the whole application, including the communication via standard input/output on the console.


And how do you mock standard input/output in Ruby?. Well. It turns out it is quite [easy to do it with Rspec](http://stackoverflow.com/questions/6335282/testing-with-stdin-and-stdout-in-rspec). But I wanted to do it without any additional requirement. How do you mock STDIN and STDOUT in plain Ruby.


It turns out it isn't difficult. Based on [this gist](https://gist.github.com/194554), I prepared my [own solution](https://github.com/gonfva/assignments/blob/master/gfv_robot_ruby/tc_robot_console.rb). First the input faker:

```ruby
class InputFaker
  def feedStrings(strings)
    @strings = strings
  end

  def gets
    next_string = @strings.shift
    next_string #TODO: I guess next_string is superflous
  end
end
```


Second the output faker

```ruby
class OutputFaker
  def initialize
    @result=[]
  end

  def write(str)
    @result << str.chomp
  end

  def clear
    @result=[]
  end

  def result
    @result.join
  end
end
```

Third, setup and teardown methods

```ruby
  def setup
    $stdin = InputFaker.new
    $stdout = OutputFaker.new
    @robotUI=RobotUI.new unless @robotUI
  end

  def teardown
    $stdin = STDIN
    $stdout = STDOUT
  end
```

And fourth, the actual use (in this case a test)

```ruby
  def actual_test(array_of_inputs,expected_output)
    $stdin.feedStrings(array_of_inputs)
    array_of_inputs.size.times{@robotUI.parse()}
    assert_equal(expected_output,$stdout.result)
  end
```
