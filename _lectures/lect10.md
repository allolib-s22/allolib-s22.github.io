---
num: "Lecture 08"
lecture_date: 2021-04-27
desc: ""
ready: true
---


Demo from Phill: 

* <https://github.com/allolib-s21/demo1-pconrad/blob/master/tutorials/allolib-s21/040_FrereJacquesTwoVoices.cpp>
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
