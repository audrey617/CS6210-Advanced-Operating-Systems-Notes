# Lesson outline
- Abstractions
- Hardware Resources
- OS Functionality
- OS Abstractions (Managing the CPU and Memory)
- By lesson end, ready to discuss structure of OS

# L01a: Principle of Abstraction
1. Abstraction: A well understood interface that hides all details within a subsystem
   - An abstraction example (we don't know how it is implemented and we don't care) - door knob, Instruction set architecture, AND logic gate, Transistor, Int data type
   - Not an abstraction - Nums of pins coming out of chips, Exact location of the base pads in a baseball field
2. Digging Deeper Into the Power of Abstractions
   - A series of abstractions sit between Google Earth and Electron holes
   - From lowest level of the hierarchy to the highest
     - **Electron and holes**
     - **Transistors**: transistor reigns in the randomness in the movement of electrons and holes, and gives us an abstraction of a switching device, which is the transistor
     - **Logic gates**: And-gate, not-gate & or-gate where we implement boolean logic using transistor as a switching device
     - **Sequential and combination logic elements**: implemented using logic gates such as and, not and or. Logic elements are then organized into a data pack, depending on what hardware circuitry or functionality we want to implement
     - **Machine Organization (datapath + control)**: The datapath (the arithmetic logic unit, the set of registers, and the CPU's internal bus(es) that allow data to flow between them) establishes the communication paths we need between these combinational and sequential logic elements. To realize whatever is the hardware device that we are trying to design, and control part of it is a finite shaped machine that controls the datapath and implements the repertoire of the hardware device that we are trying to realize. For instance, if the intent is to implement a processor, then the instructions in the processor have to be implemented using the datapath, and controlled by the control logic
     - **Instruction set architecture (ISA)**: The abstraction defined by a processor/CPU, like "intel inside" ads is the ISA been talked about by the promo. The ISA is all the details of how the ISA is actually implemented by the datapath and the control logic. The meeting point between software and hardware - the hardware implementation simply fulfils the contract of realizing the instructions of architecture of the processor. ISA defines the machine code that a processor reads and acts upon as well as the word size, memory address modes, processor registers, and data type
     - **System software(OS,compilers,etc)**: The software level doesn't care and doesn't know how the lower levels are implemented
     - **Applications**

3. Layers of Abstraction
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149048291-e2a0946c-9ef5-4c39-b930-6b201c24d87c.png" alt="drawing" width="500"/>
</p>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037703-c2c82505-ffb1-4e76-9879-d5974be3248c.png" alt="drawing" width="500"/>
</p>


# L01b: Hardware Resources
1. Hardware Continuum
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037423-72ef858f-0eae-4210-89db-c83eb98eddb4.png" alt="drawing" width="500"/>
</p>
<p> The right answer is "Flase". Basically the hardware inside a computer system consists of processor, memory and I/O devices. The organization of these hardware elements within the computer system is not going to change tremendously </p>

2. Hardware Resources in a Computer System & Organization With IO Bus
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149037935-16a0b851-7531-4024-9cf2-0b91f020362c.png" alt="drawing" width="500"/>
</p>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149038018-09f7c0f2-6531-4368-9e6e-6e802f2a1956.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>*contr - controller. Depends on the sophistication of the device, and the speed of the device we are talking about, the controllers may have different capabilities. For example, the network controller may have the ability to move the data directly from the memory into the network or from the network into the memory via direct memory access. Others like keyboard may support PIO</li>
  <li>Bus: conduit for communication, serves the purpose of connecting the CPU to the memory & I/O devices </li>
  <li> I/O bus VS System bus: The System bus connects the CPU and the main memory and the I/O bus connects the CPU to the peripheral devices. Usually the system bus is a synchronous communication device between CPU and memory, and the I/O bus is primarily intended for devices to communicate with the CPU. In this model, the system bus is typically much higher bandwidth so that the I/O bus never really disrupts CPU and memory communication. The intent is that the individual needs of each device in terms of the communication bandwidth that they may need is less than communication bandwidth that may be available for the CPU to communicate with the memory. In other words, the system bus has a communication bandwidth that is much more than the communication bandwidth that is available in the I/O bus. So the system bus is a high speed bus and it connects via a bridge to the I/O bus </li>   
   <li> The bridge itself could be a specialized I/O processer for scheduling the devices that need to communicate with the memory(DMA device)/CPU (slow speed device). The role of the bridge is like a processor controlling who has access to this I/O bus among the set of devices that may be competing at the same time for attention by the CPU, and communicating the intent of these I/O devices either directly with memory or via CPU. The I/O bus is typically the lower speed. The system bus is typically the higher speed since it needs to cater to all the clients that may want to access the memory either from the CPU or from the devices coming from bridge</li>
   <li> Other high speed devices like frame buffer may also hang off of the system bus due to the need for refreshing the screen in a rapid manner from the memory</li>
   <li> In a nutshell, if you look at the internal organization of a computer system, there are going to be 1+ CPU (single-core/multi-core/parallel), a bunch of memory, whole number of I/O devices with device controllers that communicate with the CPU or memory, a conduit system bus and I/O bus for connecting controllers to memory/CPU. There is no difference for internal organization regardless of the platform specifics </li>
</ul>

3. In summary, the organization of a computer system is consistent enough that many of the key operating system concepts applied all of them regardless of size and capacity. On the other hand, these differences should not be ignored and in advance in hardware have helped driven innovation in operating system so we can get the most out of capabilities. 

# L01c: OS Functionality
1. OS Functionalities
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149077162-21afe77f-7d7b-4e45-bbdf-88bc89a50cdb.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>The right answers: </li>
    <ul>
       <li> OS is a resource manager </li>
       <li> OS provides a consistent interface to the hardware resources </li>
       <li> OS schedule applications on the CPU </li>
    </ul>
  </li>
</ul>


2. What is an OS
   - OS protected access to hardware resources
   - OS arbitrate among competing requests
   - OS provides a well-defined APIs for accessing the hardware resources that are managed by the operating system, and these resources are provided as operating system services through this well-defined interfaces. Applications may make a request to the operating system for such hardware resources through this well-defined API interface. And the application with the response from the OS gets the services from the OS via API calls
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149077734-f0b9f896-0878-43ab-ba63-b4661b99abcd.png" alt="drawing" width="500"/>
</p>

3. Hardware software interaction
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149078514-9ec0f26f-f821-49b3-809e-cfe1f9422ba1.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>The right answer: </li>
    <ul>
       <li> it results in a CPU interrupt </li>
    </ul>
  </li>
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149078757-944e6fc1-6c6e-49bf-9ea5-4e44752c1411.png" alt="drawing" width="500"/>
</p>

# L01d: Managing the CPU and Memory
<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="500"/>
</p>
