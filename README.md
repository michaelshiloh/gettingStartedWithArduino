# gettingStartedWithArduino
A brief introduction to Arduino

This page is at github.com/michaelshiloh/gettingStartedWithArduino/

I apologize for the mess. This page was copied from a wiki and has not yet been reformatted.

Arduino
[Examples](http://teachmetomake.com/wordpress/arduino-project-examples)

The will focus on getting you started with Arduino quickly, so that you will understand the basic procedures for working with Arduino and can explore further on your own.

We will cover how to install Arduino on your laptop; how to understand, modify, and write Arduino programs; how to connect sensors to Arduino and read them from a program; and how to connect actuators (LEDs, motors, speakers) and control them from a program. Other topics will be covered as interest dictates and time permits.

Contents [hide]

    1 Before the Workshop
    2 At the Workshop
    3 First steps: Verifying correct installation
    4 What’s going on: How to use Arduino to turn something On and Off
    5 How do we add our own things (LEDs, motors, speakers, sensors, etc.)
    6 How to use a sensor
    7 Communicating with Arduino
    8 analogWrite(): Controlling speed or brightness
    9 What else can analogWrite() do?
    10 Arduino outputs: Voltage and current
    11 Controlling large loads with a transistor
    12 More to explore
    13 Examples of Projects for Inspiration
    14 Where to go next
    15 Workshop kit contents. You can  purchase the kit  from Oddwires 

Before the Workshop

Please bring your laptop. Arduino runs on Windows, Mac OSX, and Linux. Please download the (free) Arduino software prior to the workshop from http://arduino.cc/en/Main/Software, and install as much as you can. Instructions at http://arduino.cc/en/Guide/HomePage

An Arduino kit will be provided for use during the workshop or you may bring your own.

If you have questions prior to or after the workshop please contact m.shiloh@arduino.cc
At the Workshop

Please interrupt anytime with questions.
First steps: Verifying correct installation

All of this is in the Arduino Getting Started guide for your operating system.

    Connect Arduino via USB cable
        Windows? Might see “New Hardware Discovered” and later might see “New Hardware Ready for Use”.
        Mac OS X? Might see “New Network Interface Found”. Click “Network Preferences…”,  click “Apply”, and when it finishes, click “Close”. It doesn’t matter if the configuration fails.
        Linux? Nothing to do here
    Open Arduino software (IDE)
    Select Tools -> Board. You have an Uno.
    Select Tools -> Serial Port
        Windows? Chose the largest COM number
            No COMs? Raise your hand for help or visit troubleshooting
        Mac OS X? Chose either usbmodem
            No usbmodem? Raise your hand for help or visit troubleshooting
        Linux? There is only one choice
    Open File->Examples->Basics->Blink
    Click “Upload”
    Look for errors in the bottom window of the program
        Errors? Raise your hand for help or visit troubleshooting
    Look for the amber LED on your Arduino to be blinking
        No blinking? Raise your hand for help or visit troubleshooting

What’s going on: How to use Arduino to turn something On and Off

How does the program (sketch) do this? (all described in the  Blink tutorial)

/*
  Blink
  Turns on an LED on for one second, then off for one second, repeatedly.
 
  This example code is in the public domain.
 */
 
// Pin 13 has an LED connected on most Arduino boards.
// give it a name:
int led = 13;

// the setup routine runs once when you press reset:
void setup() {                
  // initialize the digital pin as an output.
  pinMode(led, OUTPUT);     
}

// the loop routine runs over and over again forever:
void loop() {
  digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);               // wait for a second
  digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);               // wait for a second
}

How do we add our own things (LEDs, motors, speakers, sensors, etc.)

The Solderless Breadboard (tutorial)

sbb_insideUse it to add an external LED. LEDs must always be used with resistors so they don’t burn . The resistor value can be anywhere from 220 ohm to 1 k ohm. The lower the resistance, the brighter the light:

led13bb_tHere’s another view:

Blink_bb

Use the Blink sketch: File -> Examples -> Basics -> Blink (think about why)

And here is a schematic of this circuit:

Blink_schem(Fritzing)

Exercise 1: Move LED to a different pin (e.g. pin 8). See if you can figure out how to do this from the schematic alone:

BlinkPin8_schemNow the LED won’t blink until you change the program, since the program is only turning pin 13 on and off. Change the program to control pin 8.

Exercise 2: If you changed the program to control only pin 8, then the built-in LED on pin 13 is no longer blinking. Can you change the program to make them both blink?

Are we limited to LEDs? No; we could replace the LED (and its resistor) with any other suitable device, with some considerations. We’ll learn more about this later.
How to use a sensor

So far we’ve only used Arduino as an output device, to control something in the physical world (the LED). The other way of interfacing to the physical world is as an input device, using a sensor to get information about the physical world. We’ll start with a photoresistor, also called a light dependent resistor or LDR. It’s a resistor whose resistance depends on the light: the more light, the lower the resistance. (The resistor we used above with the LED is a fixed resistor.)

