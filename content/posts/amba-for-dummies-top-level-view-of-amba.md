---
title: "Amba for Dummies: Top Level View of Amba"
date: 2020-05-07T08:01:19-06:00
draft: true
---
*The ARM Advanced Microcontroller Bus Architecture (AMBA) is an open-standard, on-chip interconnect specification for the connection and management of functional blocks in system-on-a-chip (SoC) designs. - Wikipedia*

<!--more-->

If the description above doesn't make much sense to you, no worries, you are not alone. Let's try to translate it to the dummy language:

AMBA is provided by ARM. Yes, the company shares the same name with the CPU architecture that is running in so many mobile phones. ARM, the company, develops ARM architecture, Advanced Risc Machine, and licences it to many other companies who materialize the design into a lot of chips that we use directly or indirectly.

AMBA is one of the designs ARM provides at no cost but a (free) licence is still required. The A(dvanced) and A(rchitecture) in AMBA is easy to understand, but what is a Microcontroller Bus?

A micro controller, or MCU (MicroController Unit) or UC ( μ-controller), can be seen as a small computer on a single integrated circuit, which normally includes a processor, memory and input/output peripherals.

A bus, is a communication system that transfers data between components, which covers all related hardware (wire, optical fiber, etc.) and software and communication protocols. (from Wikipedia)

A Microcontroller Bus is therefore a communication system that transfers data between micro controllers.

So AMBA defines the archictecture and design of hardware and software and protocols used for communication between micro controllers.

Today the scope of AMBA has gone far beyond the micro controller devices and essentially AMBA protocols defines how functional ASIC blocks communicate with each other.

A picture that I stole from the internet, Pic1, illustrates how the AMBA compliant buses (AHB/ASB/APB) connects different ASIC blocks and enables the communication among them.

![pic1](/galleries/amba_top_level_1.png)

**Generations and History**

Up to now there are 5 generations of AMBA developed by ARM (shown in Pic2, yes another picture that I stole from the internet just because I am lazy)

![pic2](/galleries/amba_top_level_2.png)

**AMBA Protocols**

For now you only need to know that

ASB is Advanced System Bus

APB is Advanced Peripheral Bus

AHB is Advanced High-performance Bus

AXI is Advanced eXtensible Interface

ACE is AXI Coherency Extensions

CHI is Coherent Hub Interface
