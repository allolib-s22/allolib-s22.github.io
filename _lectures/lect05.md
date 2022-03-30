---
num: "Lecture 05"
lecture_date: 2021-04-15
desc: "Keyboard events"
ready: false
---


Today I demonstrated some code that demonstrates a few things:

* How to play notes in code directly rather than from a sequence file
* How to use the keyboard to trigger other kinds of events (e.g. an entire sequence of notes) rather than
  just a single note
* The musical idea of a Canon, i.e. a piece where melodies are repeated at various intervals, sometimes 
  unchanged, and sometimes modified: [Wikipedia article](https://en.wikipedia.org/wiki/Canon_(music))
  - faster or slower
  - higher or lower in pitch
  - by inversion (a mirror image of the pitches rising vs. falling) 
  - or retrograde (i.e. playing the melody backwards) 
  
A "round" is a kind of canon&mdash;all rounds are canons, though not all canons are rounds.

A famous round is "Row, Row, Row Your Boat", which is the melodic material in this piece:

* <https://github.com/allolib-s22/demo1-pconrad/blob/master/tutorials/allolib-s22/020_Sequence.cpp>

We also heard several other demos by students.


# My new piece

Not claiming it's "great" art or even "good" art.  It's just "playing".

Don't necessarily aim for great art.  Just "play".  Occasionally something good, or something great will pop out.

* Tell: The story about the pottery class where half were graded on quantity and half were graded on quality.

  * The half that was graded on quantity also produced the best pottery.
  * Because they just cranked out pottery, and the practice made them good at it.
  * The ones that aimed for perfection got stuck, frustrated, and didn't learn as much.

So here's my "not great art piece".

* <https://github.com/allolib-s22/demo1-pconrad/blob/master/tutorials/allolib-s22/020_Sequence.cpp>

It's a study on "Row, Row, Row your boat".

A few techniques it illustrated:
* Using the keyboard to trigger events other than note on, note off
* Hard coding sequences, and then parameterizing those
* The ability to perform a [canon](https://en.wikipedia.org/wiki/Canon_(music))
* The abilty to perform a canon at various intervals.

# Picking up from last time, and clarifying

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

# Keyboard Event

In `01_SineEnv`, the computer's "qwerty" keyboard is mapped to pitches like this:

<img src="https://docs.google.com/drawings/d/e/2PACX-1vTrjghT3Pbc4_QEYIkrnehlIn20whDW7zvGom31EmaxOzuh5UruT4fq_4SH8PQnvyBRybMSpKBFRyUW/pub?w=932&amp;h=100">

The code to do that is here:

```
// Whenever a key is pressed, this function is called
  bool onKeyDown(Keyboard const& k) override {
    if (ParameterGUI::usingKeyboard()) {  // Ignore keys if GUI is using
                                          // keyboard
      return true;
    }
    if (k.shift()) {
      // If shift pressed then keyboard sets preset
      int presetNumber = asciiToIndex(k.key());
      synthManager.recallPreset(presetNumber);
    } else {
      // Otherwise trigger note for polyphonic synth
      int midiNote = asciiToMIDI(k.key());
      if (midiNote > 0) {
        synthManager.voice()->setInternalParameterValue(
            "frequency", ::pow(2.f, (midiNote - 69.f) / 12.f) * 432.f);
        synthManager.triggerOn(midiNote);
      }
    }
    return true;
  }
```

A few things to note in this line of code.

```
 synthManager.voice()->setInternalParameterValue(
            "frequency", ::pow(2.f, (midiNote - 69.f) / 12.f) * 432.f);
```

First, it would be better style to have declared:

```cpp
  const float A4 = 432.f; // or alternatively 440.f
```

and then written:

```cpp
synthManager.voice()->setInternalParameterValue(
            "frequency", ::pow(2.f, (midiNote - 69.f) / 12.f) * 432.f);
```	    

The magic number `69.f` here is the midi note for A4 on the piano, which is the pitch corresponding to `432.f`. 

The relationship between pitches on a well-tempered keyboard instrument is that octaves are separated by doubling.

So, if we want to "double" a number in 12 small steps, we need to multiply each step by $$ 2^{\frac{1}{12}} $$.

That's where we get the formula:

```
pow(2.f, (midiNote - 69.f) / 12.f) * 432.f)
```

The function ` int midiNote = asciiToMIDI(k.key());` is found in this file:

* <https://github.com/AlloSphere-Research-Group/allolib/blob/master/src/scene/al_PolySynth.cpp>

If you wanted to modify the keyboard layout that's the code you'd need to modify, e.g. if you wanted to:
adapt the keyboard for something other than NOT qwerty, e.g dvorak, or azerty, etc.

But there are other possibilities too.

We could eliminate the keyboard mapping to piano keyboard idea altogeher, and do something completely different:
* Map keys to drum sounds
* Map keys to chords
* Map keys to whole sequences

Note that we need a way to signal when a note is finished also: 


```
  // Whenever a key is released this function is called
  bool onKeyUp(Keyboard const& k) override {
    int midiNote = asciiToMIDI(k.key());
    if (midiNote > 0) {
      synthManager.triggerOff(midiNote);
    }
    return true;
  }
```

# Current barriers to entry with Allolib

1. The fact that you need the whole thing to be able to do any work
   * To work with Allolib Playground at the moment, you need to download the entire repo, and run your little piece of code within the context of the entire repo.
   * Back before it was on GitHub, it was distributed as a `.zip` file, and you had to do the same; run your one little file in the context of the entire code base.
   * The way that folks shared their demos was to email their one little file of code, which the instructor would then copy into their giant code base, and run.

   It seems like there should be a better way; i.e. you should be able to have a separate independent repo with your Allolib project, and just
   declare Allolib as a dependency, and have it pulled in automatically.  Or perhaps you compile Allolib down to a library or set of libraries that
   you copy into your repo.
   
2. Working with the repo creates a lot of cruft that should not necessarily be committed to the repo.
  
   The `.gitignore` for the repo could use some work.  (I've talked with Andres about this and he's open to pull requests on that.)
   
3. It would be nice if there were a series of more "gradual" examples; the [tutorials/synthesis/01_SineEnv.cpp](https://github.com/AlloSphere-Research-Group/allolib_playground/blob/master/tutorials/synthesis/01_SineEnv.cpp) is already way more complicated than a `Hello World` program might be.    

   It would be nice if we presented a series of smaller steps where the current [tutorials/synthesis/01_SineEnv.cpp](https://github.com/AlloSphere-Research-Group/allolib_playground/blob/master/tutorials/synthesis/01_SineEnv.cpp) was the *ending* point rather than the starting point, so that newcomers could understand all of the steps along the way.
   
4. It would be nice if there were an FAQ for new Allolib users, produced by new Allolib users, with the FAQs that they themselves (i.e. we ourselves!) 
   actually encounter.
   
What else would you add to this list?  Lets take a moment on the "General" channel of the slack <https://allolib-s22.slack.com> to enter some ideas.






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

* <https://github.com/allolib-s22/demo1-pconrad/blob/master/tutorials/allolib-s22/010_MakeSquareWave.cpp>

# Deconstructing `01_SineEnv.cpp`

In this lecture, we'll work through the file [tutorials/synthesis/01_SineEnv.cpp](https://github.com/AlloSphere-Research-Group/allolib_playground/blob/master/tutorials/synthesis/01_SineEnv.cpp) and try to make sense of it.

# The big picture

The file contains two C++ classes:
* `class SineEnv : public SynthVoice {`
* `class MyApp : public App {`


A few notes:
* The `SynthVoice` parent class from which `SineEnv` inherits encapsulates all of the proceses for sound and graphics.  It can be instantiated multiple times, and then gets rendered in a processing chain.
* The `App` parent class is the one that provides the facilities for applications: window, audio context, keyboard callback, etc.

The glue between them is a convenience class calls `SynthGUIManager<SineEnv>` which is parameterized with the class `SineEnv` derived from `SynthVoice`.

This makes the GUI from the primary stuff that the voice provides.

# Starting with methods of the App

*   `void onCreate() override {`  for "once per app" stuff
*   There is also a `  void onInit() override {` which is different becuase it happens before the contexts are initialized.
*   `  void onSound(AudioIOData& io) override {` is called once per full audio buffer of sound.. (not once per sample).
*   `  void onAnimate(double dt) override {` called once per "simulation frame", which is tied to the graphics frame
    - you get information about the time.   this is more like the "event loop in a game", which might be in a different thread from the
      actual graphics processing, though by default it isn't... by default it is synchronized... by default it is called at the frame
      rate of the graphics...    If my graphics frame rate is 30fps, this gets called 30 times per second, ideally... but the onDraw might
      get called, for example, 60 times per second if we are doing stereo vision or multi-pass rendering...   You might get slow rates
      on the onAnimate if the processing takes longer than a frame time.. 
*   `void onDraw(Graphics& g) override {
    - this is one pass of a framebuffer..   this might get called twice per frame, e.g. if there is stereo vision, or multi-pass rendering. 

We also see the onKeyDown and onKeyUp which control the keyboard mapping to the musical keyboard in this case.


# Deconstructed Code


Let's divide up the parts of [tutorials/synthesis/01_SineEnv.cpp](https://github.com/AlloSphere-Research-Group/allolib_playground/blob/master/tutorials/synthesis/01_SineEnv.cpp)
and try to understand them


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

# `Mesh`


```cpp
  // Additional members
  Mesh mMesh;
```

# `void init() override`

Here's the first block of code.

```cpp
  // Initialize voice. This function will only be called once per voice when
  // it is created. Voices will be reused if they are idle.
  void init() override {
    // Intialize envelope
    mAmpEnv.curve(0);  // make segments lines
    mAmpEnv.levels(0, 1, 1, 0);
    mAmpEnv.sustainPoint(2);  // Make point 2 sustain until a release is issued

    // We have the mesh be a sphere
    addDisc(mMesh, 1.0, 30);
```

This second block of code shows  a quick way to create parameters for the voice. Trigger
parameters are meant to be set only when the voice starts, i.e. they
are expected to be constant within a voice instance. (You can actually
change them while you are prototyping, but their changes will only be
stored and aplied when a note is triggered.)

```
    createInternalTriggerParameter("amplitude", 0.3, 0.0, 1.0);
    createInternalTriggerParameter("frequency", 60, 20, 5000);
    createInternalTriggerParameter("attackTime", 1.0, 0.01, 3.0);
    createInternalTriggerParameter("releaseTime", 3.0, 0.1, 10.0);
    createInternalTriggerParameter("pan", 0.0, -1.0, 1.0);
  }
```

# `void onProcess(AudioIOData& io) override`


Get the values from the parameters and apply them to the corresponding
unit generators. You could place these lines in the onTrigger() function,
but placing them here allows for realtime prototyping on a running
voice, rather than having to trigger a new voice to hear the changes.

Parameters will update values once per audio callback because they
are outside the sample processing loop.


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

# `void onProcess(Graphics& g) override {`

```cpp
  // The graphics processing function
  void onProcess(Graphics& g) override {
    // Get the paramter values on every video frame, to apply changes to the
    // current instance
    float frequency = getInternalParameterValue("frequency");
    float amplitude = getInternalParameterValue("amplitude");
    // Now draw
    g.pushMatrix();
    g.translate(frequency / 200 - 3, amplitude, -8);
    g.scale(1 - amplitude, amplitude, 1);
    g.color(mEnvFollow.value(), frequency / 1000, mEnvFollow.value() * 10, 0.4);
    g.draw(mMesh);
    g.popMatrix();
  }

  // The triggering functions just need to tell the envelope to start or release
  // The audio processing function checks when the envelope is done to remove
  // the voice from the processing chain.
  void onTriggerOn() override { mAmpEnv.reset(); }

  void onTriggerOff() override { mAmpEnv.release(); }
};
```

# `class MyApp : public App {`

``` cpp
// We make an app.
class MyApp : public App {
 public:
  // GUI manager for SineEnv voices
  // The name provided determines the name of the directory
  // where the presets and sequences are stored
  SynthGUIManager<SineEnv> synthManager{"SineEnv"};

  ...
```

```cpp

  // This function is called right after the window is created
  // It provides a grphics context to initialize ParameterGUI
  // It's also a good place to put things that should
  // happen once at startup.
  void onCreate() override {
    navControl().active(false);  // Disable navigation via keyboard, since we
                                 // will be using keyboard for note triggering

    // Set sampling rate for Gamma objects from app's audio
    gam::sampleRate(audioIO().framesPerSecond());

    imguiInit();

    // Play example sequence. Comment this line to start from scratch
    synthManager.synthSequencer().playSequence("synth1.synthSequence");
    synthManager.synthRecorder().verbose(true);
  }
```

# `void onSound(AudioIOData& io)` 

```cpp

  // The audio callback function. Called when audio hardware requires data
  void onSound(AudioIOData& io) override {
    synthManager.render(io);  // Render audio
  }
```

#   `void onAnimate(double dt) override {`


```cpp
  void onAnimate(double dt) override {
    // The GUI is prepared here
    imguiBeginFrame();
    // Draw a window that contains the synth control panel
    synthManager.drawSynthControlPanel();
    imguiEndFrame();
  }
```

#   `void onDraw(Graphics& g) override `


```cpp
  // The graphics callback function.
  void onDraw(Graphics& g) override {
    g.clear();
    // Render the synth's graphics
    synthManager.render(g);

    // GUI is drawn here
    imguiDraw();
  }
```

#   `bool onKeyDown(Keyboard const& k) override`


```cpp
  // Whenever a key is pressed, this function is called
  bool onKeyDown(Keyboard const& k) override {
    if (ParameterGUI::usingKeyboard()) {  // Ignore keys if GUI is using
                                          // keyboard
      return true;
    }
    if (k.shift()) {
      // If shift pressed then keyboard sets preset
      int presetNumber = asciiToIndex(k.key());
      synthManager.recallPreset(presetNumber);
    } else {
      // Otherwise trigger note for polyphonic synth
      int midiNote = asciiToMIDI(k.key());
      if (midiNote > 0) {
        synthManager.voice()->setInternalParameterValue(
            "frequency", ::pow(2.f, (midiNote - 69.f) / 12.f) * 432.f);
        synthManager.triggerOn(midiNote);
      }
    }
    return true;
  }
```

# `  bool onKeyUp(Keyboard const& k) override {`

```cpp
  // Whenever a key is released this function is called
  bool onKeyUp(Keyboard const& k) override {
    int midiNote = asciiToMIDI(k.key());
    if (midiNote > 0) {
      synthManager.triggerOff(midiNote);
    }
    return true;
  }

  void onExit() override { imguiShutdown(); }
};
```

# `int main() {`

```cpp
int main() {
  // Create app instance
  MyApp app;

  // Set up audio
  app.configureAudio(48000., 512, 2, 0);

  app.start();
  return 0;
}
```



