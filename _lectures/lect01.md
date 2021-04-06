---
num: "Lecture 01"
lecture_date: 2021-04-01
desc: "A focus on tutorials/synthesis/01_SineEnv.cpp"
ready: true
---


Today we did a deep dive into one file in the repo:

* <https://github.com/AlloSphere-Research-Group/allolib_playground/>

Namely this one:

* [`tutorials/synthesis/01_SineEnv.cpp`](https://github.com/AlloSphere-Research-Group/allolib_playground/blob/master/tutorials/synthesis/01_SineEnv.cpp)


By going through the code, and also looking some things up in the documentaiton here: <https://allosphere-research-group.github.io/allolib-doc/>, we learned a few things:

# `Mesh` for graphics

This `mMesh` object, an instance of `Mesh` is what supports the graphics.

[link to `Mesh` docs](https://allosphere-research-group.github.io/allolib-doc/classal_1_1_mesh.html)

```cpp
// Additional members
  Mesh mMesh;
```

# Modifying the graphics

Here is the original code for the graphics:

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
```

We noted that we can add additional params with the `getInternalParameterValue` statement, such as:


```cpp
    float attackTime = getInternalParameterValue("attackTime");
    float pan = getInternalParameterValue("pan");
```

