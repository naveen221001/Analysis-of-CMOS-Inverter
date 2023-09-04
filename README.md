# Analysis-of-CMOS-Inverter
# Innovative Inverter Design and Analysis with sky130pdk

### Unveiling the Secrets of Cutting-Edge CMOS Inverter Design Using sky130pdk and Open Source Tools

Prepare to embark on a journey into the world of inverter design like never before! This project is your gateway to exploring the intricacies of inverter operation and gaining a deep understanding of all the parameters that influence its performance. We will harness the power of the skywater 130nm pdk and a range of open-source tools, including Xschem, NGSPICE, MAGIC, Netgen, and more, to unravel the mysteries of inverter design.

Our adventure commences with a comprehensive analysis of NMOS and PMOS devices, focusing on the 1.8V standard models provided by the pdk. The goal is to establish a common working W/L (Width/Length) ratio for these transistors. Once we've laid this foundation, we'll dive into the exciting world of CMOS inverter design. This phase includes schematic creation and the meticulous measurement of critical parameters such as propagation delays, noise margins, rise and fall times, and power consumption. 

Our journey also serves as a captivating case study in utilizing SPICE's programming capabilities to enhance our measurement and analysis skills. If all goes according to plan, this project will conclude with a deeper appreciation for inverter design and its pivotal role in modern electronics.

![Innovative Inverter Design](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/690b1025d4a3408ecee18e3514bacb3a6c73f2d8/Images/Untitled%20design(1).png)

---

