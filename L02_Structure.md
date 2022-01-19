# Lesson outline
- OS Structure Overview
- The SPIN Approach
- The Exokernel Approach
- The L3 Microkernel Approach


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
   <li>What do we mean by OS structure? what we mean by this term is the way the OS software is organized with respect to the applications that it serves and the underlying hardware that it manages. It sort of like the burger between the blunts that is application of the top and technology or heart rate on the bottom and system software or OS is what connects the application to the underlying hardware. Since this lesson is all about the OS structure, so get your thoughts on why the structure of an operating system is important </li> 
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
  <li>Performance: one of the key determinant of a good OS structure is how good is the performance of the OS,that is,what is the time taken to perform services on behalf of the application. A good operating system is one that provides the service that is needed by the application very quickly and gets out of the way </li> 
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
  <li>How commercial OS meets the goals? they don't meet all the goals </li> 
</ul>

8. Monolithic Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150037356-420d6c7b-d7a0-46eb-be9d-b6a99d7528af.png" alt="drawing" width="500"/>
</p>
<ul>
  <li>Now let's talk about different approaches to OS structuring. The first structure that I've introduced to you is what we will call as a monolithic structure you have the hardware of the bottom which is managed by the OS and the hardware includes the CPU, memory, peripheral devices such as network and storage and so on, and there are applications of the top and each of these applications is in its own hardware address piece what that means is that every application is protected from one another because the hardware ensures that the address space occupied by one application is different from the other application  And that is the first level of protection that you get between the applications themselves and all the services that application expect from the OS are contained in this block and that might include file system, network access, scheduling the applications on the available CPU, virtual memory management, and access to other peripheral devices </li> 
</ul>

10. DOS-like Structure
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038025-a9894454-ade2-437b-9477-2cb9b794911e.png" alt="drawing" width="500"/>
</p>


<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038068-36f1bfda-ede2-45dc-a21b-82092d8e045a.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150038086-61ec8c44-b9cf-4cc7-be4d-03d74aa474fe.png" alt="drawing" width="500"/>
</p>

todo

12. d
13. d
14. d
15. d
16. d
17. d
18. d
19. d
20. d
21. d
22. d
23. 


