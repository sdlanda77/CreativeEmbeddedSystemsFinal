# Perfectly Crisped French Fries

[Blog post](https://edblogs.columbia.edu/comsx3930-001-2021-1/2021/04/22/perfectly-crisped-french-fries-%f0%9f%8d%9f/e)

[Video demo](https://youtu.be/9up8smvd3tY)

## Materials
- ESP32
- Breadboard
- Bidirectional Logic Level Converter (to convert 3.3v GPIO outputs to 5V for stepper driver control)
- 1KOhm resistor
- USB to micro-USB cord
- Thermistor
- Double sided adhesive tape
- Stepper Driver (I used Leadshine DM542)
- Worm Gear Actuator
- Powered by an attached stepper motor
- Fryer (I just used one that my family had and duct taped it to the worm gear)
- L bar (to mount worm gear)
- DC regulated power supply (it was best to supply ~24V to power the driver)
 - I used this same power bank to supply power to the ESP32 through a USB port
## Design
### Physical Material Design

For this part in the process of preparing fries, design was fairly straightforward. It was clear that I had to lift and lower the basket of fries into the basket. My original plan was to use a 4DOF robotic arm that would also let me dump the fries onto a plate afterwards, but I wasn't able to use the one that I was planning because it would not have been able to hold the weight of the basket of fries!  A worm gear was the first thing that came to mind as a simple solution for raising and lowering the basket in a mechanically advantageous way. In extending this the a "vending machine" format, I think it would be really visually engaging to attach two or three of these worm gear/stepper columns in a pinwheel like fashion, and have the pinwheel rotate when fries are done so that instead of requiring additional DOFs in the manipulation of the baskets, we rotate the larger system. This would be easier to control than baskets that dumped fries independently.

### Technical Design

I had to do a bit more reading about stepper motors and drivers since I was using one that was larger than the one that we covered in class and was much less straightforward to configure. I ended up using pulse/direction mode with the DIP pins set to 11000111, which corresponds to peak current of 2.84A, standstill current as half of the dynamic current, and a microstep resolution of 2 (400 steps/rev). To control the stepper driver, I needed at least 5v but the GPIO pins of the ESP32 only send 3.3V, so I used a line converter (the red square component on the breadboard) to convert 3.3V output to 5V output which was then wired to the driver. To drive the motor, I had one GPIO pin that switched between high and low to control direction, and another that was constantly pulsing to drive the motor.

I also wired a thermistor, which I connected to the metal part of the basket, near the oil to monitor the temperature. While it never happened with this small basket, this was really important if this was to be used in a restaurant setting since overheating will happen or in a vending machine setting so that the machine can know that the oil is getting too hot (for safety reasons/emergency shutoff/call for maintenance). If the metal was ever hotter than 180 degrees Celsius (~360 Fahrenheit) then the fries would be removed and would not be dipped back in to the oil until someone came and restarted the program. I used the code from Ch 13 of the ESP32 tutorial to read the temperature.

## Not on wiring
When wiring the stepper driver to the motor, make sure that you have the correct pair of wires for each coil (commonly this is red/blue and green/black or yellow but motor specs or a power supply and miltimeter can help confirm!). Refer to the code for which GPIO pins to use. The names of the variables correlate to the stepper driver, with the GPIO pins always going into A- and B- and a 5v pin going into A+ and B+. Additionally, ensure that all the circuitry has common ground. Finally, make sure that the output from your GPIO pins is sufficient for controlling th driver. In my case, I had to use a line converter to increase it to 5V from 3.3v but this will be different with different microcontrollers and drivers. 
