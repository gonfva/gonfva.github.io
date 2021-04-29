---
layout: post
title: "Little robot (II): Java version"
date: 2012-08-19T17:00:02
comments: false
categories: Developer
---

As I wrote in a <a href="http://gonfva.blogspot.com/2012/08/little-robot-i.html" target="_blank">previous post</a>, I was asked to&nbsp;design a software managing a robot inside a board of 5x5 cells. The typical commands were<br /><div><br /><div style="text-align: left;"><blockquote class="tr_bq">PLACE X,Y,F -&gt;Places the robot in the x,y position facing F<br />MOVE -&gt;Move the robot 1 cell in the facing direction<br />LEFT -&gt;Changes the facing&nbsp;counter-clockwise<br />RIGHT -&gt;Changes the facing clockwise<br />REPORT -&gt;Shows the current position and facing</blockquote></div><br />and the main requirement was that the robot didn't fall outside the board. The assignment allowed to read commands from the console (standard input/output) or from a file. You can read the assignment <a href="https://github.com/alexwibowo/Robot/blob/master/README" target="_blank">here</a>, but that's not my code.
<br /><br />
Initially I didn't want to focus on the input/output, so I decided to test only the engine. Without writing a line of functional code, I developed a <a href="https://github.com/gonfva/assignments/blob/master/gfvRobotJava/test/gfv/robot/RobotTest.java" target="_blank">JUnit test class</a> just from translating the requirements into code. It wasn't pure TDD, because there weren't tests for individual methods. But it gave me the security net I needed to play a bit with the design.
<br /><br />
It was obvious for me that the there was going to be a class between the user and the engine, mediating input and output. For me that class was the UI of the engine. I could think of a console UI, a file UI, or even a graphical UI. The implementation I decided was a <a href="https://github.com/gonfva/assignments/blob/master/gfvRobotJava/src/gfv/robot/RobotConsoleUI.java" target="_blank">console UI</a> (read from standard input and standard output), but I decided not to make tests for that class. Probably I should have done the tests, and I don't love the code in the method <b>parseLine()</b>.
<br /><br />
Following that, I focused on the <a href="https://github.com/gonfva/assignments/blob/master/gfvRobotJava/src/gfv/robot/Robot.java" target="_blank">engine class</a>. Should the engine class have everything in it? Should it manage variables independently? I did different implementations, refactoring the code, peaceful, knowing my tests were saving the bacon. In the end I decided to manage whole objects with the whole status (both position x,y and facing f). That way I was thread safe: either the new position is good, (and it becomes the current position), or bad (and I kept the previous). I played a lot with the names of these entities, not deciding between Status, and Position. I didn't love either of them, but chose <a href="https://github.com/gonfva/assignments/blob/master/gfvRobotJava/src/gfv/robot/Position.java" target="_blank">Position</a>. I also played a lot with the facing. Should I use constants in Position class, or an enum?&nbsp;I thought about creating a service implementing the commands, doing it in the Robot engine, in the position, or in the enum?
<br /><br />
The bad thing about an assignment like this is that you have too many options. The code in Java wasn't perfect, but I guess it was good enough.
<br /><br />
<br /></div>
