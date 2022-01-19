# Lesson outline
- OS Structure Overview
- The SPIN Approach
- The Exokernel Approach
- The L3 Microkernel Approach
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150064173-e203bbc9-d523-4115-a3d1-2015d6bb625d.png" alt="drawing" width="500"/>
</p>

# L02a: OS Structure Overview
1. OS System Services: 

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034600-22a4c9d4-29ae-4d04-9644-fe6f5f209ab8.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034627-20bf078b-26de-4c81-a56a-8c00a34e09cf.png" alt="drawing" width="500"/>
</p>

3. OS Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150034723-081d76a5-afb5-4a23-be20-94085e201d8e.png" alt="drawing" width="500"/>
</p>
<ul>
   <li>What do we mean by OS structure? what we mean by this term is the way the OS software is organized with respect to the applications that it serves and the underlying hardware that it manages. It sort of like the burger between the buns that is application of the top and technology or heart rate on the bottom and system software or OS is what connects the application to the underlying hardware. Since this lesson is all about the OS structure, so get your thoughts on why the structure of an operating system is important </li> 
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035082-b75e6eaa-09ad-42c8-905c-ee36a4cd4a77.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035154-c9950f69-f1af-4ae4-9c62-47aaf43e32b6.png" alt="drawing" width="500"/>
</p>

5. Goals of OS Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150035261-73b64066-fbd5-47c8-b749-532ac1580781.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>Protection: protecting the user from system and the system from user, and also users from one another, also protecting an individual user from his/her own mistakes  </li> 
  <li>Performance: one of the key determinant of a good OS structure is how good is the performance of the OS, that is, what is the time taken to perform services on behalf of the application. A good operating system is one that provides the service that is needed by the application very quickly and gets out of the way </li> 
  <li>Flexibility: in fact one of the goals that we will be focusing a lot on in this course module is flexibility. Sometimes also called extensibility. Meaning that a service that is provided by OS is not one size fits all, but service is something that is adaptable to the requirements of the application  </li> 
  <li>Scalability: to ensure the performance of the operating system goes up as you add more hardware resources to the system. This is sort of an intuitive understanding but you want to make sure that the OS delivers on this intuitive understanding that when you increase the hardware resources the performance also goes up. That's what is meant by scalability </li> 
  <li>Agility: it turns out that both the needs of the application may change of the lifetime of an application, also the resources that are available for the OS to manage and give to the application may change over time. Agility of the OS refers to how quickly the OS adapts itself to changes either in the application needs or the resource availablity from the underlying hardware</li> 
  <li>Responsiveness: How quickly the OS reacts to external events, and this is particularly important for applications  that are interactive in nature, Imagine playing video game -> click mouse -> see actions immediately on the screen </li> 
</ul>

7. Commercial OS
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150036733-47c3e28c-d031-4afc-9e8d-de77cee0b8fc.png" alt="drawing" width="500"/>
</p>

<ul>
  <li>Are all the goals simultaneously achievable in a given operaing system? In the first glance it would seem that some of the goals conflict with one another. For example, it might seem that achieve performance we may have to sacrifice protection and/or flexibility </li> 
</ul>

8. Monolithic Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150037356-420d6c7b-d7a0-46eb-be9d-b6a99d7528af.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>Now let's talk about different approaches to operating system structuring. The first structure that I will introduce to you is what we will call as a monolithic structure. You have the hardware at the bottom which is managed by the operating system and hardware includes,of course, the CPU, memory, peripheral devices such as the network and storage and so on. And there are applications at the top. And each of these applications is in its own hardware address space. What that means is that every application is protected from one another because the hardware ensures that the address space occupied by one application is different from the other applications and that is the first level of protection that you get between the applications themselves. And all the services that applications expect from the operating system are contained in this blob and that might include file system and network access, scheduling these applications on the available CPU, virtual memory management, and access to other peripheral devices. The OS itself of course is a program providing entry points for the applications for the services that are expected by the applications. And code and the data structure of the operating system is contained in its own hardware address space. What that means is that the operating system is protected from the applications and vise versa. So even if an application were to do anything in terms of misbehavior, either maliciously or unintentionally because they are in there own address spaces and the operating system is in its own hardware address space. Malfunctioning of an application does not affect the integrity of the operating system services. That is, when an application needs any system service, we switch from the hardware address space that is representing this particular application, into the hardware address space of the operating system. And execute the system code that provides the service that that is expected by the application. For example, accessing the file from the hard disk, or dynamic allocation of more memory that an application may want, or sending a message on the network. All of these things are done within the confines of the address space of the operating system itself. Note that all of the services expected of the operating system, file system, memory management, CPU scheduling, network and so on, are all contained in this one big blob. And that is the reason it's also sometimes referred to as the monolithic structure of an operating system.</li> 
</ul>

9. DOS-like Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038025-a9894454-ade2-437b-9477-2cb9b794911e.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>Some of you may remember Microsoft's first entry  in the world of PCs, with their operating system called DOS, or disc operating system, and the structure of DOS looks as shown here.  And at first glance, at least visually, You might think that this structure is very similar to what I showed you as a monolithic structure before. What is the difference you see in this structure? First I would like you to think about it yourself before we go any further, so here is a question for you.</li> 
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038068-36f1bfda-ede2-45dc-a21b-82092d8e045a.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038086-61ec8c44-b9cf-4cc7-be4d-03d74aa474fe.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>You have noticed visually that the key difference  was, the red line was replaced by a dotted  line, separating the application from the operating system.  And what you get out of that is performance.  Access to system services are going to be like  a procedure call, and what is lost in the  DOS-like structure is the fact that you don't  have protection. Of the operating system from the application.  An errant application can corrupt the operating system.  We'll elaborate on this in the next few panels.</li> 
</ul>

10.DOS-like Structure (cont)
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150066994-b09789d9-021f-4d63-b3a9-cf08a89e4463.png" alt="drawing" width="500"/>
</p>
<ul><li>
So in the DOS-like structure, the main  difference from the monolithic structure that I  showed you earlier is that the red  line separating the application from the operating system  is now replaced by a dotted line. What that means, the main difference is  there is no hard separation between the  address space of the application And the address  space of the operating system. The good  news is an application can access all the  operating system services very quickly. As they would  any procedures that they may execute within their  own application with the same speed. At  memory speeds, an application can make calls into  the operating system and get system services. That's  the good news. But the bad news is  that there is no protection of the  operating system from inerrant application. So, the  integrity of the operating system can be  compromised by a runaway application, either maliciously  or unintentionally corrupting the data structures that  are in the operating system. Now, you  may wonder why DOS Chose this particular structure. Well, at least in the early  days of PC, it was thought that a personal computer, as the name  suggests, is a platform for a single user and, more importantly, the vision was,  there will be exactly one app that is running at a time. Not even multitasking.  So performance and simplicity was the key and protection was not primary concern  in the DOS-like structure. And that you can  get good performance comes from the simple observation  that there is No hard separation between the  application and the operating system. The operating system  is not living in its own address space.  The application and the operating system are in  the same address space. And therefore, making a  system call by an application is going to happen as  quickly as the application would call a procedure  which the application developer wrote himself or herself.
   </li> </ul>

