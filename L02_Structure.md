# Lesson outline
- [OS Structure Overview](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02a-os-structure-overview)
- [The SPIN Approach](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02b-the-spin-approach)
- The Exokernel Approach
- The L3 Microkernel Approach
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150064173-e203bbc9-d523-4115-a3d1-2015d6bb625d.png" alt="drawing" width="500"/>
</p>

# L02a: OS Structure Overview
<h2>1. OS System Services: </h2>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034600-22a4c9d4-29ae-4d04-9644-fe6f5f209ab8.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034627-20bf078b-26de-4c81-a56a-8c00a34e09cf.png" alt="drawing" width="500"/>
</p>

<h2>3. OS Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034723-081d76a5-afb5-4a23-be20-94085e201d8e.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>What do we mean by OS structure? This term means the way the OS software is organized with respect to the applications that it serves and the underlying hardware that it manages. It sort of like the burger between the buns that is application of the top and technology or heart rate on the bottom and system software or OS is what connects the application to the underlying hardware.</li> 
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035082-b75e6eaa-09ad-42c8-905c-ee36a4cd4a77.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035154-c9950f69-f1af-4ae4-9c62-47aaf43e32b6.png" alt="drawing" width="500"/>
</p>

<h2>5. Goals of OS Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035261-73b64066-fbd5-47c8-b749-532ac1580781.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>Protection: protecting the user from system and the system from user, and also users from one another, also protecting an individual user from his/her own mistakes  </li> 
  <li>Performance: one of the key determinant of a good OS structure is how good is the performance of the OS, that is, what is the time taken to perform services on behalf of the application. A good operating system is one that provides the service that is needed by the application very quickly and gets out of the way </li> 
  <li>Flexibility(Extensibility): in fact one of the goals that we will be focusing a lot on in this course module is flexibility. Sometimes also called extensibility. Meaning that a service that is provided by OS is not one size fits all, but service is something that is adaptable to the requirements of the application  </li> 
  <li>Scalability: to ensure the performance of the operating system goes up as you add more hardware resources to the system. This is sort of an intuitive understanding but you want to make sure that the OS delivers on this intuitive understanding that when you increase the hardware resources the performance also goes up. That's what is meant by scalability </li> 
  <li>Agility: it turns out that both the needs of the application may change of the lifetime of an application, also the resources that are available for the OS to manage and give to the application may change over time. Agility of the OS refers to how quickly the OS adapts itself to changes either in the application needs or the resource availablity from the underlying hardware</li> 
  <li>Responsiveness: How quickly the OS reacts to external events, and this is particularly important for applications  that are interactive in nature, Imagine playing video game -> click mouse -> want to see actions immediately on the screen </li> 
</ul>

<h2>6. Commercial OS</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150036733-47c3e28c-d031-4afc-9e8d-de77cee0b8fc.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>Are all the goals simultaneously achievable in a given operaing system? In the first glance it would seem that some of the goals conflict with one another. For example, it might seem that achieve performance we may have to sacrifice protection and/or flexibility </li> 
</ul>

<h2>7. Monolithic Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150037356-420d6c7b-d7a0-46eb-be9d-b6a99d7528af.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>Now let's talk about different approaches to operating system structuring. The first structure that I will introduce to you is what we will call as a monolithic structure.   <li> You have the hardware at the bottom which is managed by the operating system and hardware includes,of course, the CPU, memory, peripheral devices such as the network and storage and so on.</li>   <li> And there are applications at the top. And each of these applications is in its own hardware address space. What that means is that every application is protected from one another because the hardware ensures that the address space occupied by one application is different from the other applications and that is the first level of protection that you get between the applications themselves.</li>   <li> And all the services that applications expect from the operating system are contained in this blob and that might include file system and network access, scheduling these applications on the available CPU, virtual memory management, and access to other peripheral devices. The OS itself of course is a program providing entry points for the applications for the services that are expected by the applications. And code and the data structure of the operating system is contained in its own hardware address space. What that means is that the operating system is protected from the applications and vise versa. So even if an application were to do anything in terms of misbehavior, either maliciously or unintentionally because they are in there own address spaces and the operating system is in its own hardware address space. Malfunctioning of an application does not affect the integrity of the operating system services. That is, when an application needs any system service, we switch from the hardware address space that is representing this particular application, into the hardware address space of the operating system. And execute the system code that provides the service that that is expected by the application. For example, accessing the file from the hard disk, or dynamic allocation of more memory that an application may want, or sending a message on the network. All of these things are done within the confines of the address space of the operating system itself. Note that all of the services expected of the operating system, file system, memory management, CPU scheduling, network and so on, are all contained in this one big blob. And that is the reason it's also sometimes referred to as the monolithic structure of an operating system.</li></li> 
</ul>

