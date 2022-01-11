# Lesson outline
- Abstractions
- Hardware Resources
- OS Functionality
- OS Abstractions (Managing the CPU and Memory)
By lesson end, ready to discuss structure of OS

# L01a: Principle of Abstraction
1. Abstraction: A well understood interface that hides all details within a subsystem
   - An abstraction example (we don't know how it is implemented and we don't care) - door knob, Instruction set architecture, AND logic gate, Transistor, Int data type
   - Not an abstraction - Nums of pins coming out of chips, Exact location of the base pads in a baseball field
2. Digging Deeper Into the Power of Abstractions
   - A series of abstractions sit between Google Earth and Electron holes
   - From lowest level of the hierarchy to highest
     - **Electron and holes**
     - **Transistors**: transistor reigns in the randomness in the movement of electrons and holes, and gives us an abstraction of a switching device, which is the transistor
     - **Logic gates**: And-gate, not-gate & or-gate where we implement boolean logic using transistor as a switching device
     - **Sequential and combination logic elements**: implemented using logic gates such as and, not and or. Logic elements are then organized into a data pack, depending on what hardware circuitry or functionality we want to implement
     - **Machine Organization (data path + control)**: The data pack establishes the communication paths we need between these combinational and sequential logic elements. To realize whatever is the hardware device that we are trying to design, and control part of it is a finite shaped machine that controls the data path and implements the repertoire of the hardware device that we are trying to realize. For instance, if the intent is to implement a processor, then the instructions in the processor have to be implemented using the data pad, and controlled by the control logic
     - **Instruction set architecture (ISA)**: The abstraction defined by a processor/CPU, like "intel inside" ads is the ISA been talked about by the promo. The ISA is all the details of how the ISA is actually implemented by the data path and the control logic. The meeting point between software and hardware - the hardware implementation simply fulfils the contract of realizing the instructions of architecture of the processor. ISA defines the machine code that a processor reads and acts upon as well as the word size, memory address modes, processor registers, and data type.
     - **System software(OS,compilers,etc)**: The software level doesn't care and doesn't know how the lower level  is implemented.
     - **Applications**

3. Layers of Abstraction
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149035805-ea0a6482-b8dd-4173-8b97-70871f714c49.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037703-c2c82505-ffb1-4e76-9879-d5974be3248c.png" alt="drawing" width="500"/>
</p>


# L01b: Hardware Resources
1. Hardware Continuum
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037423-72ef858f-0eae-4210-89db-c83eb98eddb4.png" alt="drawing" width="500"/>
</p>
The right answer is "No". Basically the hardware inside a computer system consists of processor, memory and I/O devices. The organization of these hardware elements within the computer system is not going to change tremendously 
2. Hardware Resources in a Computer System
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037935-16a0b851-7531-4024-9cf2-0b91f020362c.png" alt="drawing" width="500"/>
</p>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149038018-09f7c0f2-6531-4368-9e6e-6e802f2a1956.png" alt="drawing" width="500"/>
</p>

3. 
4. 