The LDR indicates the amount of light by changing its resistance, but Arduino can not measure resistance. Arduino can measure voltage. Fortunately, we can easily convert a varying resistance to a varying voltage using a fixed resistor to create a voltage divider. This time the fixed resistor needs a larger resistance, so select a 10 k ohm resistor and build the circuit below. You don’t need to remove the LED circuit as there should be room on your breadboard for both, and we’ll use the LED again later.

arduinoLDR_bb arduinoLDR_schemOpen and upload this sketch:

File->Examples->Basics->AnalogReadSerial

How do you know if anything is working? Arduino might be reading the sensor, but is it telling you anything? Read the Arduino tutorial to find out.
Communicating with Arduino

Arduino is connected to your computer, so they can communicate. Let’s look at the AnalogReadSerial sketch:

Serial.println(sensorValue);

This allows Arduino to send a message to your laptop. In order to see this message you need to open the Serial Monitor by clicking on the magnifying glass near the top right corner.

(By the way, this also allows a program on your laptop to communicate with a program on your Arduino. See

File->Examples->Communication

This is covered more in the intermediate Arduino workshops.)
analogWrite(): Controlling speed or brightness

If digitalWrite() can turn an LED on and off, and analogRead() can read a range of values, what would you guess analogWrite() might do?

Move the LED to pin 11:

BlinkPin11_schemNow upload this sketch:

File -> Examples -> Basics -> Fade

What’s the LED doing? Can you figure out how the sketch is doing this?

analogWrite(led, brightness);

and then

brightness = brightness + fadeAmount;

How does it know to start fading down when it reaches the maximum value?

  if (brightness == 0 || brightness == 255) {
    fadeAmount = -fadeAmount ; 
  }

Why did I ask you to move the LED to pin 11? It turns out analogWrite() only works on certain pins: 3, 5, 6, 9, 10, and 11. (reference)
What else can analogWrite() do?

It turns out that analogWrite() also works well to control the speed of a motor. However now we need to consider whether our motor is compatible with Arduino’s outputs.
Arduino outputs: Voltage and current

 

When used as outputs, two things must be considered: the voltage and the current. Our Arduino can deliver 5 v, and at most 40 mA.

The voltage is determined by Arduino, but the current is determined by whatever we’re trying to control. In the case of the LEDs, they only need 20 mA or less. The motor we have might take more than 40 mA. In the worst case, when it’s stalled, it might want a 200 mA.

The important thing to realize is that Arduino does not have the ability to limit this current. It will try to deliver whatever is asked of it, even if it overheats and damages itself.

If we want to control a device that might take more than 40 mA, we have to use an intermediary.
Controlling large loads with a transistor

arduinoLEDVin_bb

The transistor is like a bicycle gear: you control it with a small amount of current, and it in turn can control a lot more current.

The transistor also allows us to use a higher voltage than the 5V arduino can deliver.

You can test this with either

File -> Examples -> Basics -> Blink
File -> Examples -> Basics -> Fade

References:

    irlb8721pbf datasheet
    tutorial

More to explore

    digitalRead()
        What is a switch?
        arduinoWireSwitch_bb
        Arduino->File->Examples->Basics->DigitalReadSerial
        Digital Read Serial tutorial
    Making sounds: Melody tutorial
    Joining inputs and outputs: switch controls speed, switch choses between two brightness levels, thermistor or other sensor changes behavior, etc.
    Multiple output devices: play melody while controlling motor speed, etc.
    The problem with, and a solution to, delay(): BlinkWithoutDelay tutorial
    Boolean logic, tests, and conditionals

Examples of Projects for Inspiration

    One website’s opinion of the top 40 Arduino Projects
    Interfacing with Hardware
    Arduino related Instructables
    Google anything you can think of and the word “Arduino”

Where to go next

    You can always email me
    Free “Arduino in a Nutshell” booklet
    Many excellent Arduino books, e.g. “Getting Started with Arduino“
    Local places to work on projects and get informal help
        Open hack night every Wednesday at Tech Liminal in Oakland
        Hacker spaces often welcome non-members to bring projects or questions
            Noisebridge (San Francisco)
            Sudo room (Oakland)
            Hacker Dojo (Mountain View)
            Ace Monster Toys (Oakland)
    Websites with parts, tutorials, examples
        Arduino
        Adafruit Industries
        Sparkfun
        Tronixstuff
        NYU ITP
    Online resources
        Arduino forum
        Arduino Google+
        Massive list of resources
        Sensor workshop at ITP, including long list of different sensors
        Michael’s pages

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img
alt="Creative Commons License" style="border-width:0"
src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This
work is licensed under a <a rel="license"
href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons
Attribution-ShareAlike 4.0 International License</a>.