<h2>8. DOS-like Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038025-a9894454-ade2-437b-9477-2cb9b794911e.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>Some of you may remember Microsoft's first entry in the world of PCs, with their operating system called DOS, or disc operating system, and the structure of DOS looks as shown here.  And at first glance, at least visually, You might think that this structure is very similar to what I showed you as a monolithic structure before. What is the difference you see in this structure? </li> 
</ul>

<h2>9. DOS-like Structure Pros and Cons</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038068-36f1bfda-ede2-45dc-a21b-82092d8e045a.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038086-61ec8c44-b9cf-4cc7-be4d-03d74aa474fe.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>You have noticed visually that the key difference was,the red line was replaced by a dotted line, separating the application from the operating system. And what you get out of that is performance.  Access to system services are going to be like a procedure call, and what is lost in the DOS-like structure is the fact that you don't have protection of the operating system from the application. An errant application can corrupt the operating system.</li> 
</ul>

<h2>10. DOS-like Structure (cont)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150066994-b09789d9-021f-4d63-b3a9-cf08a89e4463.png" alt="drawing" width="500"/>
</p>
<ul>
<li> So in the DOS-like structure, the main difference from the monolithic structure is that the red line separating the application from the operating system is now replaced by a dotted line. What that means, <strong>the main difference is there is no hard separation between the address space of the application and the address space of the operating system.</strong> </li>
<li> The good news is an application can access all the operating system services very quickly. As they would call any procedures that they may execute within their own application with the same speed. At memory speeds, an application can make calls into the operating system and get system services. That's the good news. </li>
<li>But the bad news is that there is no protection of the operating system from an errant application. So, the integrity of the operating system can be compromised by a runaway application, either maliciously or unintentionally corrupting the data structures that are in the operating system.</li>
<li> Now you may wonder why DOS Chose this particular structure. Well, at least in the early days of PC, it was thought that a personal computer, as the name suggests, is a platform for a single user and, more importantly, the vision was, there will be exactly one app that is running at a time. Not even multitasking. <strong> So performance and simplicity was the key and protection was not primary concern in the DOS-like structure.</strong> And that you can get good performance comes from the simple observation that there is no hard separation between the application and the operating system. The operating system is not living in its own address space. The application and the operating system are in the same address space. And therefore, making a system call by an application is going to happen as quickly as the application would call a procedure which the application developer wrote himself or herself.</li>
</ul>

<h2>11. Loss of Protection in DOS like Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150067607-96991160-d03f-4df9-a0c9-85c85c650ce3.png" alt="drawing" width="500"/>
</p>
<ul><li>
But this loss of protection with the Dos-like structure is simply unacceptable for a general purpose operating system today.</li>
   <li>On the other hand, the monolithic structure gives the protection that is so important. At the same time, what it strives to do is that it reduces the potential for performance loss by consolidating all the services in one big monolithic structure. That is, even though an application has to go from its address space into the operating system's address space in order to get some service, it is usually the case that the operating system has several components and they have to talk to one another in order to provide the service that an application wants. Think about the file system, for instance, you make a call to the file system to open a file and the file system then may have to call the storage module in order to find out where exactly a file is residing. And it may have to contact The memory manager module to see where it wants to bring in the file that you want to open and see the content of. So in this sense there's infraction that's going to go on under the cover inside the operating system between components of the operating system, in order to satisfy a single service call from an application. So this monolithic structure insures that even though we have to go from an application into the operating system's address space, once you're inside the operating system's address space, then potential for performance loss is avoided by the consolidation of all the components that comprise the operating system.</li>
   <li> But what is lost in the monolithic structure that is the ability to customize the operating system service for different applications. This model of one size fits all, so far the system service is concerned, with the monolithic structure short the opportunity for customizing the operating service for the needs of different applications.</li>
   <li> Now, you may wonder why do we need to customize the operating system service for different applications? Why not one size fits all? Why is there an issue? If you look at a couple of examples, the need for customization will become fairly obvious. For example, Interactive video games. The requirement of applications, that are providing a video game experience for the user. Or consider another application, which is computing all the prime numbers. You can immediately see that the operating system needs for these two classes of applications are perhaps very different. On the one hand, for the little kid who is playing a video game, the key determinant of a good operating system would be responsiveness — How quickly the operating system is responding to his moves when he plays his video game. On the other hand, for the programmer that wrote this prime number computing application, the key determinant of performance is going to be sustained CPU time that's available for crunching this application.   
