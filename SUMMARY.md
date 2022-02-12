# Playing songs on Arduino
Submission for screening task 10<br>
By :-<br>
**Deepam Priyadarshi**<br>
deepam.priyadarshi2019@vitstudent.ac.in  
deepam.odhisha@gmail.com (personal)
<hr>

## Approach
<p>Since due to inablility of javascript to track event at microseconds level, it is impossible to accurately track the current value at a Buzzer's node/terminal and its rate of change for calculating frequency of the signal.</p>


After looking at the backend code of Arduino's inbuilt `tone()` wriiten by [Brett Hagman (Tone.cpp)](https://github.com/arduino/ArduinoCore-avr/blob/master/cores/arduino/Tone.cpp), I realized that the `tone()` function was making use of CTC mode of the 8-Bit timer2 available on Arduino. The TOP count value for timer2 gets stored in the OCR2A register and the optimal prescaler value is stored in the TCCR2B's clock select bits (last 3 bits). According to the ATmega328P datasheet  
$$F_w = \frac{F_{cpu}}{2N(C+1)}$$  
where $$F_w = frequency\: of\: ouput\: waveform $$  
$$F_{cpu} = frequency\: of\: CPU $$
$$N = prescaler\: value$$
$$C = OCRxA\: value$$
So, by knowing the last 3 values we can calculate the output frequency that the user wants to hear and initialze it to `AudioContext's` Oscillator node's frequency value. Also change the oscillator's waveform type to _square_.  
___
## Steps taken to solve
1. Added `pinNamedMap` property which maps the pin of Arduino to which the Buzzer is connected.

2. Added `arduino` property to Buzzer class which stores the instance of `ArduinoUno` class to which the Buzzer is connected.  

This will allow access to read the various register values needed for the above calculations.

3.  Added a `setInterval()` callback in `initSimulation()` method which repeatedly checks for updates in the register values and updates the oscillator frequency. The ID returned by `setInterval()` is stored in the `setIntervId` property.

4. Finally we call `clearInterval(setIntervId)` in the `closeSimulation()` method.

The only file the was modified was __`ArduinoFrontend/src/app/Libs/outputs/Buzzer.ts`__


___
## Video Demostration

<figure class="video_container">
  <iframe src="hhttps://drive.google.com/file/d/13B3qh1XeQrWI-Zpyic2ifqDlRnKm5oeu/view?usp=sharing" frameborder="0" allowfullscreen="true"> </iframe>
</figure>





