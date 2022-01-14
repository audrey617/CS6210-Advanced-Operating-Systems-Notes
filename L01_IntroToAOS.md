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
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149078757-944e6fc1-6c6e-49bf-9ea5-4e44752c1411.png" alt="drawing" width="500"/>
</p>

# L01d: Managing the CPU and Memory
1. Managing the CPU and Memory: How is it Possible
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149273410-5625a2fd-e930-450b-a1b7-39523ff5008a.png" alt="drawing" width="500"/>
</p>
</p>
<ul>
  <li>The right answer is c. OS is multiplexing the CPU among the competing apps. You may have multiple cores but you don't have one core for every app, instead the OS do the scheduling </li>
</ul>

2.Catering to Resource Requirements
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149273686-539791ab-6300-4897-a4bc-df18bf11950f.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>The resource needs of an application includes time to compute on the CPU, the memory to hold instructions and data, and peripheral devices it might need to access during execution.</li>
  <li>Are the resource requirements of a program known to the OS before you launch it? yes and no. The OS knows enough about the program at the time of launch. So that from the disk, it can create a memory footprint for the application. For instance, on your favorite platform, when you click on an icon, what is going on is, a piece of the operating system called the operating system loader is reading in the disk resident image of that application, and creating a memory resident image of that application. so this is what called the memory footprint. The memory footprint of the program contains the code that needs to get executed on the processor, global data that it might be accessing, the stack that is needed when the program is makeing procedure calls and the heap which is the dynamic memory. Then that's created by the operating system loader at the point where you click on an icon. Once the program starts running,can the application ask for additional resources at run time? of course. This is exactly the service that is provided by the operating system. For example, if the application needs more memory, it can make an operating system call, and similarly if it needs to make a connection to access a web server it makes operating system call. The operating system then performs the service on the behalf of the application, and the application can then continue with whatever it needs to get done. That's how an OS caters to the resource requirement of applications. In other words, in addtion to catering to the initial requirement of an application at the point of launching it, the OS is also the broker through which a running application can request and get additional resources during its execution time </li>
</ul>

3. Managing the CPU and Memory: Precious Resources
 <p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149275513-7d555ed3-4d7c-4169-a994-4d78a8f03e23.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>The right answer is NO. OS is also a program and has to run on the CPU as we saw when we talked about how an OS deals with external interrupts. So it is going to need seom resources (CPU, memory cycles...). But a good OS will take the minimal amount of time and minimal amount of resources to do its thing. So the right answer is no, as otherwise you will not use an OS. It is sort of like when give to a charity, the first question you ask is, what percentage of the collection is used by the charity as administrative overhead? You don't want to give to a charity that spends more than a few percentage points of the collections on administrative overhead. Same thing with an OS. Most time the resources, CPU, memory and so on, is being used for running the applications. The OS gets in the way as a broker, only for arbitrating and providing the resources needed by an application safely and securely, and then gets out of the way as quickly as possible </li>
</ul>

4. Processor Related OS Abstractions
 <p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149276528-b20c806f-4db2-4633-997b-05f688bf8ed2.png" alt="drawing" width="500"/>
</p>
<ul>
   <li> A program is the static image (memory footprint) that is loaded into memory when you launch on application </li>
   <li> A process is a program in execution. Running the program on the CPU is that by scheduling the program (static) to run on the processor,the OS gives life to program and the process is the program in execution.Therefore, the process is the program plus the state of the running program. And yes, the state of the running program is not static. It is continuously evolving as the program executes.  </li>
</ul>

5.Difference Between Process and Thread
 <p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149277420-e9cde036-b4a2-484d-9f8b-e70405729a42.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>What is the difference between a process and a thread? An analogy will help here. Let's say there is a morning newspaper. This morning newspaper lying on the dining table is like a program in memory. No life. I come to the dining table and pick up the newspaper, and particularly the sports section and reading it. My starting to read the sports section of the newspaper is akin to the OS giving life to the program by starting execute it. So now there is one life in the program. That is one line of control, that is coursing through the core and data structures of the program. This is what is called the thread of execution through the program. So we have one thread of control that is coursing through the program just as I am reading a section of the newspaper. Now my wife comes along and picking up the business section and starts reading it. That's perfectly fine. Depending on our interest, I'm reading the sports section and my wife is reading the business section. Each is reading a different section of the same newspaper. Similarly, we can have multiple lives coursing through the program. Each blazing a completely different trail through the code and data structures of the program. Now each of this is a thread of control. Could there be a conflict between these threads? Sure. My wife and I may want to read the same section of the newspaper. That's a conflict. Similarly, the threads that are executing within the same program may wanna read or update the same data structures. There are the issues that the OS has to deal with, and this is what I meant when I said that the OS is orbiter for completing requests for resources. Now generalizing it, a program can have several threads of control, and each thread of control may be course through different sections of the program. And it could also be competiing for the same section of the program as well as the same data structure in order to manipulate. Thus, a process, is a program plus the state of all the threads that are executing within the program. Just as a single newspaper could be shared by me, my wife and possibly my children, in the similar manner, a program may have multiple life coursing through it and each is a thread of control, and the process is the program in execution, meaning it is the program plus the state of all the threads that are current executing within this program. </li>
</ul>

6. Memory-related OS abstractions
 <p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/149277535-bb0c3f46-ba84-4ed6-ae97-db0a29100b78.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>How is one program, let' say an email, protected from the misbehavior of another program, say the web browser. This is where memory related operating system abstractions come into play. In particular, the Operating system provides address space as an abstraction for each process that is distinct from one another. So the data and code that corresponds to a particular program is contained in a container which is called the address space. That's the abstraction provided by the operating system. And this address space abstraction of the OS is implemented by whatever hardware capabilities that the underlying processor architecture provides you. Processor and memory are the most precious resources. And what we've done is a quick review to understand the abstractions in the OS to manage these resources.</li>
</ul>