</li></ul>

<h2>12. Opportunities for Customization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150068145-8d61e089-3c96-4cb0-a80f-1f4442c13866.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>Let's explore the opportunities for customization with a very specific example and the example is memory management, in particular, how an operating system handles page faults.</li> 
   <li>Let's say that this thread executing on the processor incurs a page fault. The first thing that the operating system has to do in order to service this page fault will be to find a free page frame to host the missing page for this particular thread. And once it allocates a free page frame, then the operating system is going to initiate the disk I/O to move the page from virtual memory into the free page frame that has been identified for hosting the missing page from this particular thread. Once the I/O is complete and the missing page for this thread has been brought from storage into the free page frame, the operating system is going to update the page table for this thread or process, establishing the mapping between the missing virtual page and the page frame that had been allocated for hosting that missing page. Once the page table has been updated, then we can resume the process so that it can continue where it left off at the point of the page fault. One thing that I haven't told you the sequence of actions that the operating system takes, is that every so often, the operating system runs a page replacement algorithm to free up some page frames. And readiness for allocating the frame to a page fault that a process may incur. Just as an airline overbooks its seats in the hope that some passengers won't show up, the operating system is also overcommitting its available physical memory hoping that not all of the pages of a particular process which is in the memory footprint of the process will actually be needed by the process during its execution. But how does the operating system know what the memory access pattern of a particular process is going to be in making this decision? The short answer is it does not. So whatever the operating system chooses as an algorithm to implement page replacement. It may not always be the most appropriate one for some class of applications. So here is an opportunity for customization depending on the nature of the application. Knowing some details about the nature of the application, it might be possible to customize the way page replacement algorithm is handled by the operating system.</li>
   <li>Similar opportunities for customization exist in the way the operating system schedules processes on the processor and reacts to external events such as interrupts and so on.</li>
</ul>

<h2>13. Microkernel based OS Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150069251-b48cac8a-e9fd-4775-920f-79fe841f5b21.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>There is a need for customization and the opportunity for customization is what spurred operating systems designers to think of a structure of the operating system that would allow customization of the services and gave birth to the idea of microkernel-based operating system.</li>
   <li> As before, each of the applications is in its own hardware address space, the microkernel runs in a privileged mode of the architecture, and provides simple abstractions such as threads, address space, and inter-process communication. In other words, small number of mechanisms are supported by the microkernel. The keyword is mechanisms, there are no policies ingrained in the microkernel. Only mechanisms for accessing hardware resources.</li>
   <li>The operating system services, such as virtual memory management, CPU scheduling, file system, and so on, that implemented as servers on top of the microkernel. So in other words, these system services execute with a same privilege as the applications themselves. Each of the system service is in its own address space and it is protected from one another and protected from the application and the microkernel, being below this red line, is running in privileged mode, it is protected from all of the applications as well as the system services. So in other words, we no longer have that monolithic structure that we had before. Instead, each operating system service is in its own hardware address space. In principle, there is no distinction between regular applications and the system services that are executing a server processes on top of the microkernel.</li>
   <li>Thus, we have very strong protection among the applications, between the applications and system services, among the system services and between application system services and the microkernel.</li>
   <li>Now, the structure what it entails is that you need the microkernel to provide inter-process communication so that the applications can request system services by contacting the servers and the servers need to talk to one another as well. And in order for them to talk to one another they need inter-process communication as well.</li>
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150069291-7a668dc6-9905-412f-9694-e89e90f31896.png" alt="drawing" width="500"/>
</p>

 <ul><li> So what have we gained by the structure? What we have gained by the structure is extensibility. Because these OS services are implemented as service processes, we can have replicated server processes with different characteristics. For instance, this application (App1) may choose to use this particular file system (file system1). Another application (App2) may choose a different file system (file system2). No longer do we have that one size fits all characterization of the monolithic kernel. And this is the biggest draw for the microkernel based design that it is easy to extend the services that are provided with the operating system to customize the services depending on the needs of the application. This all sounds good, but is there a catch?</li></ul>

