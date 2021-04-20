---
num: "Lecture 08"
lecture_date: 2021-04-20
desc: ""
ready: false
---


Today I have another demo of code for a canon, but with a difference.

* I wanted to try a new canon, Frere Jacques.
* The first time I entered the melodic material I was dissatisfied with several things:
  - Entering the notes was tedious and repetitious.
  - I was using quarter note = 60, (i.e. ♩ = 60) as the tempo, for convenience of having the numbers on the time line, lining up with the beats
  - But that's WAY TOO SLOW
  - I didn't think I should have to do the math in my head, when we are using a powerful calculating device!
  - Also, the ability to make a phrase an "echo" when it is an exact copy (i.e. softer than the previous phrase) was tedious.  I was having to
    modify individual amplitude values.
  
* So I rolled my own rudimentary sequencing software.
* This allowed me to deconstruct Frere Jacques into four phrases that repeat.
* It also allowed me to work at a higher level of abstraction.

Compare:
* First version (press 1 key to play melody): 
  - <https://github.com/allolib-s21/demo1-pconrad/blob/master/tutorials/allolib-s21/030_FrereJacque.cpp>
* Second version (press 1,2,3,4 or q,w,e,r to play melody):
  - <https://github.com/allolib-s21/demo1-pconrad/blob/master/tutorials/allolib-s21/032_TimeSignature.cpp>
