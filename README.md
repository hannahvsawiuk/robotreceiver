# Line following robot

#### Project Specifications
*Brief description:*  An autonomous robot car controlled by a magnetic field produced by a signal generated by a transmission system through a guide wire track. Instructions were sent to the transmitter through an Android app that interfaced with a Bluetooth module (voice and touch capabilities).

*Subjects:* signal processing, resonance, electromagnetism, interrupts and service routines, interrupt prioritization, C, encoded instruction, pulse width modulation, app development, interfacing between microcontrollers and external devices, Bluetooth

*Language:* C

*IDE:* [CrossIDE](http://crosside.software.informer.com/)

*Microcontrollers:*  [C8051F38x](http://www.keil.com/dd/docs/datashts/silabs/c8051f32x.pdf) for the car and [AVR ATmega328p](http://www.atmel.com/Images/Atmel-42735-8-bit-AVR-Microcontroller-ATmega328-328P_Datasheet.pdf) for the transmitter

*Electronic components:* DC motors, optoisolator, MOSFETS, [tank circuits](https://www.youtube.com/watch?v=fQ4yRVEzXQA), operational amplifiers, comparators, precision peak detectors, the [HC-06 Bluetooth module](https://arduino-info.wikispaces.com/BlueTooth-HC05-HC06-Modules-How-To)

*Detailed description*

The transmitter sent an oscillating signal at a 15-kHz frequency through a guide wire track, which generated a magnetic field surrounding the wire. A tank circuit – an inductor in parallel with a capacitor – with a resonant of frequency of 15-kHz acted as an inductive sensor in which a voltage was induced by the magnetic field from the guide wire. To allow for signal processing, the voltage was amplified by a factor of 100 and then fed into a precision peak detector. The peak detector - comprised of a non-inverting operational amplifier, ([Schottky](https://en.wikipedia.org/wiki/Schottky_diode)) diode, and a capacitor - served as a peak detector and a rectifier. The output of the rectifier portion was a DC voltage whose magnitude was proportional to that of the amplified signal. Because the voltage fluctuated accordingly with the position of the car on the track, a comparator with a low [threshold voltage](https://www.google.ca/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&cad=rja&uact=8&ved=0ahUKEwj9xPb99NfTAhUU7mMKHVFuCjsQjRwIBw&url=http%3A%2F%2Fwww.electronics-tutorials.ws%2Fopamp%2Fop-amp-comparator.html&psig=AFQjCNFnDztYyjhqUsjRbk8wrqmwep09xg&ust=1494044860945010) was used to output a logic 1 or 0. The output of the comparator was connected to one of the pins on the F38x microcontroller. This allowed for encoded instructions to be sent in binary, with a 1 generated from the signal being on and 0 from off.

The instructions were sent in five portions: start of instruction indicator, three bits, and end of instruction indicator. Each indicator and bit began with a rising edge of the signal. So, to receive instructions, an external interrupt was configured to execute a service routine in the event of a rising edge at the digital input pin on the F38x. A counter in the interrupt service routine kept track of the number of rising edges. For the bits, at certain intervals, the value of the digital input pin was sampled and stored into a vector. On the edge indicating the end of an instruction, a flag was set which called a function from main that decoded the binary vector and stored the result in a variable. The variable was an input into a set of conditional statements which called the function matching the instruction.

As a bonus feature, an android app was created using the [MIT app creator](http://appinventor.mit.edu/explore/). The app interfaced with a Bluetooth module (HC-06) which was connected to the transmission system. The app allowed for touch selection of instructions, but voice activated controls as well.

[opto-isolator](https://en.wikipedia.org/wiki/Opto-isolator)