<h2>14. Downside to Microkernel</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150070170-6abc6da5-aeaa-4388-9fc5-ce44098bad3c.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>Is there a downside to the microkernel-based approach? Well, there is. There is a potential for performance loss.</li>
   <li>Now consider this monolithic structure. Let's say this application makes a call to the file system to open up a file. The application slips through this red line into the hardware address space of the operating system. It runs in privileged mode because the operating system may have to do certain things that are privileged, and therefore, the hardware architecture of the CPU usually allows a privileged mode for execution of the operating system code. So now the app is now inside the operating system in a privileged mode with one instruction usually, called a trap instruction. For example, a system call results in a trap into the operating system. And once inside the operating system, all the work that needs to be done in order to satisfy the file system call that the app made. For instance, contacting the storage manager, contacting the memory manager and so on. All of that, are available as components within this blob, which means that those components can be accessed at the speed of normal procedure call in order to handle the original request from this application.</li>
   <li>On the other hand, if you look at a microkernel based structure, the application has to make an IPC call in order to contact the service, which is, in this case, a file system service let's say, which means that the application has to go through the microkernel, making the IPC call. Going up to the file system and the file system does the work, makes another IPC call in order to deliver the results of that system service back up to the application. So the minimum traversal so that you can see is going from the application of the microkernel, microkernel to the file system, and back into the microkernel and back up to the application. Potentially, there may be many more calls that may happen among servers that are sitting above the microkernel. Because the file system may have to contact the storage manager and the file system may have to contact the memory manager. All of those are server processes living above the microkernel and all of them require IPC for talking to one another. So what that means is that with this structure, there is a potential that we may have to switch between the address spaces of the application and many services that are living on top of the microkernel, whereas in the case of the monolithic structure, there is only two address space switches, one to go from the application into the operating system, and the other to return back to the application. Whereas in a microkernel based design, there could potentially be several address space switches depending on the number of servers that need to be contacted in order to satisfy one system call that may be emanating from the application.
</li></ul>

<h2>15. Why Performance Loss</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150070725-9b15b334-96ad-44d0-914c-d5810532cdf0.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>Why do we have this potential for performance loss, with the microkernel based design? Mainly because of the <strong> border crossings, that is, going across hardware address spaces,</strong> can be quite expensive.</li>
   <li>First there is this explicit cost of switching the address space, from one hardware address space to another hardware address space. That is the explicit cost.</li>
   <li>And in addition to the explicit cost of going across address spaces, there are implicit costs involved in this border crossing. And that comes about because of change in locality. We're going from one hardware address space to a different address space, and that changes the locality of execution of the processor. And that means that the memory hierarchy, the caches in particular, close to the processor, may not have the contents that are needed for executing the code and accessing the data structures of a particular server, different from the application. A change in locality is another important determinant of performance and it can adversely effect the performance.</li>
   <li>And also, when we are going across address spaces to ensure the integrity of the system, either the microkernel, or the server that is living on top of the microkernel, there may be a need to copy from user space to system space. And those kind of copying of data from the application's memory space into the microkernel and back out to a server process.</li>
   <li>All of those can result in affecting the performance of the operating system. Whereas they're in a monolithic structure, since all the components are contained within the same address space, it is much easier for sharing data without copying. And that's one of the biggest potential sources of performance loss when we have this microkernel based structure.</li>
