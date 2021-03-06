# pLaunchController
A JAVA wrapper for the Novation Launch Controller aimed at using the MIDI pads and knobs as input for Processing sketches.

![](pLaunchController.gif)

# Installation
## From Processing editor
In Processing, go to `Sketch`, `Import library...`, `Add library`. Search for "Launch Controller" and once found, click `Install`.
## Manual installation
Copy the file pLaunchController.jar to a folder `code` inside your sketch. This method makes the library available to an individual sketch.
If you intend to make the library available to all sketches, unzip the pLaunchController.zip file to the libraries of your Processing installation (you can see the default skecthbook location in File -> Preferences).

# Usage
To start using the library, make sure Novation Launch Controller is connected to your computer (at least one led is lit).
## Add reference to library
  1. At the top of your sketch, import the namespace `pLaunchController`:
      ```JAVA
      import pLaunchController.*;
      ```
   
   2. Declare a global variable of type `LaunchController`
        ```JAVA
          LaunchController midiController;
        ```
   3. Instantiate the controller during `setup()`:
   
  ```JAVA
      try {
          midiController = new LaunchController(this);
      }
       catch(Exception e) {
          println("Error connecting to MIDI device! Sketch will run with UI controllers. values.");
          midiController = null;
      }
  ```
  4. Optionally, use `range(float minValue,float maxValue)` to override the output of knob values, and `defaultValue(float value)` to set an initial value. By default, knobs will return values from 0 to 127.
    
  ```JAVA
    controller.getKnob(KNOBS.KNOB_1_HIGH).range(10,200).defaultValue(h);
  ``` 
  
  5. Implement one of the following methods in your sketch:
  
     * `void launchControllerKnobChanged(KNOBS knob)`
     
        Called when a knob was changed.
     * `void launchControllerPadChanged(PADS pad)`
     
        Called when you push a pad.
     * `void launchControllerControlChanged()`
     
        Called when either a pad or knob changes.
  
  An example where I use `LaunchController.onKnobChanged(KNOBS knob)` to set a few variables:
   ```JAVA
    void launchControllerKnobChanged(KNOBS knob) {
      println("Launch Control knob changed: " + knob.name());

      //Updates the values of h and base_w with the knob values
      //Note that MIDI notes are 0-127, but you can override that in setup() by
      //calling `range(float minValue,float maxValue)`
      //For example: controller.getKnob(KNOBS.KNOB_1_HIGH).range(10,200)
      h = controller.getKnob(KNOBS.KNOB_1_HIGH).value();
      base_w = controller.getKnob(KNOBS.KNOB_2_HIGH).value();
    }   
   ```
