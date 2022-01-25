# Lesson outline
- [OS Structure Overview](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02a-os-structure-overview)
- [The SPIN Approach](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02b-the-spin-approach)
- [The Exokernel Approach](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02c-the-exokernel-approach)
- [The L3 Microkernel Approach](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L02_Structure.md#l02d-the-l3-microkernel-approach)
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
   <li>On the other hand, the monolithic structure gives the protection that is so important. At the same time, what it strives to do is that it reduces the potential for performance loss by consolidating all the services in one big monolithic structure. That is, even though an application has to go from its address space into the operating system's address space in order to get some service, it is usually the case that the operating system has several components and they have to talk to one another in order to provide the service that an application wants. Think about the file system, for instance, you make a call to the file system to open a file and the file system then may have to call the storage module in order to find out where exactly a file is residing. And it may have to contact The memory manager module to see where it wants to bring in the file that you want to open and see the content of. So in this sense there's infraction that's going to go on under the cover inside the operating system between components of the operating system, in order to satisfy a single service call from an application. <strong>So this monolithic structure insures that even though we have to go from an application into the operating system's address space (compared to DOS), once you're inside the operating system's address space, then potential for performance loss is avoided by the consolidation of all the components that comprise the operating system.</strong></li>
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
   <li> But, if you're going to co-locate the kernel and extensions in the same hardware address space, isn't that compromising on protection? Wasn't that the strike against the DOS-like structure that we talked about earlier? Well, the approach that Spin took was to rely on the characteristics of a strongly typed programming language(<i> Based on wiki, "In a strongly typed language each data area will have a distinct type and each process will state its communication requirements in terms of these types."</i>), so that the compiler can enforce the modularity that we need in order to give guarantees about the protection. So, by using a strongly typed language, in the case of Spin, they used modula-3 more on that in a minute. The kernel is able to provide well-defined interfaces. All of you may be quite familiar with declaring function prototypes in a header file and having the actual implementation of the procedures in other files. In a large software project, this is the same idea that is now taken to the design of the operating system itself. After all, operating system is also a piece of software, a complex piece of software, and why not use a strongly typed language as the basis for building the operating system. That's the idea in the spin approach. Now, what you get when you use a strongly typed language, is that you cannot cheat. For instance, in a language like C, you can type cast pointers so that a given data structure can be viewed completely differently, depending on what you need to get done at the moment. That's not possible with a strongly typed language. </li>
   <li> Data abstractions provided by the programming language such as an object serve as containers for logical protection domains. That is, we are no longer reliant on hardware address spaces to provide the protection between different services and the kernel. As I mentioned the kernel provides only the interfaces and these logical protection domains actually implement the functionality that is enshrined in those interface functions. And there can be several implementations of the interface functions. And that's where the flexability comes in. </li>
   <li> Applications can dynamically bind different implementations of the same interface functions. And that's how we get different instanciations of specific system components. Getting you the flexibility that you want in constructing an operating system. Because we have co-located the kernel and the extension in the same hardware address space, we are making the extensions as cheap as a procedure call.</li>
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
      <li>Combine: As I mention before, to reduce the proliferation of small logical protection domains you may want to combine protection domains to create an aggregate larger protection domain and SPIN provides a mechanism for that, which is the combined mechanism. Once the names in a source and target protection domain have been resolved, they can be combined to create an aggregate domain. And the aggregate logical protection domain will have entry points, which is the union of the entry points that were exported as names from the source and the target or any number of such domains that have been combined together to create an aggregate domain. So this combined primitive in SPIN is mainly useful as a software engineering management tool to combat the proliferation of many small domains.</li></ul></li>
   <li>So, once again, the road map for creating services is, write your code as a Modula-3 program with well defined entry points. And using the SPIN mechanism of create, you can instantiate a service and export the names that are available in that service. And if another logical protection domain wants to use the names that are exported, it can do so by using the SPIN mechanism resolve that causes the dynamic binding of the source and target logical protection domains. And finally, the combined primitive allows aggregation of logical protection domains to create an aggregate domain, that's the union of all the entry points that are available in the component logical protection domains. This is it. This is the secret sauce in SPIN to get protection and performance while allowing flexibility. Everything hinges on the strongly-typed nature of the programming language that is being used for implementing the operating system. That is, the language allows compile time checking, and run time enforcement of the logical protection domains. That's the key to the success of this approach to providing flexibility, protection, and performance, all in one bag.
   </li>
</ul>


<h2>7. Customized OS With Spin</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150725808-db988857-deb3-4428-82dc-0e1754fda7d8.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>So the upshot of the logical protection domain is the ability to extend SPIN to include operating system services and make that all part of the same hardware address space, so no border crossing between the services or the mechanisms provided by SPIN.</li>
   <li>So here is one example (P1) where all these system services are implemented as protection domain and using create, resolve and combine. We've created all these services as logical extensions of SPIN.</li>
   <li>Here is another extension (P2) living on top of the same hardware, concurrently with the first extension. And as you see, each of these mounds represent a completely different operating system. And each of these mounds may have their own subsystems for the same functionality. For instance, this process P2 uses memory manager two. </li>
   <li>This process P1 uses memory manager one. Both of them implement the same functionality but very differently, hopefully, to cater to the needs of the applications that need those services. But they may also have common subsystems. For example, the network protocol stack, maybe shared by both extensions that live on top the same hardware framework.
   </li>
</ul>

<h2>8. Example Extensions</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150726090-840ee128-a903-4a84-8ee9-a8390afd41dd.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>Here is a concrete example of an extension. It's a fairly standard implementation, let's say of Unix operating system, but it is implemented as an extension on top of the spin. (Displayed on the left side of the slide) </li>
   <li>Here is a more fun example (Displayed on the right side of the slide) . A client server application that is implemented directly on top of spin as an extension. In other words, there is no operating system. A display client uses an extension interface to implement the functionality for displaying video that is going to be sent by a video server. So both of these are extensions on top of basic spin, and provide exactly the functionality that is needed for the video server application. And the bounding box here is showing spin and the extensions thereof. And similarly the bounding box here (Unix,left side) is showing spin and the extension thereof, in this case, it is an entire operating system. In this case(right side, Spin top chart) it is just the client of the video server. And in this case(right side, Spin bottom chart) it is just the video server itself as an extension on top of spin.
   </li>
</ul>

<h2>9. Border Crossings</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150726577-fb51a06d-fe38-4172-ab13-600c1f702104.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150726669-64a2ba90-e435-466f-9bb1-21b6d309477c.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>The right answer, either SPIN or monolithic, will result in the least number of border crossings. Why? In the microkernel based structure, we're assuming that each one of these services are available as server processes in their own hardware address space, and therefore any system service that an application needs may have to go through multiple border crossings. And by border crossing we mean going across different address spaces and the incumbent loss of locality that it entails. Whereas, in the case of monolithic, you have only two border crossings. One to get to the monolithic kernel and the other to come out back into the application. And by construction, in SPIN also, because we are taking SPIN and extending it with the services. They're all contained in the same hardware address space. So, even though we may be going through several different protection domains in satisfying the system call emanating from an application. Those prediction domains are all logical prediction domains. It does not involve border crossing that entails change of locality and loss of performance.</li>
</ul>

<h2>10. Spin Mechanisms for Events</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150726970-74c738db-57fd-4dc7-9925-d80a12b5d334.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>An operating system has to feel external events. For example, external interrupts that may come in when a process is executing, Or the process itself may incur some exceptions such as a page fault, Or it may make system calls. All of these are events that needs to be fielded by the operating system and SPIN has to support such external events. And SPIN supports such external events using an event-based communication model. Services can register what are called event handlers with the SPIN event dispatcher, and SPIN supports several types of mapping: It supports a one to one mapping between an event and a handler. It also support one to many mapping between an event and the handlers. And it also supports many to one mapping, many events being mapped to the same handler.</li>
   <li> Let's look at concrete examples to illustrate the power of this mapping and how it might help in building system services. And what this picture (right side on the slides) is showing you is a typical protocol stack.
      <ul>
         <li> You may have several different interfaces available on your machine. And therefore, a network packet may arrive through one of several interfaces — Let's say you have an Ethernet interface and you have an ATM interface.</li>
         <li>A network packet may arrive on the Ethernet port or an ATM port.Those are events, and both of those may be IP packets, in which case there is an IP handler that needs to see the events.</li>
         <li> So here is an example of many events mapping to the same handler. Ethernet packet arrival is an event, ATM packet arrival is an event, different events but they map the same handler, which is the IP handler. That's an example of many-to-one mapping. </li>
         <li>The processing of the packet by this IP handler results in an IP packet arrival event. And there will be several clients of the IP layer of the protocol stack. There will be UDP transport, there will be TCP transport, there will be an ICMP layer. And they are all sitting on top of this IP network layer.So when an IP packet arrival event is figured by this handler, there are multiple clients for that, and this is an example of a one to many mapping, one event to multiple handlers, that need to get triggered. And the SPIN dispatcher allows any number of handlers to be registered as handling a particular event type. And when that even type is reached, then all the handlers associated with that event type will get scheduled by the SPIN dispatcher. The order in which they get scheduled is not something that the designer can count on, because SPIN has freedom in the ordering which these event handlers may get scheduled when a particular event arise. But all the handlers that are associated with an event will get triggered when that event is released.That's an example of one-to-many mapping. </li>
         <li>And finally, here is an example of a one to one mapping. If it's an ICMP packet, the ICMP handler processes the IP packet, and if it is an ICMP packet, then it raises an event than an ICMP packet has arrived, and maybe there is only one client for that particular event, and that may be the ping program, so that's one-to-one mapping.</li>
         <li>Even handlers may also be specified with guards of finer grain handler execution. For example, this handler could specify that only when IP packets arrive it should be executed. So that's a guard that the IP packet handler could specify so that even though different kinds of packets may arrive on these interfaces, this handler will only get triggered when the packet that arrived on the different interfaces and IP packet.
         </li>
      </ul>
   </li>
</ul>

<h2>11. Default Core Services in Spin</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150732153-097e5719-fe02-4fb1-87d6-daf1a889045f.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>So now, we know the toolbox provided by SPIN for building an operating system. Now, one can build each of the services we talked about early on, that an operating system should provide, such as memory management, CPU scheduling, threads, file system, network protocol stack, and so on, from scratch, as extensions to SPIN.</li>
   <li>As we know, memory management and CPU scheduling are core services that any operating system should provide. However, an extensible operating system should not dictate how these services should be implemented. SPIN provides interface procedures for implementing these services in the operating system.</li>
   <li> Physical memory is a precious resource. We know that native operating systems such as Linux or Windows manages the physical memory that is available from the hardware. SPIN wants to allow extensions to manage physical memory allocated to them in whatever fashion they choose to. The macro-allocation of a bunch of physical memory to an extension, it's outside the scope of this discussion. But, assume that the allocation of a bunch of physical memory happens when an extension's charged up.</li>
   <li>The discussion in this tablet frame is to do with the management of the pre-allocated physical memory by the extension. The interface functions that I'm showing you here from memory management are simply header files provided by SPIN. For example, allocating a page frame. Deallocating a page frame. Reclaiming a page frame. Similarly, allocating a virtual page or deallocating a virtual page which might be used for dynamic memory allocation. Translating, has to do with creating and destroying address spaces, adding or removing mapping between virtual pages, and physical frames. All of those are interface functions, that are provided as header files by SPIN. And memory management, because of what we said earlier about overcommitment of memory. Not all of a processor's address space is going to be fitting in physical memory. So, there are event handlers, that are provided as part of the core service of SPIN for handling page fault, access fault, meaning, if you had a page that is write protected and if a process tries to write to it, that's an access violation. Or, if a process is trying to access a region of memory that it doesn't have access to, generating a bad address exception. All of these are interface functions that are defined as core services of memory management in the SPIN operation system. </li>
   <li>It is not saying anything about how these services are implemented, but it is giving you just a header file. The implementer of an extension has to write the actual code for these header functions and create a logical protection domain that corresponds to physical address management, virtual address management, translation management, and the handler functions for dealing with these different types of events. Once the logical protection domain is dynamically instantiated, it becomes an extension of SPIN, and after that there's no border crossing between a particular service that has been so instantiated and SPIN itself. And all of these functions are invoked automatically when the hardware events occur, corresponding to a page fault or access violation fault and so on.
   </li>
</ul>

<h2>12. Default Core Service in Spin (cont)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150732651-1404aa8b-8e8f-4b52-b117-0c46f1b89d4a.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>SPIN arbitrates another precious resource, which is another core service, namely the CPU. SPIN only decides at a macro level, the amount of time, that is given to a particular extension. That's done through the SPIN global scheduler. The global scheduler interacts with the application threads package. And application is a loose term here. It is the extension that is living on top of SPIN. Which may be an entire operating system or maybe just an application. </li>
   <li>For example, let's say, we are running Linux and Vista as two extensions on top of SPIN. Each maybe given a particular time slice, say of x milliseconds. How each extension uses the time that has been given to it for scheduling user-level processes running inside the operating system is entirely up to those extensions. And to support the concept of threads in the operating system and management of time, SPIN provides an abstraction called strand. The actual operating systems that extend SPIN will have the threads map to strands. So strand is the unit of scheduling that SPIN's global scheduler uses, but the semantics of the strand is entirely decided by the extension. If, for instance, I'm implementing pthreads, I will define the semantics of the strand to be the semantics of the pthread's scheduler. And there are event handlers that help in the scheduling that needs to happen in the extensions. And the kind of events that SPIN provides for this core service of CPU scheduling are block, unblock, checkpoint, and resume. And the extensions event handlers have to give the semantic meaning of what needs to happen when these event handlers are called, because these are only interface functions. What needs to happen when this interface function is called is up to the extension. For example, a disk interrupt handler may result in an unblock event being raised for a particular strand that was waiting for the disk IO completion. Similarly, if an application were to make a system call that is a blocking system call, then the service that provides that facility to the application will raise this block event, which will result in the extension taking the appropriate action of saving the state of the currently running process. And putting it in the appropriate queues that it has, to wait for that system call completion. </li>
   <li>So in a nutshell, what SPIN provides are exactly the kind of primitives that may be needed by an extension that wants to provide the service of CPU scheduling. So SPIN only provides the interface function definitions. The semantics are all up to the extension on how exactly the scheduling is affected. And all that SPIN does is to ensure that the extension gets time on the CPU through this global scheduler that SPIN has for allocating time to different extensions that may be concurrently living on top of SPIN.</li>
</ul>

<h2>13. The SPIN Approach Conclusion</h2>
<ul>
   <li>There are some deep implications that may not be readily obvious. Core services are trusted services, since they provide access to hardware mechanisms. Why? The services may need to step outside the language-enforced protection model to control the hardware resources. In other words, the applications that run on top of an extension have to trust the extension. Extensions to core services affect only the applications that use that extension. That is, it is not catastrophic and does not affect other applications that do not rely on this particular extension. </li>
</ul>


# L02c: The Exokernel Approach
TODO: take a look and add to note
http://www.cs.cornell.edu/courses/cs6410/2010fa/lectures/08-extensible-kernels.pdf
https://amplab.github.io/cs262a-fall2016/notes/09-extensible-oses-Exokernel-Spin.pdf
https://ucbrise.github.io/cs262a-spring2018/notes/19-extensible-oses-Exokernel-Spin.pdf
https://cs.nyu.edu/~rgrimm/teaching/sp11-os/exospin.pdf

Todo: video to watch after read - Dawson R. Engler, Frans Kaashoek and James O'Toole, "Exokernel: An Operating System Architecture for Application-Level Resource Management ",
https://www.youtube.com/watch?v=TZB60F5kNSk

<h2>1. Exokernel Approach to Extensibility</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150733149-da884df6-8d47-4ca3-9782-f3b5b775bf0d.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>Having seen SPIN's approach to extensibility, now we will look at Exokernel's approach to operating system extensibility. The name Exokernel itself comes from the fact that the kernel exposes hardware explicitly to the operating system extensions living above it. </li>
   <li>The basic idea in Exokernel, is to decouple authorization of the hardware from its actual use.</li>
   <li> let's say you want to do research in my lab. I may interview you, and once we're on the same page, I'll give you a key to the lab. And the resources you need in order to do work in the lab. for example, laptop, servers and so on. Then I get out of the way. When you actually use the resources, that's exactly the same idea in Exokernel.</li>
   <li> Library operating system asks for a resource. Exokernel will validate the request for the resource from the library. And bind the request to the specific hardware resource. In other words, Exokernel exposes the hardware that was requested by the Library OS through creating a secure binding between the ask and the actual hardware resource. Once Exokernel has established this binding, it creates an encrypted key for the resource, and gives it to the requesting library operating system. Similar to the analogy that I gave you, of a student using my lab resources, the semantics of how the resource is going to be used by the library is entirely up to the library. Of course, within the norm of accepted use, similar to what I may impose as certain restrictions or rules of behavior that students have to observe in the lab. Same way, there are certain accepted norms for the user's resource that Exokernel may have imposed. And so long as the library operating system is staying within those norms, then the semantics of how a particular hardware resource is used is entirely up to the library operating system. Once a library operating system has asked for a resource and Exokernel has created the binding for that resource to the requesting library operating system, then the operating system is now ready to use the resource.</li>
   <li> Now, how does it use the resource? Basically, what the library operating system will do is present the encrypted key that is received, authenticating that use of the resource for this library to the Exokernel. In other words, Exokernel will be able to validate whether the key presented to it, is the key that was presented for this particular libraries operating system. So in other words, the key cannot be forged and cannot be passed around. If I gave a key to this library operating system, that key, if it is presented to the Exokernel by this library operating system, it's a valid key. Even if it's a valid key, but it is not the operating system to which Exokernel gave the key, then that request would be denied. So with a valid key, any time the library operating system can present the key to the Exokernel, Exokernel will validate it, and then, the library operating system is free to go in using that resource for which it has this valid key. This is sort of like a doorman in an apartment building, checking when a resident comes in, whether the resident is a bona fide occupant of the residence. Once inside his apartment, what the resident does is not something that the doorman cares about. Exactly the same thing is being done by Exokernel as a doorman for using the hardware resource for which a valid key exists with a library operating system.</li>
   <li> So, establishing the secure binding is a heavy duty operation. That's where Exokernel comes in the middle of saying, well, this particular library operating system wants access to a specific resource, can I give it? And it makes that decision. Once such a secure binding has been established, the actual use of the hardware is going to be much cheaper.
   </li>
</ul>

<h2>2. Examples of Candidate Resources</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150736446-1c09f27d-d0f2-4ae3-8460-000a23f64a8e.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>You are thinking, wow this sounds tedious and not performance conscious if exokernel has to validate the key every time for the library to use it. Well, it depends on what we mean by a resource.</li>
   <li> Let's look at some examples. Here is an example of a candidate resource, a TLB entry. TLB entry is going to establish a mapping between a virtual page to a physical page. That mapping of the virtual page to the physical page is done by the library. Now, once the mapping has been done by the library, it presents the mapping to the exokernel along with capability of the key, the encrypted key that it has for a particular TLB entry. Exokernel validates it and puts this mapping into the specific TLB entry of the hardware TLB. Now this is a privileged operation. Putting an entry into the hardware TLB, is a privileged operation. The library operating system cannot do it by itself, because it doesn't have the same privilege as exokernel. And therefore, once that capability in the form of the encrypted key for this TLB entry is presented to exokernel, then exokernel, on behalf of that operating system, is putting that mapping that has been established by the library operating system into the specific TLB entry of the hardware TLB. Once this entry has been put into the TLB, the process that is going to be using that virtual page, when it is running, can use this multiple times without exokernel intervention. So even though putting it into the hardware TLB require the intervention of exokernel because we are messing with hardware. Once that entry has been put in, that entry is on behalf of this library operating system. And processes of that library operating system, when they are running on the CPU, can access the TLB. And do the translation any number of times because all of that is happening under hardware control and exokernel, of course, is not in the middle of any of that. So that gives you an idea of how, even though we are seeing that in order to do certain things in the hardware, you need exokernel help for the library operating system. The normal use of a hardware resource is not going to be in any way affected by the fact that exokernel is in the middle between the hardware and the library operating systems.</li>
   <li> Here is another example of a candidate resource. Let's say that the operating system wants to install a packet filter that needs to be executed every time a network packet arrives on behalf of a library operating system. Predicates for looking at this incoming packet are loaded into the kernel by the library operating system. Now, this is a heavy-duty operation, because you're doing it with the help of exokernel. But once those predicates have been loaded into exokernel by the library operating system, on every packet arrival exokernel will automatically check it using those predicates.</li>
   <li>So those are examples of candidate resources that tell you that establishing the binding may be expensive but using the binding, once established, does not incur the intervention by exokernal and therefore it can happen at hardware speeds.
   </li>
</ul>

<h2>3. Implementing Secure Bindings</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150881628-c40d8052-5149-47ef-846e-eb565f2ab3bf.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Now let's talk about the mechanisms that are there in Exokernel for implementing these secure bindings. There are three methods.</li>
   <li> The first method is Hardware mechanisms. And I gave you this example of a TLB entry. Other examples of hardware mechanisms include getting a physical page frame from exokernel or a portion of the frame buffer that is being used by the display. These are all examples of specific hardware resources that can be requested by the library operating system and can be bound to that library operating system by exokernel. And exported to the library operating system as an encrypted key, and once the library operating system has the encrypted key for that resource, it can use that any time it wants.</li>
   <li> The second mechanism that exokernel has is software caching on behalf of each library operating system, specifically the shadow TLB, or caching the hardware TLB in a software cache for each library operating system is to avoid the context switch penalty when exokernel switches from one library operating system to another. Basically, what will happen is that at the point of context switch, exokernel will dump the hardware TLB into a software TLB native structure that is associated with that specific library operating system. And similarly load the software TLB of the library operating system to which it is switching to into the hardware TLB. We will talk about these mechanism in much more detail shortly but at this point I wanted to mention that this is second mechanism that exists in exokernel for establishing a secure binding between a library operating system and the hardware.</li>
   <li> The third mechanism that exokernel has for establishing a secure binding on behalf of an operating system is downloading code into the kernel. This is simply to avoid border crossing by inserting specific code that an operating system once executed on behalf of it. I gave you the example of the packet filter earlier. So that's an example of downloading according to the kernel that needs to be executed on behalf of a particular guest operating system. And if you think about it, this idea of downloading code into the kernel is very similar to the spin idea of extending the kernel with logical protection domains that I created and dynamically linked in. Similarly, in the exokernel world, a library operating system can, if allowed by exokernel, securely download code into the kernel that will get executed under specific conditions that are laid down by the library operating system.
   </li>
</ul>

<h2>4. Exokernel vs Spin</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150882510-8118dff5-59b0-4edc-8e8f-86fff517bca9.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150882638-fefef873-7929-4fc6-9c5c-b3ea2b66564b.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>The right answer is exokernel. Now so long as SPIN's logical protection remains, are entirely following modular three language enforced, compile time checking, and run time verification, there is no violation of protection In SPIN. But we cannot say the same about Exokernel because it is arbitrary code that is being downloaded into the kernel by a library operating system and that's the reason that Exokernel may end up compromising protection more than the SPIN mechanism. But having said that, I should mention that it's not always possible to live within modular-3 enforced protection domains, even in SPIN. Because we've seen that even in SPIN, in order to do certain things in the hardware, SPIN may have to step outside the protection boundaries of modular-3. In other words, a reality that exists with real hardware is that it's not always possible to do this within the confines of language-enforced protection domains. But if you just think in terms of the logical protection domains as defined by SPIN as modular three objects. Those have strong guarantees of protection compared to arbitrary code that we can download into Exokernel.
   </li>
</ul>

<h2>5. Default Core Services in Exokernel</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150882956-39456960-6123-4f54-a1ec-18a3a66369dd.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>When we discussed spin, I mentioned that memory management and CPU management are core services that any operating system has to provide, and we discussed how spin had its own way of dealing with those core services. We will do the same analysis for exokernel as to how it does memory management and CPU scheduling. </li>
   <li> first memory management</li>
   <li> Specifically let's see how exokernel will handle a page fault incurred by a library operating system. So in this picture that I'm showing you, here is an application thread running, and maybe this application thread, belongs to a specific library operating system and so long as this application thread is doing normal memory accesses, where all its virtual addresses have been mapped to physical page frames. The thread is executing at hardware speeds on the CPU. Life is good. But life may not be always good. Because this thread my incur a page fault. And when it incurs a page fault, the page fault is first fielded by Exokernel.</li>
   <li> Exokernel knows which library operating system is currently executing on the CPU, it has no knowledge of processes within a library operating system. All it knows is that this library operating system is doing something on the CPU. And it knows that there is a page fault incurred, and it can kick it up to the library operating system through a registered handler and we will talk about how these handlers are known to Exokernel later on. Right now, I just want to give you road map of how a page fault is handled in Exokernel.</li>
   <li> Because the library operating system knows about processes, whereas Exokernel has no knowledge about that, and it services the page fault. And servicing the page fault may involve requesting Exokernel for a page frame to host the specific page that is missing. And if it does that as we detailed before, that will involve the library asking Exokernel for a page frame and Exokernel creating a binding for a page frame. And returning an encrypted key for the page frame. Assume for the moment that the library operating system has page frames already with it and all that it is doing at this point is in servicing the page fault, it establishes a mapping between the virtual piece that was missing and the page frame that contains the contents of the virtual page. Once it does that, that mapping between the virtual page and the frame that it corresponds to has to be presented to the Exokernel. </li>
   <li> So the library presents that mapping to the Exokernel along with the TLB entry, where it wants this mapping to be placed in the hardware TLB. Remember that, when the process runs the CPU is consulting the hardware TLB, to see if there's a valid mapping between the virtual page number and a physical frame, so that the CPU can go and access that page. And we got the page fault because the mapping did not exist, and the whole point of this exercise is for the library operating system to reestablish a mapping. But it cannot do that directly into the TLB, so it presents a mapping to Exokernel along with the encrypted key that represents the capability that the library operating system has to a specific TLB entry where it wants this to be placed.</li>
   <li> Exokernel will validate the encrypted key presented by the library operating system, and assuming all is good, it will go ahead and install the mapping in the hardware TLB, and this is a privileged operation, meaning that it can be done only in the kernel mode of the processor. That's the reason that you have the red line between the library operating system that runs at the non-privileged level, and Exokernel that runs at the privileged level to do certain operations such as, installing an entry into the TLB. And once the entry had be installed in the TLB, if the library operating system is once again scheduled on the processor and if the same process is run by the library operating system when it generates the same virtual address we're going to find a valid mapping and life will be good.
   </li>
</ul>


<h2>6. Secure Binding</h2>
<ul>
   <li>Downloading code into the kernel and secure binding, how is this secure? This is a bit dicey. Here, the library operating system is given an ability to drop code into the kernel. The rationale is purely a performance one. Namely, avoid border crossings. But obviously it can be a serious security loophole. Even in spin we've noticed that a core service may have to step outside the language enforce protection mechanism. In order to control hardware resources. Bottom line is, while both SPIN and Exokernel start out with the idea of allowing extensibility, they may have to necessarily restrict who will be allowed to do such extensions. Not any arbitrary user. It has to be a trusted set of users.
   </li>
</ul>


<h2>7. Memory Management Using S-TLB</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150884183-eeed29b4-b162-4538-a018-42c471d9fe2f.png" alt="drawing" width="500"/>
</p>

<ul>
   <li> I mentioned software caching as a mechanism that's available in exokernel for establishing secure binding. And software TLB is one specific example of using the software caching idea.</li>
   <li> As we know, when we have a context switch one of the biggest sources of performance loss is the fact that you're losing locality for the newly scheduled process. And since the address base occupied by this library operating system (OS1) and this library operating system (OS2) are necessarily completely different, when we switch from one library operating system to another, we have to flush out the entire TLB. And if we do that, when we run this other library operating system, it's not going to find any of its virtual addresses in the TLB. And that's a huge source of overhead and in order to mitigate that overhead, exokernel has this mechanism called software TLB.</li>
   <li> The idea is quite simple, the software TLB is sort of a snapshot of the hardware TLB for each of the operating systems. So this software TLB (S-TLB OS1), if a data structure in the exokernel that represents the mappings for operating system one (OS1). Similarly, this data structure(S-TLB OS2) represents the mapping for library operating system number two(OS2). So let's say currently we're running library operating system one. So the TLB entries correspond to valid mappings for library operating systems one. And let's say that exokernel decides to switch from this library operating system (OS1) to this one (OS2). At that point, what exokernel will do is to dump the TLB into the software TLB data structure it has on behalf of OS1. Actually not all of the TLB, but we'll get to that later on, some subset of the TLB mappings will be dumped into this data structure(S-TLBOS1). And let's say that we are switching from this library operating system (OS1) to this library operating system (OS2). In that case, what exokernel is going to do is, it's going to pre-load the TLB with the software TLB data set (S-TLB OS2) that is associated with this library operating system(OS2). Essentially, what we are accomplishing is that when the library operating system starts running on the CPU it will find some of its mappings already present in the hardware TLB. That's the idea in exokernel of having the S-TLB data structure associated with every library operating system to mitigate the loss of locality that happens when you do context switch, in terms of address translations. Of course, when the library operating system starts running, and it does not find a mapping for a virtual address in the TLB, at that point, exokernel is going to kick up that missing virtual page translation in the TLB as a page fault up into the library operating system and you'll get result exactly as we detailed earlier.
   </li>
</ul>



<h2>8. Default Core Services in Exokernel (cont)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150884996-c03b49a5-1816-4d68-8cbc-629e3ff4d285.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>The second core service of course is CPU scheduling. And exokernel in order to facilitate this core service, maintains a linear vector of time slots.</li>
   <li>So, time is divided into these epochs T1, T2, T3 and so on, and every time quantum has a begin and an end. And these time quantums represent the time that is allocated to the library operating systems that live on top of exokernel. And the time quantum is bound by the begin end markers for each library operating system.</li>
   <li>Each library operating system gets to mark its time quantum at startup in this linear vector of time slots. So, for instance, OS1 may say that I get this time slot (T1), I get this time slot (T3), maybe some of the time slot (T5) and so on. And similarly, OS2 marks its spots in the linear vector time slots.</li>
   <li>So CPU scheduling in exokernel is essentially looking at this linear vector of time slots and asking the question, in this time quantum, which is the library operating system that should be running on the processor? And there is a start time for the time quantum. There's an end time for the time quantum. And Let's say, OS1 is now running on the CPU. When the timer interrupt goes off, at this endpoint, control is transferred by exokernal to the library operating system to do any saving that it has to do of the context. And the time that is allowed for a library operating system to do the saving and restoring of context at the point of a context switch is bounded. And if an operating system misbehaves, let's say that OS1, when exokernel says it's time for you to save your context and give back the processor to me so that I can schedule it, for some of the operating systems. And let's say OS1 takes more time than is allowed to at the point of this context, which, in that case, what will happen is exokernel will remember that OS1 misbehaved And it will take time off of OS1 the next time it is scheduled. For there's a penalty associated with exceeding the time quantum. The time quantum is bound. During this time quantum, OS1 has complete control of the processor, and exokernel is not going to get in the middle of it, unless a process that is running on behalf of OS1 incurs a page fault. In that case, exokernel has to come in the middle in order to field that page fault and pass it up to the operating system. And otherwise, during this time quantum, the operating system is entirely at liberty to use a processor for running whatever processes it wants to.</li>
   <li>At the end of the time quantum, the time integer goes off. Exokernel feels it and kicks it up to the operating system and tells the operating system to clean up its act. Save any context it wants so that the CPU can be reallocated to the next library operating system. And that's where the time is bounded, as to how much time the library operating system can take in order to do that saving of the context.
   </li>
</ul>

<h2>9. Revocation of Resources</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150885574-acca2869-5b54-44a2-9f00-42a3b4c2c646.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Notice so far that Exokernel supports no extractions. It only has mechanisms for securely giving resources to the Library Operating System. And the resources may be a space resource, memory, time resource, or specific hardware resource like area of the graphic display, and so on. Therefore, exokernel needs a way of revoking or reclaiming resources that have been allocated to a library operating system. Of course, exokernel keeps the scoreboard as to what resources have been allocated to different library operating systems. And therefore, at any point of time, it can revoke the resources that had been given to a library operating system. Sort of similar to a student working in my lab, if he or she graduates, I might say, well, it's time for you to return the key to the lab, and I have a way of revoking that resource that I may have given to a student for use in my lab. Similarly, exokernel has mechanisms for revoking resources from a library operating system.</li>
   <li>So specifically, for instance, if you think about the CPU, exokernel has no idea what the library operating system is using the CPU for, as opposed to SPIN for instance, which has an abstraction of strand that represents the user level that represents the library's notion of a thread. So exokernel has a revoke mechanism for exactly this purpose of revoking resources that have been allocated to a library operating system. And the revoke call, which is an up call into the library operating system, is going to give this library operating system a repossession vector, saying these are the resources I'm taking away from you. For instance, it could say, you know, remember I gave you these page frames? Well, I'm going to reclaim those page frames. And when it gives those repossession vector to the library operating system, it is a responsibility of the library operating system to do what it needs to do in order to clean up. In other words, the library takes corrective action commensurate with the repossession vector that has been presented by exokernel to it. For example, if the exokernel tells this library that I'm going to take away page frame number 20 and page frame number 25 from you, then the library operating system will say, oh, in that case I have to stash away the contents of those page frames into the disk. So that's the corrective action that the library operating system will have to take when it is informed by exokernel that some hardware resources that have been given to it have been taken away by exokernel. To make the life of the library operating system easier, exokernel also allows a library to seed it with autosave options for resources that exokernel wants to revoke. So, in other words, if exokernel decides to revoke, let's say, some page frames from the library operating system, the library could have seeded the exokernel ahead of time. That, any time you want to take away these page frames, dump it into the disk. And that kind of seeding allows the exokernel to do the work on behalf of the library operating system. So that, at the point of revocation, the amount of work that the library has to do, in order to do corrective action for the repossession, is minimal.
   </li>
</ul>

<h2>10. Code Usage by Exokernel</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150886065-b781b600-9ad1-4e76-88cf-29b559a2355e.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150886225-c0f5140f-25b4-4988-83e9-5a6af8954bcf.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Here are a couple of examples. I'm sure that you may have thought of other examples as well. But packet filter is one thing that I mentioned already to you. And this is something that may be a critical component of performance for any operating system. And therefore, it might install a packet filter for demultiplexing incoming network packets so that exokernel can hand packets intended for a particular library operating system by running this code on behalf of the library operating system. A second example would be things that a library would like exokernel to do on its behalf, even when it is not currently scheduled. For instance, a garbage collection mechanism for an application is something that a library operating system may want to be run on behalf of it. And that's something that can be installed as a code that is downloaded into exokernel and executed on behalf of that library operating system. So these are all examples, you may have thought of other examples as well.
   </li>
</ul>

<h2>11. Putting it all Together</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150886447-75c972e7-f5d8-49af-8fc5-ccc2894cbff9.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>So let's put it all together: the mechanisms exokernel offers, and how library operating systems can live on top of this red line, that is the protection boundary for exokernel. And, meaningfuly execute applications that belong to it on the hardware resources, and not interfere with one another. That is, achieve both extensibility, protection, and performance. So one of the hooks for getting good performance is called that a performance critical for library operating system is something that can downloaded securely into the exokernel, so that piece of code becomes sort of the extension of exokernel. For a particular library operating system. This may be for OS one, this maybe for OS two and so on and so forth.</li>
   <li>So, now with this setup at any point of time some application process of some library operating system is running on the CPU. Now remember that Exokernel has no idea about processes within any of these library operating systems. All it knows is existence of these library operating systems and the fact that Exokernel has been the broker in giving some hardware resources, capabilities for some hardware resources, I should say, to specific library operating systems, and it has also been the broker for downloading some code specific to library operating systems into the exokernel codebase itself. Let's say that this particular thread (In CPU) belongs to this library operating system as long as this thread is well behaved by which I mean it's not doing anything funky. Doing normal program execution. Accessing memory. For which, mapping exists in TLB and so on. Life is good. Address translation happens on every memory access entirely in the CPU, and the process is making forward progress at memory speeds without intervention from exokernel or any of the library operating system. But, there could be this discontinuities, in the execution of this process. For example, let's say that this processor thread makes a system call, like opening a file. When it does that, that is a discontinuity in the normal execution of this process. Or worse yet, it may incur a page fault, meaning that not all of the pages for this particular process is currently In the mapping available to the hardware in the TLB, and therefore, there's a page fault. Even worse, this thread could do something stupid, such as divide by 0, or something like that, that causes an exception. And lastly, the thread is not doing anything to cause the discontinuity, but there is an external interrupt that came in and that is going to cause a discontinuity to this execution of this process on the CPU. All such discontinuities essentially result in the CPU incurring a fault or a trap, and the trap is fielded by exokernel. When such discontinuities occur, exokernel has to pass the program discontinuity to the appropriate library operating system that is living on top of it. Now, exokernel knows, based on the linear vector of time slots that I mentioned earlier which library operating system is currently running on the CPU. And therefore, it knows the right library operating system to which it has to pass the discontinuity that occurred right now for the currently executing process. To facilitate a finer-grain association between these different kinds of discontinuities. And the specific functionality in the library operating system for dealing with those discontinuities exokernel maintain state for each currently existing library operating system and we will discuss the state maintained by exokernel on behalf of every library operating system next.
   </li>
</ul>


<h2>12. Exokernel Data Structures</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150887819-b1251cc5-9181-4399-8521-199ae87e3959.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>To facilitate the bookkeeping needed for the different types of program discontinuities that I mentioned, exokernel maintains the PE data structure (Portable Executable. <i>Per wiki, The PE format is a data structure that encapsulates the information necessary for the Windows OS loader to manage the wrapped executable code. This includes dynamic library references for linking, API export and import tables, resource management data and thread-local storage (TLS) data.</i>) on behalf of each library operating systems.</li>
   <li>So for example, this PE data structure (PE OS1) corresponds to this library operating system(OS1). This PE data structure (PE OS2) corresponds to this library operating system (OS2). And the PE data structure contains the entry points in the library operating system for dealing with the different kinds of program discontinuities. For example, exceptions that are thrown by the currently executing process has a specific entry point in the library operating system. And similarly, external interrupts are going to be handled by interrupt handlers that have specific entry points in the library operating system. And a process may make system calls and if it makes system calls, there is a protected entry context, which is the entry point in a library operating system for system calls that are made by the currently running process that belongs to that specific library operating system. This last entry may correspond to the addressing context that is the location of the handler for page fault service in the library operating system.</li>
   <li>So in a nutshell, this PE data structure that is unique for every library operating system, contains the handler entry points for different types of events that are fielded by exokernel. In this sense, the PE and the associated exokernel action of calling the appropriated handler entry point, when a particular event is triggered, is very similar to the event handler mechanism that we discussed in the spin operating system.</li>
   <li>In addition to the PE data structure, we already mentioned the software TLB, that exokernel maintains on behalf of every operating system. So this is in software TLB (S-TLB OS1) for OS1, software TLB (S-TLB OS2) for OS2 and these are what are called a guaranteed mappings. In other words, I mentioned that on a context switch exokernel when it goes from one operating system to another, it saves the current TLB in the hardware, the hardware TLB in the software TLB that is associated with this particular operating system. And here is where this entry point is important. This actually is specifying to exokernel the set of guaranteed mappings that a particular library operating system wants exokernel to maintain on its behalf, every time it is scheduled. And that's the set of TLB entries that are dumped into the software TLB data structure at the point of a context switch. Not all of the hardware TLB entries, but only the entries that have been guaranteed to be kept on behalf of a particular operating system are dumped into a software TLB data structure. And those are the ones that will be repopulated into the hardware TLB when the same operating system is scheduled on the hardware. I mentioned an external interrupt is another source of discontinuity for the currently executing process. Well, note that an external interrupt may not always be for the currently scheduled operating system. It may be for some other operating system, maybe this operating system scheduled a disk I/O. And the disk I/O got complete. And that interrupt that came into exokernel is on behalf of operating system OS2 and not the process that is currently executing on the CPU for operating system OS1. Exokernel has to have a way of associating the interrupt that is coming in with a correct operating system which is expecting it. Downloading code into the kernel, which I mentioned as one of the mechanism that exokernel provides, allows first level interrupt handling on behalf of a library operating system.
   </li>
</ul>

<h2>13. Performance Results of Spin and Exokernel</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150888565-b2fa8dd3-b6c5-49a6-9006-95a180b96def.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Systems research is 1% inspiration and 99% perspiration.The only way to convince someone of your ideas is to build a system and evaluate it. </li>
   <li>Now, how do you go about evaluating a system such as Spin or ExoKernel? You can qualitatively argue that you're achieving extensibility without sacrificing safety. Simply by the way the system is constructed. But you also have to show that you're not losing out on performance, due to the extensability hooks. There's an interesting dilemma when you're reading research papers. How do you make sense out of the quantitative results from a data research paper? When you research papers, remember that absolute numbers are meaningless. It is the trends that are meaningful.</li>
   <li>While doing any performance study of a new system that you're proposing, you have to identify what the competition is. Remember that spin and exokernel were done in the early to mid 90s. For spin and exokernel, the competition is a monolithic operating system and a microkernel-based operating system. And the competition at that time for both Spin and exokernel was UNIX as a monolithic example, and Mach as a micro kernel example. And the performance questions always center around space and time. For example, how much better timewise, is the extended kernel, whether it is a spin approach or exokernel approach, compared to a microkernel-based approach in terms of performance. And since we know an extended kernel may have to incur loss of locality and border crossing overhead and so on compared to a monolithic kernel, another question that we may want to ask is, is the extended kernel at least as good as a monolithic kernel? What is the code size of implementing a standard operating system, say Unix, as a monolithic operating system, or a micro kernel based operating system or an extended kernel operating system. So that's a space question. And so, we can have time questions, as well as space questions when we propose a new way of doing any particular system component. I encourage you to read the performacne results in the papers.</li>
   <li>Key takeaway that you will see when you read the performance results reported by both SPIN and exokernel, is that they do much better than Mach microkernel. For protected procedure call. That is, when you go from one protection domain to another, how well are you doing? For that, both SPIN and exokernel exceed the performance of Mach. And you also see that both SPIN and exokernel do as well for dealing with system calls as a monolithic kernel does.
   </li>
</ul>


# L02d: The L3 Microkernel Approach