</ul>

<h2>16. Features of Various OS</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150071147-919f6783-2c47-4b31-abb0-fc180901d3b5.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150071219-42b49716-3249-407e-99b9-130ec558c35d.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>A Monolithic structure definitely gives you protection, no questions about that. And we also argued that it's performant because of the fact that border crossings are minimized and loss of locality is minimized. And, and sources of copying overhead are minimized. All of that add up to giving good performance for the Monolithic structure. On the other hand It's not easily extensible. Any change to the operating system would require rebuilding the monolithic structure with the changed characteristic of the system service. So, one size fits all is what you get with a monolithic structure.</li>
   <li>A DOS-like structure is performant because there is no separation between the application and the operating system and, therefore, an application can execute system services at the same speed as it would execute a procedure call that is part of that application itself. And it's also easily extensible because you can build new versions of system service. To cater to the needs of specific applications. But on the other hand, it fails on the safety attribute. Because there is no boundary separating the kernel from the user space. </li>
   <li>A micro-kernel based operating system also pays attention to protection because it makes sure that the applications and the servers are in distinct hardware address spaces separated from the microkernel itself and it is also easily extensible because you can have different servers that provide the same service but differently to cater to the needs of the application. But it may have performance flaws because of the need for so many border crossing that might be needed to go between applications and the server processes. Having said that I want to give a note of caution, on the surface it may appear that the microkernel based approach may not be performant because of the potential for frequent border crossings. I'll have a surprise for you on this aspect when we discuss the L3 microkernel later on in this course module where it is shown that a microkernel can be made performant by careful implementation, that's the key. I'll leave you with that thought, but we'll come back to a micro kernal base design using L3 later on.</li>
</ul>

<h2>17. What do we Want</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150071850-d086e764-924b-4366-a4ce-9e7653ba0a44.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Here's another way to visualize the relationship between these different attributes that I mentioned of performance, extensibility and protection or safety.</li>
   <li>A DOS-like structure that does not pay attention to safety or protection, needs the two attributes of performance and extensibility.</li>
   <li>A microkernel-based approach achieves protection and extensibility, but may have issues with respect to performance</li>
   <li>A monolithic structure may yield good performance, has protection. But, it is not easily extensible. </li>
   <li>Now what do we want? Of course we want all three of these characteristics in an operating system structure. But can we have all of these three characteristics in an operating system? In other words, what we would like the operating system structure to be such that we get to this center of the triangle that caters to all three attributes — Performance, Extensibility, and Protection. And the research ideas that we will study in this course module is looking at ways to get to the center of the triangle so that all three attributes can be present in the structure of the operating system. We will resume the course module with research approaches that have been proposed than that we will cover in this course module that help us get to the middle of the triangle.</li>
</ul>

# L02b: The SPIN Approach
<h2>1. The SPIN Approach Introduction</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150072482-07961bc0-df5c-4ce2-90be-8a9fd4acd5a7.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150072549-0b743c5d-4255-4d7b-af8f-ee3e04210f25.png" alt="drawing" width="500"/>
</p>

<ul><li>
So now we set the stage for discussing the spin and the exokernel approaches to achieving extensibility of the operating system without losing out on protection or performance. Both these approaches start with two premises: 
   <ul><li>The first premise, is that microkernel-based design compromises on performance due to frequent border crossings</li> 
   <li> The second premise is that monolithic design does not lend itself to extensibility. </li></ul>
   Because of the starting premises of spin and exokernal. Both these approaches have certain commonality in what they strive to do, although the path taken by these two approaches are very different.
</li></ul>


