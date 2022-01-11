# Lesson outline
- Abstractions
- Hardware Resources
- OS Functionality
- OS Abstractions (Managing the CPU and Memory)
By lesson end, ready to discuss structure of OS

# L01a: Principle of Abstraction
![image](https://user-images.githubusercontent.com/62491253/149030892-f78b48c2-8b01-4b75-afe3-ec8a686a52d6.png=100x20)
![image](https://user-images.githubusercontent.com/62491253/149030914-b6e3f04f-01c7-41f6-9f56-233238e6892b.png=100x20)

1. Abstraction: A well understood interface that hides all details within a subsystem

![image](https://user-images.githubusercontent.com/62491253/149031233-3af9c2cd-f7a3-4fa3-8a37-7c210ca77958.png=100x20)
![image](https://user-images.githubusercontent.com/62491253/149031308-4d971f1d-fd4c-4aa3-bbba-d0fe3e0627b1.png=100x20)

2. Digging Deeper Into the Power of Abstractions
  - a series of abstractions sit between Google Earth and Electron holes
   - From lowest level of the hierarchy to highest
     - **Electron and holes**
     - **Transistors**: transistor reigns in the randomness in the movement of electrons and holes, and gives us an abstraction of a switching device, which is the transistor
     - **Logic gates**: And-gate, not-gate & or-gate where we implement boolean logic using transistor as a switching device
     - **Sequential and combination logic elements**: implemented using logic gates such as and, not and or. Logic elements are then organized into a data pack, depending on what hardware circuitry or functionality we want to implement
     - **Machine Organization (data path + control)**: The data pack establishes the communication paths we need between these combinational and sequential logic elements. To realize whatever is the hardware device that we are trying to design, and control part of it is a finite shaped machine that controls the data path and implements the repertoire of the hardware device that we are trying to realize. For instance, if the intent is to implement a processor, then the instructions in the processor have to be implemented using the data pad, and controlled by the control logic
     - **Instruction set architecture (ISA)**: The abstraction defined by a processor/CPU, like "intel inside" ads is the ISA been talked about by the promo.The ISA is all the details of how the ISA is actually implemented by the data path and the control logic
     - **System software(OS,compilers,etc)**
     - **Applications**

3. Digging Deeper Into the Power of Abstractions
