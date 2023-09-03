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

![Ids vs Vds](./Images/nfet_Ids_vs_Vds.png)

These plots provide valuable insights into the characteristics of the NMOS transistor.

## 3. Unraveling the CMOS Inverter

### 3.1 The Fascination of CMOS Circuits
In our journey, we've made an intriguing observation: neither NMOS nor PMOS transistors alone can produce both HIGH and LOW logic values. However, their complementary behavior inspired the idea of combining them. By placing PMOS as a Strong 1 between VDD and the output (Vout) and NMOS as a Strong 0 between Vout and GND, we create what is known as a Complementary Metal-Oxide-Semiconductor (CMOS) configuration. This forms the foundation of the simplest circuit: the CMOS inverter.

![CMOS Inverter](./Images/CMOS_Inverter_Schematic.png)

CMOS circuits are divided into two networks: the pull-up network (P-channel MOSFETs) and the pull-down network (N-channel MOSFETs). This ingenious arrangement ensures that only one transistor is on at any given time, eliminating resistive paths to ground and allowing for the generation of Strong High and Strong Low outputs.

### 3.2 In-Depth Analysis of the CMOS Inverter (Pre-Layout)
Before we dive into the intricacies of the CMOS inverter, let's revisit its fundamental purpose. An inverter, in essence, performs the NOT logic operation, complementing its input. Ideally, the output should faithfully follow the input with no delay or propagation issues. However, real-world inverters can be quite complex, with various factors affecting their performance, including speed, load tolerance, noise susceptibility, bandwidth, and more.

This section serves as a comprehensive exploration of inverter analysis, encompassing schematic design, parameter measurement, experimentation, and the development of a circuit capable of meeting our initial objectives. Our schematic design is based on the earlier-determined (W/L) ratios, with PMOS having an S (Aspect Ratio) of 4 times that of NMOS, and NMOS having S = 1/0.30 microns. A symbol for this design is also created for ease of schematic creation.

![CMOS Inverter Schematic](./Images/CMOS_inv_sch_and_sym.png)

With our schematic in place, we embark on a series of calculations. Throughout this journey, the term (W/L) is referred to as S or Aspect Ratio for simplicity. We utilize a designated testbench for transient and DC analysis:

![CMOS Inverter Testbench](./Images/cmos_inv_tb.png)

#### 3.2.1 DC Analysis and Key Design Parameters
DC analysis plays a pivotal role in plotting the Voltage Transfer Characteristics (VTC) curve of the CMOS inverter. This analysis involves sweeping the Vin (input voltage) from high to low to observe how the circuit responds to different input voltage levels. The resulting VTC curve reveals crucial insights into the inverter's behavior.

![CMOS Inverter VTC](./Images/cmos_inv_dc_anal.png)

The VTC curve depicts how the output changes concerning variations in the input voltage. In our case, the curve resembles a square wave (albeit non-ideal), transitioning around 0.75 volts of input. Five distinct regions of operation emerge, each corresponding to the behavior of the NMOS and PMOS transistors with respect to input potential changes.

To fully understand the inverter's performance, we examine five critical parameters:
- VOH (Maximum output voltage for logic '1')
- VOL (Minimum output voltage for logic '0')
- VIH (Maximum input voltage interpreted as logic '0')
- VIL (Minimum input voltage interpreted as logic '1')
- Vth (Inverter Threshold voltage)

Notably, Vth should ideally be at VDD/2 for maximum noise margins. Initially, achieving this precise value might be elusive, but adjustments to device parameters can bring us closer to this target. Our simulations help us identify optimal parameters to achieve a Vth closer to Vdd/2, enhancing noise margins.

Our quest for inverter excellence also reveals that the total power dissipation is a mere 5.55u Watts for our device. Notably, power consumption spikes only during state transitions, which occur within the transition region, spanning a width of (VIH - VIL) = 0.24V.

Join us on this thrilling journey of discovery into the fascinating world of inverter design and analysis!
