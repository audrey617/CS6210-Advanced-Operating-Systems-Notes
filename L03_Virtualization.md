# Lesson outline
- [Intro to Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03a-intro-to-virtualization)
- Memory Virtualization
- CPU & Device Virtualization
- 
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150921690-d2850ea5-1e99-4a22-87f0-f244d18792b7.png" alt="drawing" width="500"/>
</p>

# L03a: Intro to Virtualization
<h2>1. Intro to Virtualization Introduction</h2>
<ul>
   <li>As we mentioned at the conclusion of the previous lesson module, the drive for extensibility in operating system services led to innovations in the internal structure of operating systems and dynamic loading of operating system modules in modern operating systems. And do not stop there. In this lesson, we will see how the concept of virtualization has taken the vision of extensibility to a whole new level. Namely, allowing the simultaneous co-existence of entire operating systems on top of the same hardware platform. Strap your seatbelts and get ready for an exciting ride. Before we look deep into the nuts and bolts of virtualization, let's cover some basics.
   </li>
</ul>

<h2>2. Virtualization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150922126-a5977ffd-1a4b-4627-ae6b-84548b71f971.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150922258-63c12546-6a15-4464-8880-60a3f002c963.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>You know what, if you picked all of them you're right on. We've heard the term virtualization in the context of virtual memory systems. Data centers today catering to cloud computing use virtualization technology. Java virtual machine. Virtual Box is something that you may be using in your laptop for instance. An IBM VM/370, was the mother of all virtualization in some sense, because way back in the 60s and the 70s, they pioneered this concept of virtualization. Google Glass, of course many of you may have heard the hype behind it, as well as the opportunities for marketing that this provides. Cloud computing, Dalvik Virtual Machine in the context of Android, VMware Workstation as an alternative to Virtual Box, and of course the movie, Inception, has a lot of virtualization themes in it.
   </li>
</ul>

<h2>3. Platform Virtualization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150922456-57e3b544-ca3d-4b07-954e-7617da5dd359.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>The words virtual and virtualization are popular these days. From virtual worlds and virtual reality to applications level virtual machines like Dalvik or Java's virtual machine. What we are concerned with here are virtual platforms. And by platform we mean an operating system running on top of some hardware.</li>
  <li>Let's say Alice has her own company, Alice Inc. And she has some cool applications to run for productivity. And let's say that they're running on a Windows machine, on some server that the company maintains. Now, if cost were not an issue, then this will be the ideal situation for Alice Inc. The hope of virtualization is that we will be able to give a company perhaps not as well endured as Alice Inc., let's's say Bala Inc., almost the same experience that Alice Inc., gets at a fraction of the cost. So, instead of a real platform that Alice Inc., has, Bala Inc., gets a virtual platform. Now, in this diagram, what I've shown you is a black box to represent the virtual platform. Because, so far as Bala Inc., is concerned, they don't really care what goes on inside this black box. All they want is to make sure that the apps that they want to run can run on top of this virtual platform. In other words, as long as the platform affords the same capabilities and the same abstractions for running the applications that Bala Inc. is happy.</li>
  <li>Now, we however as aspiring operating system designers are very much interested in what goes on inside this black box. We want to know how we can give Bala Inc., the illusion of having his own platform without actually giving him one. And including all of the cost, the implementation and maintenance associated with implementing such a virtual platform.
 </li>
</ul>

<h2>4. Utility Computing</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150922842-792c17e4-39a5-42e2-b361-867319dfd9b1.png" alt="drawing" width="500"/>
</p>

<ul>
   <li> Now, if we peak inside the black box, however,we find that it is not just Bala who is using the resources in the virtual platform, but there is also Piero and there is also Kim, and possibly others who are also running their own applications, their own operating system on the same shared hardware resources. Why would we want to do this? Well, unless we are quite unlucky, the hope is that sharing hardware resources across several different user communities is going to result in making the cost of ownership and maintenance of the shared resources much cheaper. The fundamental intuition that makes sharing hardware resources across the diverse set of users is the fact that resource usage is typically very burst.
   </li>
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151079284-7065de30-a9e7-4f53-a4b8-e60e6875d599.png" alt="drawing" width="500"/>
   </p>
