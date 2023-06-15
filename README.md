# Component Tester using Arduino Nano
Repository for the Arduino-based project for the course EE381A taken at IITK.

## Project Objectives
The aim is to make a compact and affordable device based on Arduino Nano that can detect whether the device in question is a resistor, capacitor, inductor, BJT or MOSFET while also predicting the characteristics of the devices with reasonable accuracy and displaying it all on an OLED display.

## Motivation
The idea to make such a tester first came up when it was realized how difficult it is to sort the values of resistors and capacitors quickly after we mixed them up in the lab. Also, it is often found that the values of large electrolytic capacitors do not match the advertised values. It is also useful in calibrating circuits as the tester is fairly accurate in measuring the values.

The goal is to create a simple and cheap tester that can be built even by beginners. Existing solution to these problems are quite expensive and include LCR meters that cost dozens of times more.

## Project Timeline
1. [Mapping characteristics of the Arduino and other equipments and deciding upon a circuit that works.](#circuit-schematic)
2. [Implementation.](#implementation)
3. [Finding the accuracy and range of the tester.](#results)

## Components Used
1. Arduino Nano (atmega328p)
2. 3 x 680Ω resistors
3. 3 x 470kΩ resistors
4. 1 x 10kΩ resistor
5. Pushbutton
6. 0.96inch OLED monochrome display (SSD1306)

## Usage
1. The circuit tests the device when the push button is pressed. The component must be inserted into the female header terminal (U2) 
with each leg in separate nodes.
2. The results will appear on the OLED displayed after testing is complete.

## Circuit Schematic

![The Circuit Diagram](https://github.com/ayucd/ComponentTester/blob/main/Circuit.png)

## Implementation
1. **Detection of Diodes:** The tester initially identifies the number of diodes connected to it. It does this by setting on pin to high through 680Ω resistor and checking if the device conducts. The forward voltage is measured but it is exploiting the parasitic capacitance of the diode. The diode is charged with 5V 5ms pulse and **voltage** is observed till conduction stops.

2. **Detection of NPN & PNP Transistors:** If two diodes are found and they have common anode pin but different cathode pin then it 
is a NPN transistor; and if they have common cathode pin and different anode pin then it is a PNP transistor.
  1. **Measurement of NPN Transistor characteristics:** The base pin (which is now identified) is connected to VCC through 680kΩ resistor one the other pins are connected to VCC through 680kΩ resistor and the GND. The voltages are checked at *assumed* collector, base and emitter and DC gain is calculated. If it is less than 1 then collector and emitter are interchanged and the **DC gain** is calculated again. The **base-emitter forward voltage** is also measured.
  2. **Measurement of PNP Transistor:** Here, the base is connected to GND through the 680kΩ resistor and *assumed* emitter is connected to VCC through 680kΩ resistor the collector is connected to GND. The process of finding the device characteristics is similar to that of the NPN transistor.

3. **Detection of MOSFETs:** If a single diode is detected by the tester then it may be the body diode of a MOSFET hence a test is 
performed for it.
  1. **NMOS:** The other pins without the connection of the diode are set to high voltage through a 680kΩ resistor and voltage drop is measured. If the pins conduct then it is an NMOS. The **gate capacitance** and **threshold voltage** are measured by switching the gate pin from high to low and comparing the discharge with capacitor discharge equation and finding out the voltage at which drain-source conduction stops.
  2. **PMOS:** Similar test as NMOS.

4. **Detection of Capacitors:** Whether a capacitor is connected between two pins can be found by charging the pins for 10ms and observing voltage across pins after switching the pins to ground through a resistor, *if the voltage falls slowly* then a capacitor is connected. A 5V 100Hz PWM are applied to the pins through the 680kΩ resistor and voltage is measured in between pulses till the voltage reaches 0.3V at which charging time is calculated and the capacitance is found. The capacitor is then discharged by connecting to ground.

5. **Detection of Resistors:** A resistor when connected through 680kΩ resistor to VCC will behave as voltage divider and hence resistance can be calculated. If the resistance is large then a 470kΩ resistor is used instead of 680kΩ ohm.

6. **Detection of Inductors:** After checking for resistances, the inductors are checked for at the last by *charging of inductor equation*. One of the pins is set to VCC through 680ohm resistor and other to ground. The current through the inductor is measured by measuring voltage drop across 680 ohm resistor and calculating the current. The *change in current with time is compared with the equation* and the inductance is found.

7. The characteristics of the device detected are displayed to the OLED and the **Adafruit GFX library** which provides support for 
the SSD1306 display is used.



