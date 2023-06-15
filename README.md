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
2. 3 680 Ohm Resistors
3. 3 470k Ohm Resistors
4. 10k Ohm resistor
5. Pushbutton
6. 0.96inch OLED Monochrome Display (SSD1306)

## Usage
1. The circuit tests the device when the push button is pressed. The component must be inserted into the female header terminal (U2) 
with each leg in separate nodes.
2. The results will appear on the OLED displayed after testing is complete.

## Circuit Schematic

![The Circuit Diagram](https://github.com/ayucd/ComponentTester/blob/main/Circuit.png)