<h2>2. What are we Shooting for in OS Structure</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150072666-46fe76de-23e0-416b-b4aa-0a77ee303c95.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>So, let's revisit what we are shooting for in the structure of an operating system. </li>
   <li>We want the operating system strucure to be thin like a microkernel, that is, only mechanisms should be in the kernel, and no policies should be ingrained in the kernel itself.</li>
   <li>The structure should allow fine-grained access to system resources without border crossing as much as possible like the DOS-like structure. That is, it should have a structure similar to what we saw in DOS.</li>
   <li>And it should be flexible, meaning resource management should be easily morphed to suit the needs of the application without sacrificing protection and performance. So, the flexibility part of it should be similar to what we can get from microkernel based approach, but at the same time we want the protection and the performance we can get with the monolithic approach.</li>
   <li>So in other words, in a nutshell, what we want in the operating structure is performance, protection, and flexibility.</li>
</ul>

<h2>3. Approaches to Extensibility</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150073187-1e6849fa-c52f-498d-83d9-9c287149e778.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>We'll now turn our attention to the issue of extensibility and some of the approaches that have been taken to achieve this goal. Historically I should mention that there was interest in extensibility at least as far back as 1981 with a system that was developed at CMU called the Hydra operating system.</li>
   <li>The Hydra operating system provided kernel mechanisms for resource allocation. 
      <ul>
         <li>The key word is mechanisms, not policies, just mechanisms for resource allocation in the kernel.</li>
         <li>And it had a way of providing access to resources, using a capability based approach. The notion of a capability has a special conotation in the operating system, that is, an entity that can be passed from one to the other, cannot be forged, and can be verified. All of the things that you want in order to make sure that the system integrity is not compromised, as enshrined in this abstract notion of capability. And as originally envisioned, capability was a heavyweight mechanism in terms of implementing it efficiently in an operating system. 
         <li>And because capability is a heavyweight mechanism, in the hydra operating system, resource managers were built as coarse-grained objects in order to reduce the border crossing overhead, because border crossing in the hydra system would mean that you have to pass capability from one object to another. And validate the capability for entering a particular object and so on, and for that reason, hydra used coarse-grained objects to implement resource managers. That way, they can reduce the border crossing overhead. And implementing resource managers as coarse-grained objects also means that it limits the opportunities for customization and extensibility. <strong>In other word, the coarser you make these object, the less opportunity you have for customizing the services. Which is exactly the strike against monolithic kernel as well.</strong></li></ul>
   <li>So, while in principle, hydra had all the right ideas of providing minimal mechanisms in the kernel, and having the resource managers implement policies because the fundamental mechanism for accessing the resources was through this capability, which is a heavyweight abstract notion to implement efficiently. In practice, hydra did not fully achieve its goal of extensibility.</li>
   <li>One of the most well-known extensible operating system of the early 90s was the Mach operating system from CMU. It was microkernel-based, providing very limited mechanisms in the microkernel. And implementing all the services that you expect from an operating system as server processes that run as normal user level processes above the kernel. And clearly, with this microkernel based approach, Mach achieved its goal of extensibility. So it focused on extensiblity and portability. The keyword is portability. And therein lies the rub - Performance took a backseat. Because Mach was very much focused on making the operating system portable across different architectures, in addition to paying attention to extensibility. And this, unfortunately, gave a bad press for microkernel based design. Because it focused on these twin goals of portability and extensibility, allowing performance to take a backseat. And since operating systems are generally so focused on performance. This design choice in Mach of supporting portability gave microkernel-based design a bad press.</li>
   <li>But later on, when we look at L3 approach to microkernel-based design, we will revisit the right way to build a microkernel-based design. But in this lesson, let's focus on Spin Approach to Extensibility.</li></ul>
  
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150073790-8d7ce279-611f-4ff7-b3d8-6a6ae8c68064.png" alt="drawing" width="500"/>
</p>
   