<ul>
   <li> If we were to draw. Let's take one particular resource usage, for instance, memory. If we were to draw Bala's memory usage over time, it might look like this. And maybe Piero's need for memory over time may look like this. And Kim's like this, and so on. Now, adding all of these Dynamic needs of different user communities, we may see a cumulative usage pattern that might look like this. Now, let's consider Bala's cost. If he were to buy his own server, then he would have to buy as much as this, because that is the peak usage that he has and probably he'll want to do more than that just to be on the safe side. The virtual machine actually has a total available memory that's much more than the individual needs of any one of these guys and each of these guys. Get to share the cost of the total resources among themselves. On a big enough scale, what this would mean is that each of these guys will have potentially access to a lot more resources than they can individually afford to pay for at a fraction of the cost, because both the cost of acquiring the resource as well as maintaining it and upgrading it and so on is borne collectively.</li>
   <li>And that in a nutshell is the whole behind utility computing. That is promoted by data centers world wide, and this is how Amazon, Webservice, Microsoft, and so on they all provide resource on a shared basis to a wide clientele.</li>
   <li>Some of you may have already seen the connection to what we have seen in the previous lecture. Yes. Virtualization is the logical extension to the idea of extensibility or specialization of services that we've seen in the previous lesson, through the spin and exokernel papers. Only now, it is applied at a much larger granularity. Namely an entire operating system. In other words, virtualization is extensibility applied at the granularity of an entire operating system as opposed to individual services within an operating system.
   </li>
</ul>


<h2>5. Hypervisors</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151079599-bd7f0ca8-8b7f-48ce-85e1-47bb20164864.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Now returning to our look inside the black box, notice how we have multiple operating systems running on the same shared hardware resources. Now how is this possible? How are the operating systems protected from one another, and who decides who gets the resource and at what time? What we need then is an operating system of operating systems. Something called a virtual machine manager or hypervisor.</li>
   <li>The terminology that you will often encounter is either VMM, which stands for virtual machine monitor or hypervisor. And these operating systems that are running on top of the shared hardware resources are often referred to as virtual machines, or VM for short. We've always been using the term VM to mean virtual memory. You have to do a mind shift for this lesson module, VM means virtual machine. And each of these operating systems that run on top of the shared hardware resource, I'll often refer to them as either guest operating systems or virtual machines or VM for short. So watch out.</li>
   <li>Now, I should point out that there are two types of hypervisors.</li>
   <li>The first type is what is called a native hypervisor or bare metal, meaning that the hypervisor is running on top of the bare hardware. And that's why it's called a bare-metal hypervisor or a native hypervisor. And all of the operating systems that I'm showing you inside of the black box are running on top of this hypervisor. They're called the guest operating systems because they're the guest of the hypervisor running on the shared resource.</li>
   <li>The second type of hypervisor, what is called the hosted hypervisor, the hosted ones run on top of a host operating system and allows the users to emulate the functionality of other operating systems. So the hosted hypervisor is not running on top of the bare metal, but it is running as an application process on top of the host operating system. And the guest operating systems are [UNKNOWN] clients of this hosted hypervisor. Some examples of hosted hypervisors include VMware, Workstation, and Virtual Box. Both of these terminologies you may have heard of. If you don't have access to a computer that's running Linux operating system in this course you're likely to be doing your course projects on a virtual box or a VMWare workstation that's available to run on Windows platform. For the purpose of this lesson today, however, we will be focusing on the bare metal hypervisors. These bare metal hypervisors interfere minimally with the normal operation of these guest operating systems on the shared resources in a spirit which is very similar to the extensible operating systems that we studied earlier, like the spin and exokernel and therefore the bare metal hypervisors, offer the best performance for the guest operating system on the shared resource.
   </li>
</ul>