## Table of Contents
- [1. Tools and PDK Setup](#1-tools-and-pdk-setup)
  - [1.1 Setting Up the Essential Tools](#11-setting-up-the-essential-tools)
- [2. Exploring MOSFET Models](#2-exploring-mosfet-models)
  - [2.1 General MOSFET Analysis](#21-general-mosfet-analysis)
- [3. Unraveling the CMOS Inverter](#3-unraveling-the-cmos-inverter)
  - [3.1 The Fascination of CMOS Circuits](#31-the-fascination-of-cmos-circuits) 
  - [3.2 In-Depth Analysis of the CMOS Inverter (Pre-Layout)](#32-in-depth-analysis-of-the-cmos-inverter-pre-layout)
    - [3.2.1 DC Analysis and Key Design Parameters](#321-dc-analysis-and-key-design-parameters)
    - [3.2.2 Power Analysis (Transient) and Key Design Parameters](#322-dc-analysis-and-key-design-parameters)
    - [3.2.3 Transient Analysis via graph for rise delay and fall delay](#323-rise-fall-delay)


## 1. Tools and PDK Setup

### 1.1 Setting Up the Essential Tools
To embark on our inverter design and analysis journey, we've carefully chosen a set of indispensable tools:
- Ngspice: Your trusty companion for Spice netlist simulations. [Ngspice Website](http://ngspice.sourceforge.net/)
- Magic: The enchanting world of layout design and DRC awaits you with Magic. [Magic Website](http://opencircuitdesign.com/magic/)
- Xschem: Your go-to tool for schematic capture. [Xschem Website](http://repo.hu/projects/xschem/)

## 2. Exploring MOSFET Models

### 2.1 General MOSFET Analysis
Our exploration begins with a detailed analysis of the MOSFET models available within the sky130 pdk. We'll be focusing on the 1.8V transistor models (pfet_01v8.sym and nfet_01v8.sym). However, feel free to experiment with other available models. Below, you'll find the schematic created in Xschem.

![NMOS Characteristic Schematic](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/bc18b4025a63b19ca93839e8e4a99ac9e75c18fa/Images/1.png)

The components used include:
- nfet_01v8.sym from the xschem_sky130 library
- vsource.sym from the xschem devices library
- code_shown.sym from the xschem devices library

We'll use this setup to generate basic characteristic plots for an NMOS Transistor, such as Ids vs Vds and Ids vs Vgs. To achieve this, save the circuit with the specified component placements, and then click on Netlist and Simulate. Ngspice will take over, performing the necessary simulations and calculations. Afterward, you can use commands like display and setplot to explore and plot the available vectors.

![Ids vs Vgs](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/6eaf9a24c4e4a2eb5563ec2f756f1ea19e873f24/Images/2.png)

![Ids vs Vds](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/78b674dc546dc17b7bb73cdd93c312c69e0fb32b/Images/4.png)

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/6831eb9d596684340219a443b74ec002571e2937/Images/3.png)

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/09f7df0cdd83c1d58c33778f41e02ddb51091bdc/Images/5.png)

These plots provide valuable insights into the characteristics of the NMOS transistor.

## 3. Unraveling the CMOS Inverter

### 3.1 The Fascination of CMOS Circuits
In our journey, we've made an intriguing observation: neither NMOS nor PMOS transistors alone can produce both HIGH and LOW logic values. However, their complementary behavior inspired the idea of combining them. By placing PMOS as a Strong 1 between VDD and the output (Vout) and NMOS as a Strong 0 between Vout and GND, we create what is known as a Complementary Metal-Oxide-Semiconductor (CMOS) configuration. This forms the foundation of the simplest circuit: the CMOS inverter.

![CMOS Inverter](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/8adafb402c0aa5abaa3e30440ae0a10521eaf826/Images/6.png)

CMOS circuits are divided into two networks: the pull-up network (P-channel MOSFETs) and the pull-down network (N-channel MOSFETs). This ingenious arrangement ensures that only one transistor is on at any given time, eliminating resistive paths to ground and allowing for the generation of Strong High and Strong Low outputs.

### 3.2 In-Depth Analysis of the CMOS Inverter (Pre-Layout)
Before we dive into the intricacies of the CMOS inverter, let's revisit its fundamental purpose. An inverter, in essence, performs the NOT logic operation, complementing its input. Ideally, the output should faithfully follow the input with no delay or propagation issues. However, real-world inverters can be quite complex, with various factors affecting their performance, including speed, load tolerance, noise susceptibility, bandwidth, and more.

This section serves as a comprehensive exploration of inverter analysis, encompassing schematic design, parameter measurement, experimentation, and the development of a circuit capable of meeting our initial objectives. Our schematic design is based on the earlier-determined (W/L) ratios, with PMOS having an S (Aspect Ratio) of 4 times that of NMOS, and NMOS having S = 1/0.30 microns. A symbol for this design is also created for ease of schematic creation.

![CMOS Inverter Schematic](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/9856dc33073a5358ab8b366cb8f25b35c88eda44/Images/Screenshot%20from%202023-09-03%2018-34-41.png)
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/29c91c5c97c22c2f548507241ee6646a570d14a0/Images/Screenshot%20from%202023-09-03%2018-33-59.png)


With our schematic in place, we embark on a series of calculations. Throughout this journey, the term (W/L) is referred to as S or Aspect Ratio for simplicity. We utilize a designated testbench for transient and DC analysis:

![CMOS Inverter Testbench](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/f371a07dca4f3ed7dc2790a670def3cf8084cf1a/Images/Screenshot%20from%202023-09-03%2018-32-44.png)

#### 3.2.1 DC Analysis and Key Design Parameters
DC analysis plays a pivotal role in plotting the Voltage Transfer Characteristics (VTC) curve of the CMOS inverter. This analysis involves sweeping the Vin (input voltage) from high to low to observe how the circuit responds to different input voltage levels. The resulting VTC curve reveals crucial insights into the inverter's behavior.

![CMOS Inverter VTC](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/b3f5caa74480e708472256973612410b3ff9e541/Images/12.png)

The VTC curve depicts how the output changes concerning variations in the input voltage. Five distinct regions of operation emerge, each corresponding to the behavior of the NMOS and PMOS transistors with respect to input potential changes as shown below:

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/b37b2ac31eec2fbc16c7fe2be70f8ede35aa6d90/Images/xOvt4.jpg)


To fully understand the inverter's performance, we examine five critical parameters:
- VOH (Maximum output voltage for logic '1')
- VOL (Minimum output voltage for logic '0')
- VIH (Maximum input voltage interpreted as logic '0')
- VIL (Minimum input voltage interpreted as logic '1')
- Vth (Inverter Threshold voltage)

Notably, Vth should ideally be at VDD/2 for maximum noise margins. Initially, achieving this precise value might be elusive, but adjustments to device parameters can bring us closer to this target. Our simulations help us identify optimal parameters to achieve a Vth closer to Vdd/2, enhancing noise margins.In our simulation the noise margin is pretty good, till 0.73 V the input voltage is identified as logic '0' and 0.98 as logic '1'. 

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/81adef315dc10f5933679fdfb1186144430725bc/Images/7.png)

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/5053726dd34804a8fd07cb0af90eb71ce53647c0/Images/8.png)

#### 3.2.2 Power Analysis (Transient) and Key Design Parameters

For power calculations, we need to do the trnsient analysis because the average power is integrating power from 0 to a finite time and then divide it by time.
In this analysis we are taking the width of pmos as 2 and that of nmos at 1.
The plot of current vs Vout in the figure shown below signifies that while charging the load capacitance, the current is drawn from the Vdd source. But when it discharges the current is constant because, during the second half cycle, the pmos is off, and the current in nmos directly discharges to the ground, there is no connection between Vdd and nmos during this cycle.

There are majorly three types of power we come across in our study.
1)Dynamic Power: Represented in blue, whenever we are switching we have this dynamic power.
2)Short Circuit Power: It come only due to non-ideality of our input, clock, as in our simulation we have finite value of Vin pulse. Due to this we have a situation where both our pmos and nmos are on, so we have a path directly from Vdd to Ground. Its is somewhat represented by red when the curve is not constant.
3)Static Power: This happens when either of nmos or pmos is off. And, is represented by red where the curve is constant.

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/3c064298f050441677c519ce4066c168dca5bd6e/Images/19.png)

![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/3c064298f050441677c519ce4066c168dca5bd6e/Images/18.png)

Calculating Power:
Taking a time period, where we will integrate the current over that period and multiply that with the voltage 1.8v and then divide the whole thing by the time to get the average power over that cycle.
Our quest for inverter excellence also reveals that the total power dissipation is less for our device. Notably, power consumption spikes only during state transitions, which occur within the transition region.
We can also reduce the power consumption by reducing the switching activity, Vdd and load capacitance.
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/3c064298f050441677c519ce4066c168dca5bd6e/Images/16.png)

#### 3.2.3 Transient Analysis for Propagation Delay, RIse delay and Fall delay

Propagation Delay: We do the transient analysis to find out the time distance when input goes 50% of the original value corresponding the output transtion to 50% of the actual.
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/047db69ca1ed95e5ebd097a988f888e28f7a2d12/Images/21.png)
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/047db69ca1ed95e5ebd097a988f888e28f7a2d12/Images/24.png)
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/047db69ca1ed95e5ebd097a988f888e28f7a2d12/Images/25.png)
![..](https://github.com/naveen221001/Analysis-of-CMOS-Inverter/blob/047db69ca1ed95e5ebd097a988f888e28f7a2d12/Images/26.png)


Rise Time & Fall Time Calculation: 
Rise time can be calculated as the time taken by the voltage output to rise from 10% of its value to 90%.
Fall time can be calculated as the time taken by the voltage output to fall from 90% of its value to 10%.

![..]()

Join us on this thrilling journey of discovery into the fascinating world of inverter design and analysis!
