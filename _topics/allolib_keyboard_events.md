---
topic: "allolib: keyboard events"
desc: "Making something happen when you press a key"
category_prefix: "allolib: "
indent: true
---


Inside a class that derives from `App`, i.e. inside:

```
class MyApp : public App {
```

you will find this method:

```
  // Whenever a key is pressed, this function is called
  bool onKeyDown(Keyboard const& k) override {
  ...
  }
```

A common case is that this method contains code that defines the computer's "qwerty" keyboard
layout to correspond to a conventional keyboard.
