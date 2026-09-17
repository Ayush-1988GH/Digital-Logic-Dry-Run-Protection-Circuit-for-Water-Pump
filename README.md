# Digital-Logic-Dry-Run-Protection-Circuit-for-Water-Pump
Digital Water Pump Dry-Run Protection and Automatic Shutdown System

1. Aim :
To build a digital circuit that automatically switches off a water pump if it senses the pump 
is running without water for too long, and to make the circuit remember this fault until it is 
cleared by pressing a RESET button manually.

2. Apparatus / Materials Required :
 IC / Part No.  Component                    Qty
 74LS74         D Flip-Flop                   1
 74LS161        4-bit Synchronous Counter     1
 74LS85         4-bit Magnitude Comparator    1
 74HC02         Quad 2-Input NOR Gate         1
 74LS04         Hex Inverter (NOT gate)       1
 74LS08         Quad 2-Input AND Gate         1
 NE555          Timer IC (clock generator)    1
 BC547          NPN Transistor                2
 TIP122         NPN Darlington Transistor     1
 1N5408         Flyback Diode                 1
 LM2596         Buck Converter Module         1
 Mini DC Submersible Pump (3-6V) / Load LED
 Metal / Stainless Steel Probes
 Breadboard (830 point)
 Resistors: 100k, 10k, 4.7k, 22k, 1k, 100k pot 
 Capacitors: 10uF, 10nF, 100nF, 100uF 
 Push Buttons (START and RESET)
 LEDs (fault / status indication)
 12V DC Adapter
 Connecting wires, Multimeter

3. Theory :
A water pump can get damaged if it keeps running when there is no water at its inlet. 
This is called 
dry running
. A simple ON/OFF switch cannot detect this because it has 
no memory and no idea of time. This project solves the problem using only digital logic ICs, 
with no microcontroller or coding involved. 
Two metal probes are dipped in the water. When water touches both probes, a small 
current flows between them. A BC547 transistor senses this tiny current and converts it 
into a clean HIGH or LOW logic signal called WATER. This signal is inverted to get 
NO_WATER.
A 74LS74 flip-flop remembers whether the pump has been commanded ON by the START 
button — this is the circuit's memory. A 74LS08 AND gate combines the pump-ON signal 
with NO_WATER to create the DRY_RUN signal: it only becomes HIGH when the pump is 
running and water is missing at the same time.
An NE555 timer produces a slow clock, about one pulse per second, so the fault takes a 
few seconds to confirm instead of tripping on a single splash. A 74LS161 counter counts 
these clock pulses only while DRY_RUN is HIGH. A 74LS85 comparator watches the 
count and produces a pulse the moment it reaches four i.e. 0100 .
This pulse is short, so a 74LS02 NOR latch is used to remember the fault permanently 
(until RESET). The latched fault output turns on a transistor that clears the 74LS74 flip-flop, 
which switches the pump OFF through a BC547 + TIP122 driver stage. Pressing RESET 
clears both the counter and the latch so the demonstration can be repeated.

4. Procedure :
Set the LM2596 output to exactly 5.0V using a multimeter before connecting any IC.
Wire the water probes with transistor Q1 and one 74LS04 gate. Test that the WATER 
signal goes HIGH when the probes are dipped in water.
Wire the 74LS74 flip-flop with the START push button. Confirm Q_PUMP goes HIGH on 
START and stays HIGH.
Build the NE555 stable clock and adjust the potentiometer until it blinks about once a 
second.
Wire the 74LS08 AND gates to form DRY_RUN and COUNTER_RUN from Q_PUMP, 
NO_WATER and RESET_N.
Connect the 74LS161 counter to the clock and COUNTER_RUN. Confirm it counts 1, 2, 
3, 4 with LEDs.
Wire the 74LS85 comparator with the B input fixed at 0100 (decimal 4). Confirm its A=B 
output goes HIGH at count 4.
Build the 74LS02 NOR latch. Test that a brief SET pulse holds the fault HIGH until 
RESET is applied.
Connect the latched fault through transistor Q3 to the 74LS74 clear pin, so a fault 
clears the pump memory.
Build the pump driver (Q2 predriver, TIP122, flyback diode). Test switching with the 
pump disconnected and load LED first, then connect the real pump.
Wire the RESET push button to clear the flip-flop, counter and latch together.
Run the complete demonstration: press RESET, press START, remove the probes 
from water, watch the count reach 4, confirm the pump switches off on its own, then 
press RESET to repeat.

5. Precautions :
Always check the LM2596 output with a multimeter and set it to 5.0V before connecting 
any IC.
Keep the water container and the electrical breadboard physically separate from each 
other.
Use only the low-voltage pump and probes near water. Never bring the 12V adapter or 
mains supply close to water.
Do not let the pump run dry for a long time, even while testing — a few seconds is 
enough to confirm the fault.
Place a 100nF capacitor close to the VCC-GND pins of every IC to avoid false switching 
from supply noise.
Double-check every pin number against the datasheet before wiring — a wrong 
connection can damage an IC.
Keep the pump's current-carrying wires separate from the logic wiring to avoid motor 
noise resetting the ICs.
Verify the flyback diode's direction across the pump before switching it on.

6. Conclusion :
The circuit successfully senses missing water, waits for a few seconds to confirm the fault 
instead of reacting to a single splash, counts this time digitally, and switches the pump off 
on its own once the fault is confirmed. The fault stays remembered until RESET is 
pressed, so the pump cannot restart by accident. The complete decision-making path 
uses only standard digital ICs — no microcontroller or software was needed — which 
shows how combinational logic, flip-flops, counters, a comparator and a latch can work 
together to solve a real, practical problem.