<ul><li> 
So the key idea in spin is to <strong> co-locate a minimal kernel with its extension in the same hardware address space and avoid the border crossing between the components of the kernel and the extensions of the kernels </strong> that are containing the specific services that the applications need. And this co-location, also means that we avoid the border crossing which he said is one of the biggest potentials for losing out on performance.</li>
   <li> But, if you're going to co-locate the kernel and extensions in the same hardware address space, isn't that compromising on protection? Wasn't that the strike against the DOS-like structure that we talked about earlier? Well, the approach that Spin took was to rely on the characteristics of a strongly typed programming language(<i> Based on wiki, "In a strongly typed language each data area will have a distinct type and each process will state its communication requirements in terms of these types."</i>), so that the compiler can enforce the modularity that we need in order to give guarantees about the protection. So, by using a strongly typed language, in the case of Spin, they used modula-3 more on that in a minute. The kernel is able to provide well-defined interfaces. All of you may be quite familiar with declaring function prototypes in a header file and having the actual implementation of the procedures in other files. In a large software project, this is the same idea that is now taken to the design of the operating system itself. After all, operating system is also a piece of software, a complex piece of software, and why not use a strongly typed language as the basis for building the operating system. That's the idea in the spin approach.</li>
   <li> Now, what you get when you use a strongly typed language, is that you cannot cheat. For instance, in a language like C, you can type cast pointers so that a given data structure can be viewed completely differently, depending on what you need to get done at the moment. That's not possible with a strongly typed language. Data abstractions provided by the programming language such as an object serve as containers for logical protection domains. That is, we are no longer reliant on hardware address spaces to provide the protection between different services and the kernel. As I mentioned the kernel provides only the interfaces and these logical protection domains actually implement the functionality that is enshrined in those interface functions. And there can be several implementations of the interface functions. And that's where the flexability comes in. </li>
   <li> Applications can dynamically bind different implementations of the same interfaith functions. And that's how we get different instanciations of specific system components. Getting you the flexibility that you want in constructing an operating system. Because we have co-located the kernel and the extension in the same hardware address space, we are making the extensions as cheap as a procedure call.</li>
   <li>So in a nutshell, what we've accomplished with a Spin approach to extensibility as we are writing on the characteristics of a strongly typed programming language, that enforces strong typing and therefore allows the operating system designer to implement logical protection domains instead of relying on hardware address spaces. And consequently we're making extensions as cheap as procedure calls.</li>
</ul>

<h2>4. Logical Protection Domains</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150074539-ee059bd3-3901-4c1c-b916-db11f76bb9cc.png" alt="drawing" width="500"/>
</p>

<ul>
   <li> Modula-3 is a strongly typed language with built-in safety and encapsulation mechanisms.</li>
   <ul><li>It does automatic management of memory. That is, since it does automatic storage management, there are no memory leaks. Modula-3 supports a data abstraction called an object with well defined entry points. Only the entry points are known outside the object, not the implementation of the code for that entry point, or the data structures that are contained within an object. And therefore there's no cheating possible as you can do with a language like C.</li>
      <li>And modula-3 allows exposing the externally visible methods inside an object using generic interfaces. And it also supports the notion of threads that execute in the context of the object, and it allows raising exceptions, for example, when there is a memory access violation.</li>
      <li>All of the features that I mentioned here in a nutshell allows implementing system services as an object with well defined entry points.</li>
      <li>This modula-3 allows the creation of logical protection domains. What you can do from outside the object is what the entry point methods that are inside the object will let you do and no more. In other words, we are getting the safety property of a monolithic kernel without having to put system code in a separate hardware address space. So in other words the logical protection domains give you both protection and performance, the two things that we strive for.</li>
      <li>Now, what about flexibility? Well, the genetic interface mechanism allows you to have multiple instances of the same service. And a given application may be able to exploit the different instances of services that are available that cater to the same generic interface, and that's the way you can get flexibility as well.</li></ul>
   <li>And objects that implement specific services can be the desired granularity of the system designer. It can be fine-grained, or it can be a collection. You can think of individual hardware resources as fine-grained object. For example, a page frame and what you can do with a particular page frame. You can have interfaces that provide a certain functionality. That can be what an object is. For example a page allocation module can be on object. And it can also make a collection of interfaces into an object. For example, an entire virtual memory subsystem can be an object that is hierarchically composed of page allocation module, and within that, you may have hardware resources defined as objects as well. And all of these objects, whether it is at the coarse level of a collection of interfaces, or individual interface that is a component of this collection, or specific hardware resources, all of those are accessible via capabilities. </li>
   <li>Now the word capability may give you jitters, because I just now said that capabilities traditionally in the operating system parlance signifies a heavyweight mechanism. But because we are dealing with a strongly typed language, capabilities to objects can be supported as pointers. Or in other words, the programming language supported pointers can serve as capabilities to the objects. So now, with this idea access to the resources, that is entry point functions within an object that is representing a specific resource, is provided via capabilities that are simply language supported pointers. And because they are language supported pointers, these capabilities that we are talking about here, are much cheaper compared to real capabilities as was used in the hydra operating system.</li>
