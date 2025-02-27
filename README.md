Download link :https://programming.engineering/product/memory-mapped-i-o-polling-and-timers/

# Memory-Mapped-I-O-Polling-and-Timers
Memory Mapped I/O, Polling and Timers
Due date/time: During your scheduled lab period in the week of February 5, 2024. [Due date is not when Quercus indicates, as usual.]

Learning Objectives

The goal of this lab is to explore the use of devices that provide input and output capabilities for a processor, and to see how a processor connects to those inputs and outputs, through the Memory Mapped method. You’ll also be introduced to a device that has a special purpose in computer systems, the Timer, which is used for precise measurement and allocation of time within the computer.

What To Submit

You should hand in the following files prior to the end of your lab period. However, note that the grading takes place in-person, with your TA:

• The assembly code for Parts I,II, III and IV in the files part1.s, part2.s, part3.s and part4.s.

Background

There are two basic techniques for synchronizing with I/O devices: program-controlled polling and interrupt-driven approaches. We will use the polling approach in this exercise, and interrupts will be covered in the next lab.

In general, an embedded system like the one we are using in the lab, has what is called a parallel port that provides for data transfer to or from external input/output devices such as the LEDs or Switches or Keys on the DE1-SoC board. The transfer of data may involve from 1 to 32 bits, and we call it ‘in parallel’ if there is more than 1 (and ’serial’ if just one bit at a time). The number of bits, n, and the type of transfer depend on the specific parallel port being used. The parallel port interface we will use contains the four registers shown in Figure 1. Although we are calling these registers, they shouldn’t be confused with the 32 registers in the Processor. These reside in the input/output unit, and as taught in class, they are accessed through memory mapping, and so have a specific addressed assigned to them. Each register is n bits long. The registers have the following roles:

Data register: holds the n bits of data that are transferred between the parallel port and the NIOS II processor. It can be implemented as an input, output, or a bidirectional register.

Direction register: defines the direction of transfer for each of the n data bits when a bidirectional interface is generated.

Interrupt-mask register: used to enable interrupts from the input lines connected to the parallel port.

Edge-capture register: indicates when a change of logic value is detected in the signals on the input lines connected to the parallel port. Once a bit in the edge capture register becomes asserted, it will remain asserted. An edge-capture bit can be de-asserted by writing to it using the NIOS II processor.

Part I

You are to write a NIOS II assembly language program that displays a binary number on the 10 LEDs (that you’ve been using in the previous labs) under control of the four pushbuttons (also called KEYs) on the DE1-SoC board.

The DE1-SoC Computer contains a parallel port connected to 10 red LEDs on the board. Figure 2 shows the ad-

dress we’ve used for the LEDs and picture of the register, taken from page 4 of the DE1-SoC_Computer_NiosII.pdf document that was given out in Lab 1. You may wish to read that document more now that we’re getting deeper into the labs in this course.


Figure 2: The parallel ports connected to the 10 red LEDs

The following functionality should be included:

If KEY0 is pressed on the board, you should set the binary number displayed on the 10 LEDs to be 1 in base 10, which we will annotate from now on as 110 (or in base 2 as 00000000012).

If KEY1 is pressed then you should increment the displayed number, but don’t let the number go above 1510 (i.e. pressing the key won’t change the value if it is already at 1510 or 11112).

If KEY2 is pressed then decrement the number, but don’t let the number go below 1 (i.e. pressing the key won’t change the value on the LEDs if it is already 1).

Pressing KEY3 should blank the display (which is the same as 0), and pressing any other KEY after that should return the display to 1.

The parallel port connected to the pushbutton KEYs has the base address 0xFF200050, as illustrated in Figure 3. In your program, use the polling I/O method to read the Data register to see when a button is being pressed.

When you are not pressing any KEY the Data register provides 0, and when you press KEYi the Data register provides the value 1 in bit position i. Once a button-press is detected, be sure that your program waits until the button is released. IMPORTANT: You must not use the Interruptmask or Edgecapture registers for this part of the exercise. Doing this way first (a later section changes this) will teach you partly why the edge capture register and associate hardware are useful.

Create a new folder to hold your solution for this part and put your code into a file called part1.s. You will need to make a Monitor Program project for running your code on a DE1-SoC board; but you can also debug your program at home using CPUlator. Show it working to your TA for grading in the lab. Submit part1.s to Quercus before the end of your lab period.

Part II

Write a NIOS II assembly language program that displays binary counter on the 10 LEDs. The counter should be incremented approximately every 0.25 seconds. When the counter reaches the value 25510, it should start again at 0. The counter should stop/start when any pushbutton KEY is pressed.

this bit in your program to make the NIOS II processor wait for this event to occur. To reset the TO bit to 0 you have to write the value 0 into this bit-position.


Figure 4: The Interval Timer registers.

Put your code into a folder called part3 and a file called part3.s, and test and debug your program and show it working to your TA for grading. Submit part3.s to Quercus before the end of your lab period.

Part IV

In this part you are to write an assembly language program that implements a real-time binary clock. Display a binary version of time on the 10 LEDs on the DE1-SoC board. We want to display both seconds and hundredths of a second. The seconds should be displayed on the high-order 3 bits of the red LEDS (i.e. LEDR9:7) and the hundredths of seconds should be displayed on the low-order 7 bits (i.e. LEDR6:0). Your code should keep the seconds and hundredth seconds counting separately, which means you’ll need to write think about how to code so that they work correctly together.

Measure time intervals of 0.01 seconds in your program by using polled I/O with the Timer. You should be able to stop/run the clock by pressing any pushbutton KEY. When the clock reaches 810 seconds 9910 hundreths, it should wrap around to 000:0000000.

Put your code into a folder called part4 and a file called part4.s, and test and debug your program and show it working to your TA for grading. Submit part4.s to Quercus before the end of your lab period.
