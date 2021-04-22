---
num: "Lecture 04"
lecture_date: 2021-04-15
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


# Unit Generators

Near the top of the we see that there is a collection of public data members all declared under the comment `// Unit Generators`.

The idea of a Unit Generator has been a basic concept in sound synthesis for about long as folks have been using electronics and computers to do analog and digital synthesis.  The Wikipedia article (as of 04/13/2021) is a bit disapponting; it might be a nice class project for someone to take on improving both the content and the sources: <https://en.wikipedia.org/wiki/Unit_generator>, but it does have the basic idea.

```cpp
  // Unit generators
  gam::Pan<> mPan;
  gam::Sine<> mOsc;
  gam::Env<3> mAmpEnv;
  // envelope follower to connect audio output to graphics
  gam::EnvFollow<> mEnvFollow;
```

In Gamma (the part of Allolib that does sound synthesis), these Unit Generator objects provide a new value with each "tick of the clock".

* By "each tick of the clock" what we mean is, if we are sampling at 44100 Hz, each tick of the clock is 1/44100 of a second.   
* Different unit generators can work in different *domains* (e.g. audio vs. graphics domain)

If you define different domains, some unit generators can work in the graphics domain at 30fps, while others are in the audio domain.

There is a default domain however. 

The unit generators get "wired together", so to speak in the audio callback.  (See below.)


# `void onProcess(AudioIOData& io) override`


Get the values from the parameters and apply them to the corresponding
unit generators. You could place these lines in the onTrigger() function,
but placing them here allows for realtime prototyping on a running
voice, rather than having to trigger a new voice to hear the changes.

Parameters will update values once per audio callback because they
are outside the sample processing loop.

My understanding is that the loop ` while (io()) {` processes one sample at a time from a block of samples contained in an audio buffer
available through `AudioIOData& io`.   (I may have misspoken in class and implied that the `onProcess` function is called once
per sample; instead, I think it is more accurate to say that the `while (io())` loop is executed once for each sample.


```cpp
  // The audio processing function
  void onProcess(AudioIOData& io) override {
    mOsc.freq(getInternalParameterValue("frequency"));
    mAmpEnv.lengths()[0] = getInternalParameterValue("attackTime");
    mAmpEnv.lengths()[2] = getInternalParameterValue("releaseTime");
    mPan.pos(getInternalParameterValue("pan"));
    while (io()) {
      float s1 = mOsc() * mAmpEnv() * getInternalParameterValue("amplitude");
      float s2;
      mEnvFollow(s1);
      mPan(s1, s1, s2);
      io.out(0) += s1;
      io.out(1) += s2;
    }
    // We need to let the synth know that this voice is done
    // by calling the free(). This takes the voice out of the
    // rendering chain
    if (mAmpEnv.done() && (mEnvFollow.value() < 0.001f)) free();
  }
```
