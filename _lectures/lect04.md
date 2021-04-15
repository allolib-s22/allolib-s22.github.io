---
num: "Lecture 04"
lecture_date: 2021-04-13
desc: "High Level Agenda for course, lab00, additive synthesis"
ready: false
---


# A high level agenda for the course

In this course, I'd like us to try to both
* Learn Allolib
* Forge a path to help future students learn Allolib

That includes both:
* creating curriculum materials (collaboratively, as class&mdash;not just me!)
* make improvements to the software itself, especially around how it is packaged and presented





# Additive Synthesis, illustrated through Audacity


First, a 10 minute video: <https://www.youtube.com/watch?v=YsZKvLnf7wU>

The formula for a square wave is the sum of the odd harmonics, each multiplied by 1/n where n is the harmonic number, e.g.

$$ a\sin(x) + \frac{a}{3}\sin(3x) + \frac{a}{5}\sin(5x)  + \frac{a}{7}\sin(7x) \dots  $$


What that looks like when graphed is here: <https://www.youtube.com/watch?v=Lu2nnvYORec>

Here's a spreadsheet where we can calculate the amplitude and harmonic values: 
* <https://docs.google.com/spreadsheets/d/1N9A6Jpid7ClQaMbIHqghPa53KN8YsDMdrmdCOg5oF5w/edit?usp=sharing>

```
Fundamental Frequency	Amplitude	
440	0.8	
```		

```
Harmonic	Frequency	Amplitude
	(Harmonic * Fundamental)	Amplitude * (1/harmonic)
1	440	0.800000
3	1320	0.266667
5	2200	0.160000
7	3080	0.114286
9	3960	0.088889
11	4840	0.072727
13	5720	0.061538
```
We can use Audacity <https://www.audacityteam.org/> to illustrate this and hear it.


And here's how to do it using Allolib:

* <https://github.com/allolib-s21/demo1-pconrad/blob/master/tutorials/allolib-s21/010_MakeSquareWave.cpp>

