
# Four-Quadrant CMOS Analog Multiplier

This repository presents the design of Four-Quadrant CMOS Analog Multiplier based on Gilbert Cell implemented using Synopsis Custom Compiler on 28nm CMOS Technology as a part of <a href='https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/'>Cloud Based Analog IC Design Hackathon</a>.

# Table of Contents
 * [Introduction](#Introduction)
 * [MOS Differential Pair](#MOS-Differential-Pair)
 * [Gilbert Cell](#Gilbert-Cell)
 * [Attenuator](#Attenuator)
 * [Circuit Diagram](#Circuit-Diagram)
 * [Tools Used](#Tools-Used)
 * [Pre-Layout Schematics](#Pre-Layout-Schematics)
 * [Simulation](#Simulation)
 * [Netlist of the Circuit](#Netlist-of-the-Circuit)
 * [Observations](#Observations)
 * [Author](#Author)
 * [Acknowledgements](#Acknowledgements)
 * [References](#References)

# Introduction:

Bipolar version of the Gilbert cell [1] has been
successfully used for many years in the form of analog
multipliers. Replacing the Bipolar Junction Transistors with
MOSFETs heavily limits the range of the differential input
signals. This is because the output current of the MOS
differential pair depends non-linearly on the tail bias current
and the input signal. The MOS analog multiplier is however
an extremely useful circuit in integrated VLSI systems. In
this design an attempt has been made to increase the input
range of the signal swing by employing attenuating stages
which reduce the input signal levels before giving the input
into the Gilbert cell stage.

# MOS Differential Pair:

The circuit diagram of the MOS Differential Pair is shown in Fig. 1. The sum of the currents flowing through both MOSFETs is constant and equal to the tail current. Under Zero signal conditions, by symmetry half of the Tail current flows in each of the MOSFETs.

<p align="center">
<img width="300"
  src="images/mos_diff_pair_circuit.png"></br>
  Fig. 1: MOS Differential Pair 
</p>

With simple analysis it can be proven that when a differential input voltage signal is applied, the differential output current is represented by the equation: 

<p align="center">
  <img src="/latex/mos_diff_pair_2.png" />
</p>

Where,

<p align="center">
  <img src="/latex/sym_def.png" />
</p>

The equation is valid as long as,

<p align="center">
  <img src="/latex/diff_limit.png" />
</p>

Beyond which the complete tail current will flow in either of the MOSFETs.

For a small signal Vx the differential output current can be approximated to be linear with Vx. Therefore, this circuit can be considered as a current steering circuit.

# Gilbert Cell:

The Gilbert cell uses Three of these differential
pairs and two inputs to steer tail currents depending on the
two inputs.

<p align="center">
<img width="400"
  src="images/gilbert_cell_circuit.png"></br>
  Fig. 2: MOS Gilbert Cell
</p>

Voltage Vx determines the tail current for each of the differential pairs stacked above (M3-M4 and M5-M6). Differential current in M3-M4 and M5-M6 is determined by the input voltage signal Vy. Currents of M3 and M5 are summed up to produce I1 and currents of M4 and M6 are summed up to produce I2. The differential current (I1-I2) is therefore made a function of both Vx and Vy (approximated as the scaled product of Vx and Vy under small signal conditions). This current can then be passed into a pair of resistors to sense them as a differential voltage.

# Attenuator:

A PMOS transistor in the common source configuration with a diode connected NMOS transistor can be used either as an amplifier or attenuator by appropriately varying the device aspect ratio. The configuration is followed by a NMOS source follower.

<p align="center">
<img width="400"
  src="images/att_circuit.png"></br>
  Fig. 3: Attenuator Circuit
</p>

The gain of the given configuration is:

<p align="center">
  <img src="/latex/att.png" />
</p>

This gain formula is valid even for fairly large voltage swings at the input. For attenuation, the denominator term must be greater than the numerator term and this can be achieved by making the aspect ratio of the NMOS device larger than that of the PMOS device.

# Circuit Diagram:

<p align="center">
<img 
  src="images/full_circuit.png"></br>
  Fig. 4: Circuit Diagram
</p>

# Tools Used:

<b>• Synopsys Custom Compiler:</b></br>
&emsp;The Synopsys Custom Compiler™ design environment is a modern solution for full-custom analog, custom digital, and mixed-signal IC design. As the heart of the Synopsys Custom Design Platform, Custom Compiler provides design entry, simulation management and analysis, and custom layout editing features. This tool was used to design the circuit on a transistor level.

<b>• Synopsys Primewave:</b></br>
&emsp;PrimeWave™ Design Environment is a comprehensive and flexible environment for simulation setup and analysis of analog, RF, mixed-signal design, custom-digital and memory designs within the Synopsys Custom Design Platform. This tool helped in various types of simulations of the above designed circuit.

<b>• Synopsys 28nm PDK:</b></br>
&emsp;The Synopsys 28nm Process Design Kit(PDK) was used in creation and simulation of the above designed circuit.

<b>• Krita:</b></br>
&emsp;Digital hand-drawn circuit diagrams used for illustration were made using Krita graphics editor.

# Pre-Layout Schematics:

## Gilbert Cell:

<p align="center">
  <img width="600" src="images/gilbert_cell_schematic.png"></br>
  Fig. 5: Gilbert Cell Schematic
</p>
<p align="center">
  <img width="300" src="images/gilbert_cell_symbol.png"></br>
  Fig. 6: Gilbert Cell Symbol
</p>

## Attenuator:

The lengths of all transistors are 0.03um. The width of the diode connected NMOS device (0.3um) is greater than the width of the PMOS device(0.1um) which makes this circuit function as an attenuator.

<p align="center">
  <img width="400" src="images/att_schematic.png"></br>
  Fig. 7: Attenuator Schematic
</p>
<p align="center">
  <img width="300" src="images/att_symbol.png"></br>
  Fig. 8: Attenuator Symbol
</p>

## Multiplier:

The transistors in the schematic shown in Fig. 9 work as current mirrors and provide bias currents to other blocks in the circuit.

<p align="center">
  <img src="images/multiplier_schematic.png"></br>
  Fig. 9: Multiplier Schematic
</p>
<p align="center">
  <img width="300" src="images/multiplier_symbol.png"></br>
  Fig. 10: Multiplier Symbol
</p>

## Circuit for Testing:
Two of the resistors are used in converting the output differential current into differential voltage. The other resistor is used to set the bias currents for the circuit.
<p align="center">
  <img src="images/testbench_schematic.png"></br>
  Fig. 11: Schematic for the testbench
</p>

# Simulation:

## Transient Analysis:
Two sine waves of different frequencies are being used as inputs. The output wave form is the multiplication of the two sine waves. This waveform can also be considered as Amplitude modulation - DSBSC (Dual side Band Suppressed Carrier).
<p align="center">
  <img src="images/tran1.png"></br>
  Fig. 12: Multiplication of two Sine Waves.
</p>
<p align="center">
  <img src="images/tran2.png"></br>
  Fig. 13: Multiplication of two Sine Waves.
</p>

## Parametric Sweep:
This is used to plot any output attribute over varying input attribute. Here, both the inputs are being varied from -1.5V to +1.5V and the outputs have been plotted.
<p align="center">
  <img src="images/transfer_char.png"></br>
  Fig. 14: Transfer Characteristics of the Multiplier.
</p>

# Netlist of the Circuit:

Refer to the netlist of the circuits here: <a href='/Netlist'>Netlist</a>

# Observations:

The output is fairly linear with respect to both the inputs within the range -1V to +1V. The circuit functions as expected even with frequencies ranging in the order of hundreds of Megahertz. A tradeoff exists between the overall gain obtained and the linearity of the circuit with respect to each input.

# Author:
• Salai Pragadeshwaran B, B.Tech(ECE), National Institute of Technology, Trichy-620015.

# Acknowledgements:
• Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd.</br>
• <a href='https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/'>Cloud Based Analog IC Design Hackathon</a></br>
• <a href='https://www.synopsys.com/'>Synopsys India</a></br>
• <a href='https://www.vlsisystemdesign.com/'>VLSI System Design (VSD) Corp. Pvt. Ltd India</a></br>
• Chinmay panda, IIT Hyderabad</br>
• Sameer Durgoji, NIT Karnataka</br>

# References:
[1] B. Gilbert, “A high-performance monolithic multiplier using active
feedback,” IEEE J. Solid-State Circuits, vol. SC-9, pp. 364-373, Dec.
1974.

[2] SHI-CAI QIN ANDRANDY L. GEIGER, A +-5-V CMOS Analog
Multiplier, IEEE Journal of Solid State Circuits, vol SC-22, pp.1143–
1146, Dec. 1987.

[3] Razavi, Behzad. 2001. Design of analog CMOS integrated circuits.
Boston, MA: McGraw-Hill.
