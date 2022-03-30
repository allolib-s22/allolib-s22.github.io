---
num: "Lecture 08"
lecture_date: 2021-04-27
desc: ""
ready: true
---


Demo from Phill: 

* <https://github.com/allolib-s22/demo1-pconrad/blob/master/tutorials/allolib-s22/040_FrereJacquesTwoVoices.cpp>
* We see two `SynthVoice` classes:
  - a `SquareWave` class built with additive synthesis based on the `SineEnv` class from `01_SineEnv`
  - an `FM` class based on the `FM` class from `04_FM`
* We added an enum:
  ```cpp
    enum Instrument
  {
    INSTR_SQUARE,
    INSTR_FM,
    NUM_INSTRUMENTS
  };
  ```
* Then, we added a parameter with a default value to our existing `playSequenceFJ` method:

  Previoiusly:
  ```cpp
  
    case '1':
      std::cout << "1 pressed!" << std::endl;
      playSequenceFJ(1.0, 120);
      return false;

    case '2':
      std::cout << "2 pressed!" << std::endl;
      playSequenceFJ(1.0, 60);
      return false;

    case '3':
      std::cout << "3 pressed!" << std::endl;
      playSequenceFJ(1.0, 240);
      return false;

    case '4':
      std::cout << "4 pressed!" << std::endl;
      playSequenceFJ(1.0, 30);
      return false;
  ```
  
  New:

  ```cpp
   case 'a':
      std::cout << "q pressed!" << std::endl;
      playSequenceFJ(1.0, 120, INSTR_FM);
      return false;

    case 's':
      std::cout << "w pressed!" << std::endl;
      playSequenceFJ(1.0, 60, INSTR_FM);
      return false;

    case 'd':
      std::cout << "e pressed!" << std::endl;
      playSequenceFJ(1.0, 240, INSTR_FM);
      return false;

    case 'f':
      std::cout << "r pressed!" << std::endl;
      playSequenceFJ(1.0, 30, INSTR_FM);
      return false;
    }
  ```
  
  Method definition:
  
  ```cpp
   void playSequenceFJ(float offset = 1.0, float bpm = 120.0, Instrument instrument = INSTR_SQUARE)
  {
    std::cout << "playSequenceFJ: offset=" << offset << " bpm=" << bpm << std::endl;
    Sequence *fjSequence = sequenceFJ(offset);
    playSequence(fjSequence, bpm, instrument);
  }
  ```

  ```cpp
  void playSequence(Sequence *s, float bpm, Instrument instrument = INSTR_SQUARE)
  {
    float secondsPerBeat = 60.0f / bpm;

    std::vector<Note> *notes = s->getNotes();

    for (auto &note : *notes)
    {
      playNote(
          note.getFreq(),
          note.getTime() * secondsPerBeat,
          note.getDuration() * secondsPerBeat,
          note.getAmp(),
          note.getAttack(),
          note.getDecay(),
          instrument);
    }
  }
  
# Changes to playNote

OLD

  ```cpp
  void playNote(float freq, float time, float duration = 0.5, float amp = 0.2, float attack = 0.1, float decay = 0.1)
  {
    auto *voice = synthManager.synth().getVoice<SquareWave>();
    // amp, freq, attack, release, pan
    voice->setTriggerParams({amp, freq, 0.1, 0.1, 0.0});
    synthManager.synthSequencer().addVoiceFromNow(voice, time, duration);
  }
  ```
  
NEW
  ```cpp
     void playNote(float freq, float time, float duration = 0.5, float amp = 0.2, float attack = 0.1, float decay = 0.1, Instrument instrument = INSTR_SQUARE)
    {
      SynthVoice *voice;
      switch (instrument)
      {

      case INSTR_SQUARE:
        voice = synthManager.synth().getVoice<SquareWave>();
        voice->setInternalParameterValue("amplitude", amp);
        voice->setInternalParameterValue("frequency", freq);
        voice->setInternalParameterValue("attackTime", 0.1);
        voice->setInternalParameterValue("releaseTime", 0.1);
        voice->setInternalParameterValue("pan", -1.0);
        break;

      case INSTR_FM:
        voice = synthManager.synth().getVoice<FM>();

        voice->setInternalParameterValue("amplitude", amp);
        voice->setInternalParameterValue("freq", freq);
        voice->setInternalParameterValue("attackTime", 0.1);
        voice->setInternalParameterValue("releaseTime", 0.1);
        voice->setInternalParameterValue("pan", 1.0);

        break;
      default:
        voice = nullptr;
        break;
      }
      synthManager.synthSequencer().addVoiceFromNow(voice, time, duration);
    }
  ```