</ul>

<h2>5. Pointers</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150724161-c5e14444-92ec-4ddc-b848-e2ff6565e42d.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150724267-08b5b88f-469f-4320-ac04-338b4428c206.png" alt="drawing" width="500"/>
</p>
<ul>
<li>The right answer is modula -3 pointers are type-specific. That is, pointers in modula-3 cannot be forged, there's no way to subvert the protection mechanism that is built into the language. So if I have a data structure defined In Modula-3. And if I have pointer to the data structure, the only way you can use that pointer, is as a pointer to that type of data structure. You cannot take a data structure and cast it to appear like something else, this is something that we as C programmers, maybe very used to doing, but that's not something that is possible in Modula-3. And that is what allows us to implement logical protection domains in Modula-3 Using objects and capabilities to objects as pointers supported by the programming language.</li>
</ul>

<h2>6. Spin Mechanisms for Protection Domains</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150724711-cf4f23ec-2293-49ca-9e8f-1f923826e970.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>There are three mechanisms in SPIN to create protection domains, and use them.<ul>
      <li>Create: The first one is the create call that allows creating a logical protection domain. And this mechanism in SPIN allows initiating an object file with the contents and export the names that are contained as entry point methods inside the object to be visible outside. That's what this create call, supported by SPIN, provides to a service creator. For example, if I'm creating a memory management service. I can write the entry point functions in my memory management service and export the names using this create mechanism that's available in SPIN. </li>
      <li>Resolve: The second mechanism in SPIN is resolving names. If one protection domain wants to use the names that is there in another protection domain. The way we can accomplish that is by using this resolve primitive that's available in SPIN. Resolve is very similar to linking two separately compiled files together that a compiler does routinely. So, you may be very familiar with the compilation process where you may separately compile files and once you have done the separate compilation of the files, then you go through a link phase of the compiler where the linker resolves the names that are being used by one object file with the names that are defined in another object file. That's the same thing that the resolve mechanism of SPIN does is, it resolves the names that are being used in source, which is a logical protection domain, and the target, which is another logical protection domain. As the result of this resolve step, the source logical protection domain and the target logical prediction domain are dynamically linked or bound together. And once bound together, accessing methods that are inside this target protection domain happens at memory speeds, meaning it is as efficient as a procedure call, once this resolve step has happened. </li>
      <li>Combine: As I mention before, to reduce the proliferation of small logical protection domains you may want to combine protection domains to create an aggregate larger protection domain and SPIN provides a mechanism for that, which is the combined mechanism. Once the names in a source and target protection domain have been resolved, they can be combined to create an aggregate domain. And the aggregate logical protection domain will have entry points, which is the union of the entry points that were exported as names from the source and the target or any number of such domains that have been combined together to create an aggregate domain. So this combined primitive in SPIN is mainly useful as a software engineering management tool to combat the proliferation of many small domains.</li></ul> </li>
   <li>So, once again, the road map for creating services is, write your code as a Modula-3 program with well defined entry points. And using the SPIN mechanism of create, you can instantiate a service and export the names that are available in that service. And if another logical protection domain wants to use the names that are exported, it can do so by using the SPIN mechanism resolve that causes the dynamic binding of the source and target logical protection domains. And finally, the combined primitive allows aggregation of logical protection domains to create an aggregate domain, that's the union of all the entry points that are available in the component logical protection domains. This is it. This is the secret sauce in SPIN to get protection and performance while allowing flexibility. Everything hinges on the strongly-typed nature of the programming language that is being used for implementing the operating system. That is, the language allows compile time checking, and run time enforcement of the logical protection domains. That's the key to the success of this approach to providing flexibility, protection, and performance, all in one bag
   </li>
</ul>
