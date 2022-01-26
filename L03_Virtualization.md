# Lesson outline
- [Intro to Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03a-intro-to-virtualization)
- [Memory Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03b-memory-virtualization)
- [CPU & Device Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03c-cpu--device-virtualization)

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
  <li>Let's say Alice has her own company, Alice Inc. And she has some cool applications to run for productivity. And let's say that they're running on a Windows machine, on some server that the company maintains. Now, if cost were not an issue, then this will be the ideal situation for Alice Inc. The hope of virtualization is that we will be able to give a company perhaps not as well endured as Alice Inc., let's's say Bala Inc., almost the same experience that Alice Inc., gets at a fraction of the cost. So, instead of a real platform that Alice Inc. has, Bala Inc. gets a virtual platform. Now, in this diagram, what I've shown you is a black box to represent the virtual platform. Because, so far as Bala Inc., is concerned, they don't really care what goes on inside this black box. All they want is to make sure that the apps that they want to run can run on top of this virtual platform. In other words, as long as the platform affords the same capabilities and the same abstractions for running the applications that Bala Inc. is happy.</li>
  <li>Now, we however as aspiring operating system designers are very much interested in what goes on inside this black box. We want to know how we can give Bala Inc., the illusion of having his own platform without actually giving him one. And including all of the cost, the implementation and maintenance associated with implementing such a virtual platform.
 </li>
</ul>

<h2>4. Utility Computing</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/150922842-792c17e4-39a5-42e2-b361-867319dfd9b1.png" alt="drawing" width="500"/>
</p>

<ul>
   <li> Now if we peek inside the black box, however,we find that it is not just Bala who is using the resources in the virtual platform, but there is also Piero and there is also Kim, and possibly others who are also running their own applications, their own operating system on the same shared hardware resources. Why would we want to do this? Well, unless we are quite unlucky, the hope is that sharing hardware resources across several different user communities is going to result in making the cost of ownership and maintenance of the shared resources much cheaper. The fundamental intuition that makes sharing hardware resources across the diverse set of users is the fact that resource usage is typically very burst.
   </li>
</ul>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151079284-7065de30-a9e7-4f53-a4b8-e60e6875d599.png" alt="drawing" width="500"/>
   </p>
<ul>
   <li> If we were to draw, let's take one particular resource usage, for instance, memory. If we were to draw Bala's memory usage over time, it might look like this. And maybe Piero's need for memory over time may look like this. And Kim's like this, and so on. Now, adding all of these Dynamic needs of different user communities, we may see a cumulative usage pattern that might look like this. Now, let's consider Bala's cost. If he were to buy his own server, then he would have to buy as much as this, because that is the peak usage that he has and probably he'll want to do more than that just to be on the safe side.</li>
   <li>The virtual machine actually has a total available memory that's much more than the individual needs of any one of these guys, and each of these guys get to share the cost of the total resources among themselves. On a big enough scale, what this would mean is that each of these guys will have potentially access to a lot more resources than they can individually afford to pay for at a fraction of the cost, because both the cost of acquiring the resource as well as maintaining it and upgrading it and so on is borne collectively.</li>
   <li>And that in a nutshell is the whole behind utility computing. That is promoted by data centers worldwide, and this is how Amazon, Webservice, Microsoft, and so on they all provide resource on a shared basis to a wide clientele.</li>
   <li>Some of you may have already seen the connection to what we have seen in the previous lecture. Yes. Virtualization is the logical extension to the idea of extensibility or specialization of services that we've seen in the previous lesson, through the spin and exokernel papers. Only now, it is applied at a much larger granularity, namely an entire operating system. In other words, virtualization is extensibility applied at the granularity of an entire operating system as opposed to individual services within an operating system.
   </li>
</ul>


<h2>5. Hypervisors</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151079599-bd7f0ca8-8b7f-48ce-85e1-47bb20164864.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Now returning to our look inside the black box, notice how we have multiple operating systems running on the same shared hardware resources. Now how is this possible? How are the operating systems protected from one another, and who decides who gets the resource and at what time? What we need then is an operating system of operating systems, something called a virtual machine manager or hypervisor.</li>
   <li>The terminology that you will often encounter is either VMM, which stands for virtual machine monitor, or hypervisor. And these operating systems that are running on top of the shared hardware resources are often referred to as virtual machines, or VM for short. We've always been using the term VM to mean virtual memory. You have to do a mind shift for this lesson module, VM means virtual machine. And each of these operating systems that run on top of the shared hardware resource, I'll often refer to them as either guest operating systems or virtual machines or VM for short. So watch out.</li>
   <li>Now, I should point out that there are two types of hypervisors.</li>
   <li>The first type is what is called a native hypervisor or bare metal, meaning that the hypervisor is running on top of the bare hardware. And that's why it's called a bare-metal hypervisor or a native hypervisor. And all of the operating systems that I'm showing you inside of the black box are running on top of this hypervisor. They're called the guest operating systems because they're the guest of the hypervisor running on the shared resource.</li>
   <li>The second type of hypervisor, what is called the hosted hypervisor. The hosted ones run on top of a host operating system and allows the users to emulate the functionality of other operating systems. So the hosted hypervisor is not running on top of the bare metal, but it is running as an application process on top of the host operating system. And the guest operating systems are clients of this hosted hypervisor. Some examples of hosted hypervisors include VMware, Workstation, and Virtual Box. Both of these terminologies you may have heard of. If you don't have access to a computer that's running Linux operating system in this course you're likely to be doing your course projects on a virtual box or a VMWare workstation that's available to run on Windows platform.</li>
   <li>For the purpose of this lesson today, however, we will be focusing on the bare metal hypervisors. These bare metal hypervisors interfere minimally with the normal operation of these guest operating systems on the shared resources in a spirit which is very similar to the extensible operating systems that we studied earlier, like the spin and exokernel and therefore the bare metal hypervisors, offer the best performance for the guest operating system on the shared resource.
   </li>
</ul>


<h2>6. Connecting the Dots</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151086365-fdc60937-70de-4b2a-823c-52b95dfc6883.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>If you're a student of history, you probably know that ideas come at some point of time and the practical use may come at a much later point of time. An excellent example of that is Boolean algebra, which was invented at the turn of the century as a pure mathematical exercise by George Boole. Now, Boolean algebra is the basis for pretty much anything and everything we do with computers. The concept of virtualization also had its beginning way back in time.</li>
   <li>It started with IBM VM 370 in the 60s and the early 70s. And the intent. And IBM VM370 was to give the illusion to every user on the computer as though the computer is theirs. That was the vision and it was also a vehicle for binary support for legacy applications that may run on older versions of IBM platforms.</li>
   <li>And then of course we had the microkernels that we have discussed in the earlier course module which surfaced in the 80s and early 90s.</li>
   <li>That in turn gave way to extensibility of operating systems in the 90s.</li>
   <li>The Stanford project SimOS in the late 90s, laid the basis for the modern resurgence of virtualization technology at the operating system level and in fact, was the basis for VMware.</li>
   <li>Even the specific ideas we're going to explore in this course module. Through Xen and VMware, are papers that date back to the early 2000s. They were proposed from the point of view of supporting application mobility, server consolidation, collocating, hosting facilities, distributed web services. These are all sort of the candidate applications for which Xen and VMware while positioning this virtualization technology.</li>
   <li>And now today, virtualization has taken off like anything. Why the big resurgence today? Well, companies want a share of everybody's pie. One of the things that has become very obvious, is the margin for device making companies in terms of profits, is very small. And everybody wants to get into providing services for end users. This is pioneered by IBM and others are following suit as well. So the attraction with virtualization technology is that companies can now provide resources with complete performance isolation and bill each individual user separately. And companies like Microsoft, Amazon, HP, you name it, everybody's in this game wanting to provide computing facilities through their data centers to a wide diversity of user communities. So that it's a win-win situation for both the users that do not want to maintain their own computing infrastructure and constantly upgrading them. And the companies like IBM that has a way of providing these resources on a rental basis, on a utility basis, to the user community. You can see the dots connecting up from the extensibility studies of the 90s like the spin and exokernel, to virtualization today, that is providing computational resources, much like we depend on utility companies to provide electricity and water.</li>
   <li>In other words, virtualization technology has made computing very much like the other utilities that we use to such as electricity and water and so on. And that's the reason why there's a huge resurgence in the user virtualization technology in all the data centers across the world. 
   </li>
</ul>


<h2>7. Full Virtualization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151086897-c9c07f8b-cd3e-4f0d-aa55-589f29048291.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>One idea for this virtualization framework is what is called full virtualization. And in full virtualization the idea is to leave the operating system pretty much untouched.So you can run the unchanged binary of the operating system on top of the hypervisor. This is called full virtualization because the operating system is completely untouched. Nothing has been changed. Not even a single line of code is modified in these operating systems in order to run on the hypervisor simultaneously. But we have to be a little bit clever to get this to work, however.</li>
   <li>Operating systems running on top of the hypervisor are run as user-level processes. They're not running at the same level of privilege as a Linux operating system that is running on bare metal. But if the operating system code is unchanged, it doesn't know that it does not have the privilege for doing certain things that it would do normally on bare metal hardware. In other words, when the operating system executes some privileged instructions, meaning they have to be in a privileged mode or kernel mode to run on bare metal in order to execute those instructions. Those instructions will create a trap that goes into the hypervisor and the hypervisor will then emulate the intended functionality of the operating system. And this is what is called the trap and emulate strategy.</li>
   <li>Essentially, each operating system thinks it is running on bare metal. And therefore, it does exactly what it would have done on a bare-metal processor, meaning that it'll try to execute certain privileged instructions thinking it has the right privilege. But it does not have the right privilege, because it's run as a user-level process on top of the hypervisor. And therefore, when they try to do something that requires a high level of privilege than the user level, it will result in a trap into the hypervisor, and the hypervisor will then emulate the intended functionality of the particular operating system.</li>
   <li>There are some thorny issues with this trap and emulate strategy of full virtualization, and that is, in some architectures, some privilege instructions may fail silently. What that means is, you would think that the instruction actually succeeded, but it did not. And you may never know about it. And in order to get around this problem, in fully virtualized systems, the hypervisor will resort to a binary translation strategy, meaning it knows what are the things that might fail silently in the architecture and looks for those gotchas in each of these individual binaries of the unmodified guest operating systems. And through binary editing strategy, they will ensure that those instructions are dealt with careful. So that if those instructions fail silently, the hypervisor can catch it and take the appropriate action. And this was a problem in early instances of Intel architecture. Both Intel and AMD have since started adding virtualization support to the hardware, so that such problems don't exist any more. But in the early going, when virtualization technology was experimented with, in the late 90's and the early 2000s, this was a problem that virtualization technology had to overcome in order to make sure that you can run operating systems as unchanged binaries on a fully virtualized hypervisor. Full virtualization is the technology that is employed in the VMware system.
   </li>
</ul>

<h2>8. Para Virtualization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087303-e71c4c92-dc94-4e20-8b1c-c20dd8358b8c.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Another approach to virtualization is to modify the source code of the guest operating system. If we can do that, not only we can avoid problematic instructions, as I mentioned earlier with full virtualization, but we can also include optimizations. For instance, letting the guest operating system, see real hardware resources, underneath the hypervisor, access to real hardware resources and also being able to employ tricks such as page coloring, exploiting the characteristics of the underlaying hardware. It is important to note, however, that so far as the applications are concerned, nothing is changed about the operating system because the interfaces that the applications see is exactly the interfaces provided by the operating system.</li>
   <li>If there is an application that is running on window, it sees the same API. If the application is running on top of Linux, it sees exactly the same API as it would if this Linux operating system was running on native hardware. In this sense, there's no change to the application's themselves. But, the operating system has to be modified in order to account for the fact that it is not running on bare metal, but it is running as a guest of the hypervisor. And this is why this technology is often referred to as Para Virtualization, meaning it is not fully virtualized, but a part of it is modified to account for being a guest of the hypervisor. The Xen product family uses this para virtualization approach. Now this brings up an interesting question. I said that, in order to do this paravisualization we have to modify the operating system, but how big is this modification?
   </li>
</ul>


<h2>9. Modification of Guest OS Code</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087470-8c07df3b-22b9-49d4-9ffd-ee18328caf5d.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087637-ffa876e4-ccdc-458f-b438-c3b18dd266fb.png" alt="drawing" width="500"/>
</p>


<h2>10. Para Virtualization (cont)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087677-b19fd790-8249-4d5b-ba09-c5affe8f6dc0.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>This table is showing you the lines of code that the designers of Xen hypervisor had to change in the native operating systems. The two native operating systems that they implemented on top of Xen hypervisor are Linux and Windows XP.</li>
   <li>And you see that, in the case of Linux, for instance, the total amount of the original code base that had to be changed is just about 1%, 1.36%.</li>
   <li>And in the case of XP, it is miniscule, almost in the noise.</li>
   <li>So in other words, even though, in para virtualization we have to modify the operating system to run on top of the hypervisor, the amount of code change that has to be done in the operating system in order to make it run on top of the hypervisor can be bound to a very small percentage of the total code base of the original operating system, which is good news.
   </li>
</ul>


<h2>11. Big Picture</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087855-39ac676a-bd1b-48f1-9aef-0344be23b9de.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>So, what is the big picture with virtualization? In either of the two approaches that I mentioned, whether it is full virtualization or parel virtualization, we have to virtualize the hardware resources and make them available safely to the operating systems that are running on top of the hypervisor.</li>
   <li>And when we talk about hardware resources, we're talking about the memory hierarchy, the CPU, and the devices that are there in the hardware platform. How to virtualize them and make them available in a transparent manner for use by the operating systems that live above the hypervisor? And how do we affect data and control transfer between the guest operating systems and the hyper-visor? So these are all the questions that we will be digging deeper into in this course module. That wraps up the basic introduction to virtualization technology. And now it is time to roll up our sleeves and look deeper into the nuts and bolts of virtualizing the different hardware elements in the hypervisor. 
   </li>
</ul>

# L03b: Memory Virtualization
<h2>1. Memory Virtualization Introduction</h2>
<ul>
   <li>We will now dig deeper into what needs to be done in the systems software stack to support virtualization of the hardware resources for use by the many operating systems living on top of the hardware simultaneously. As you've seen in the earlier course module on operating system extensibility,the question boils down to how to be flexible while being performance conscious and safe. </li>
   <li>You'll see that a bulk of our discussion on virtualization centers around memory systems. The reason is quite simple, namely, memory hierarchy is crucial to performance. Even efficient handling of device virtualization is heavily reliant on how memory system is virtualized and made available to the operating systems running on top of the hypervisor. So let's start with techniques for virtualizing the memory management in a performance-conscious manner.
   </li>
</ul>


<h2>2. Memory Hierarchy</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107374-37c39582-86b8-4b89-9e13-142b186defd3.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>I'm sure by now this picture is very familiar to you. Showing you the memory hierarchy going all the way from the CPU to the virtual memory on the disk. You have several levels of caches, of course, the TLB that holds address translations for you, virtual to physical, and you have the main memory and then the virtual memory on the disk. Now, caches are physically tagged and so you don't need to do anything special about them from the point of your virtualizing the memory hierarchy.</li>
   <li>The really thorny issue is handling virtual memory, namely the virtual address to the physical memory mapping, which is the key functionality of the memory management subsystem in any operating system.
   </li>
</ul>


<h2>3. Memory Subsystem Recall</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107518-87e99c8c-08b5-42fd-921c-a34bb0feb117.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Recall that in any modern operating system, each process is in it's own protection domain, and usually a separate hardware address space. The operating system maintains a page table on behalf of each of these processes. The page table is the operating system's data structure that holds the mapping between the virtual page numbers and the physical pages where those virtual pages are contained in the main memory of the hardware.</li>
   <li>The physical memory is contiguous starting from zero to whatever is the maximum limit of the hardware capabilities are. But the virtual address pace of a given processor is not contiguous in physical memory, but it is scattered all over the physical memory. And that's in some sense the advantage that you get with page based memory management, that a process notion of which virtual address being contiguous is not necessarily reflected in the physical mapping of those virtual pages to the physical pages in the main memory.
   </li>
</ul>


<h2>4. Memory Management and Hypervisor</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107671-ac933acf-6c3d-40b5-804a-8b2132688560.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>In the virtualized setup, the hypervisor sits between the guest operating system and the hardware. So the picture gets a little bit complicated. Inside each one of these operating systems, they are user-level processes, and each process is in its own protection domain. What that means is that in each of these operating systems, there is a distinct page table that the operating system maintains on behalf of the processes that are running in that operating system, similarly in this case.</li>
   <li>Does the hypervisor know about the page tables maintained on behalf of the processes that are running in each one of these individual operating systems? The answer is no. Windows and Linux are simply protection domains distinct from one another so far as the hypervisor is concerned. The fact that each of Windows and Linux within them contain application of a processes is something that the hypervisor doesn't know about at all.
   </li>
</ul>


<h2>5. Memory Manager Zoomed Out</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107791-a6b93f81-5a27-4dee-8b15-522ba97c4faf.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>So far as each one of these operating systems is concerned, in their native form, when they think about physical memory, they think of physical memory as contiguous.But, unfortunately, the real physical memory, we're going to call it machine memory from now on, is in the control of the hypervisor. Not in the control of any one operating system.</li>
   <li>So, the physical memory of each of these operating systems is itself an illusion. It's no longer contiguous in terms of the real machine memory that the hypervisor controls. So for instance, if you look at the physical memory that windows has, I've broken that down into 2 regions, R1 and R2. And R1 has, let's say, a number of pages 0 through Q, and R2 has a number of pages starting from Q plus 1 through N. So this is a physical memory. But if you look at this region R1, it's occupying a space in the machine memory, the real physical memory, which we call the machine memory controlled by the hypervisor over here. R2 is not contiguous with respect to R1 in the machine memory. It's been put over here. And come over to Linux, and it has its own physical memory, and we'll call that, region 1 and region 2 again. And it has a total capacity of M + 1 physical page frames, all of which zero through L are contiguous in the machine memory and L+1 through M are contiguous in machine memory, but, they're not contiguous with respect to the other region R1.</li>
   <li>Why would this happen? The hyperwaves is not being nasty to the operating system. After all, even if all of the N plus 1 pages of Windows, and N plus 1 pages of Linux would be contiguous in the machine memory, they cannot all start at 0 because the physical memory, the real physical memory, which we're calling machine memory, is the only thing that is contiguous. And that has to be partitioned between these two guys, and therefore, the starting point for the physical memory, the illusion of the physical memory, so far as a particular operating system, cannot be 0. Also, the memory requirements of operating systems are dynamic and bursting. When Windows started out initially with Q plus 1 pages, and later on it needed additional memory, then it's going to request the hypervisor and at that point, it may not be possible for the hypervisor to give another region, which is contiguous with the previous region that Windows already has. So these are the reasons why the physical memory of an operating system itself becomes an illusion, in terms of how it is actually contained in the machine memory.
   </li>
</ul>


<h2>6. Zooming Back In</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107860-6132c860-a753-4887-b356-03a9f5f2987d.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Zooming back in to what is going on within a given operating system, we already know that the process address space for an application is an illusion in the sense that the virtual memory space of this process is contiguous, but in terms of physical memory they are not contiguous. And the page table data structure that the operating system maintains, on behalf of the processes, is the one that supports this illusion so far as this process is concerned. By mapping the virtual page number of the process, using the page table for this particular process into the physical page number, where a particular virtual page may be contained in physical memory. This is a setting in a non-virtualized operating system. So the page table serves as the broker to convert a virtual page number to a physical page number.</li>
   <li>In a virtualized setting, we have another level of indirection, that is this physical page number of the operating system has to be mapped to the machine memory or the machine page numbers. We'll call the pages in the machine memory, which is the real thing, as MPN, short for Machine Page Numbers. And this goes from zero through some max, which is the total memory capacity that you have in your hardware. This data structure, the page table, is a traditional page table, that gives a mapping between virtual page number and the physical page number. And this is a traditional page table. The mapping between the physical page number and the machine page number, that is PPN to MPN mapping, is kept in another page table, which is called shadow page table "S-PT".</li>
   <li>So now, in a virtualized setting, there's a two-step translation process to go from VPN to MPN. The page table maintained by the guest operating system is the one that translates VPN to PPN, and then there is this additional page table called a shadow page table that converts the PPN to MPN. That brings up an interesting question.
   </li>
</ul>


<h2>7. Who Keeps PPN MPN Mapping</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107913-b7a8b9f4-787b-4134-aaa8-4b6ffaa1ad0b.png" alt="drawing" width="500"/>
</p>


<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151113193-febcb7fa-6abf-4829-9e2c-0bb52db0f88f.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>In the case of a fully virtualized hypoviser, the guest operating system has no knowledge of machine pages. It thinks that it's physical memory is contiguous because it is thinking that it is running on bare metal, nothing has been changed in the operating system to run on top of a hypervisor in a fully virtualized setting. And therefore, it is the responsibility of the hypervisor to maintain the PPN to MPN mapping.</li>
   <li>In a para virtualized setting, on the other hand, the guest operating system knows that it is not running on bare metal. It knows that there's a hypervisor in between it and the real hardware. And it knows, therefore, that its physical memory is going to be fragmented in the machine memory. So the mapping PPN to MPN can be kept in either the guest OS or the Hypervisor. But, usually, it is kept in the guest operating system. We'll talk more about that later on.
   </li>
</ul>

<h2>8. Shadow Page Table</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151107985-cc40f232-506c-477d-9888-885dff7a7db0.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Let's understand what exactly this shadow page table is and what it is.</li>
   <li>In many architectures, for example, Intel's X86 family, the CPU uses the page table for address translation. What that means is presented with the virtual address, The CPU gets to what? It first looks up the TLB to see if there is a match for the virtual page number contained in this virtual address. If there is a match, it's a hit and it can translate this virtual address to the physical address. If it is a miss, CPU knows word in memory the page table data structure is kept by the operating system. And therefore what it does, it goes to the page table, which is in main memory, and retrieves the specific entry which will give it the translation from the virtual page number to the physical page number. And once it gets that, It'll stash it in the TLB, as well, and be able to generate the physical address that is specified by this particular virtual address. So that's the way the CPU does the translation in many architectures. So in other words, both the TLB and the page table are data structures that the architecture uses for address translation. Page table is also a data structure that is set by the operating system for enabling the processor to do this translation. So in other words, the hardware page table is really the Shadow page table in the virtualized setting, if the architecture is going to use the page table for address translation.
   </li>
</ul>


<h2>9. Efficient Mapping (Full Virtualization)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108049-b273dac5-490e-42d8-9f62-96dda191ee92.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>As I mentioned in a fully virtualized setting, the guest operating system has no idea about machine pages. It thinks that the physical page number that it is generating, is the real thing. But it is not. And therefore, there is two levels of indirection, one level of indirection going from virtual page to physical page, that's an illusion. Then the hypervisor has to take this physical page and using the shadow page table, convert it into the machine page. And as I said, the shadow page table may be the real hardware page table that the CPU uses as well. So this is the data structure that is maintained by the hypervisor to translate the PPN to MPN.</li>
   <li>So, how to make this efficient? Because on every memory access of a process of the guest operating system, the virtual address has to be converted to a machine address. So in other words, we want to avoid the one level of indirection that's happening because the virtual page number has to be converted to a physical page number by the guest operating system, and then it has to be looked up in the shadow table by the hypervisor to generate the machine page number.So we would like to make this process more efficient because it's happening on every memory access by getting rid of one level of indirection, that is, this translation by the guest operating system. </li>
   <li>Remember that, the guest operating system is the one that establishes this mapping in the first place between a virtual page number and a physical page number. By creating an entry in the page table for the process that is generating this virtual address in the first place. And updating the page table is a privileged instruction. So, when the guest operating system tries to update the page table or what it thinks is the page table, it'll result in a trap. Any time the guest operating system tries to update this page table to establish a mapping between VPN and PPN, there'll be a trap. </li>
   <li>And this trap is called by the hypervisor. And what the hypervisor is going to do is, it's basically going to say, oh, this particular VPN corresponds to a specific entry in the shadow page table. So the update that guest OS is making to it's page table data structure, which it thinks is the real thing. It's not the real thing. The hypervisor is updating the same mapping by saying" well, this particular VPN is going to this machine page number, that's the one that we're going to put as the entry here."</li>
   <li>So as a result, what we have done is, even though there is a solution, the real translations, that is a mapping between VPN and MPN, is remembered in the shadow page table, which may be the hardware page table if the processor is using the page table for its address translation or it could be the TLB. In either case, the hypervisor is going to install the translation from VPN to MPN into the TLB and the hardware page table.</li>
   <li>So now we basically bypassed the guess operating system's page table in order to to the translation because every time a process generates a virtual address. We are not going through the guest operating system to do the translation, but so long as the translation has already been installed in the TLB and the hardware page table. The hypervisor without the intervention of the guest operating system can translate the virtual page number of a user level process running on top of the guest operating system directly to the machine based number using the TLB and the hardware page table.</li>
   <li>So this is a trick to make address translation efficient, because it's extremely crucial that on every memory access we don't go through the guest top rating system to do the address translation, it's just not acceptable. And this is the trick that is used in VMware ESX server that is implemented on top of Intel hardware.
   </li>
</ul>


<h2>10. Efficient Mapping (Para Virtualization)</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108137-bbed6351-acdb-452f-bf76-ebaee255a65b.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>In a para virtualized setting, on the other hand, the operating system knows that its physical memory is not contiguous. And there for this burden of efficient mapping can be shifted into the guest operating system itself.</li>
   <li>So now the guest operating system is going to maintain contiguous physical memory to make it simpler in terms of all the other subsystems to do that. But it is also going to know that its notion of physical memory, is not the same as machine memory, and so it will map the discontiguous physical memory to real hardware pages. So that burden of doing the PPN to MPN mapping can be pushed into the guest operating system in a very virtualized setting.</li>
   <li>So on an architecture like Intel, where the page table is a data structure of the operating system. And it is also used by the hardware to do the address translation. The responsibility of allocating and managing the hardware page table data structure can be shifted into the guest operating system. In a fully virtualized setting, it's not possible to do that because the operating system in a fully virtualized setting is unaware of the fact that it is not running on bare metal. But in a para virtualized setting, since it is possible to do that, it is more efficient to push this efficient mapping handling into the guest operating system.</li>
   <li>For example, in Xen which is a para virtualized hypervisor, it provides a set of Hypercalls for the guest operating system to tell the Hypervisor about changes to the hardware page table. So, for instance, there is a call that says create page table and this allows a guest operating system to allocate and initialize a page frame that it has previously acquired a real page frame that it is previously acquired from the hypervisor as a hardware resource. It can target that physical page frame as a page table data structure. Recall that each guest operating system, would have gotten a bunch of physical memory from the hypervisor at the beginning of establishing its foot print on the hypervisor. And so it can use one of those real physical memories to host a page table data structure, on behalf of a new process that it launches now. So anytime a new process starts up in the guest operating system, the guest operating system will make a hypercall to Xen saying "Please create a page table for me, and this is the page frame that I'm giving you to use as the page table". So, when the guest operating system has to operate this particular process which got launched, then it can make another hypercall to the hypervisor saying "please switch to page table, and here is the location of the page table". The hypervisor doesn't know about all these processes. All it understands is that there's a hypercall that says change the page table from whatever it used to be to this new page table. And that essentially results in this guest operating system, switching the address space of the currently running process on the the bare hardware on the bare metal to P1 by this switch page table hypercall. Xen will do that appropriate thing, of setting the hardware register, of the processor to point to this page table data structure. In response to this hypercall from the guest operating system. If the process P1 one were to page fault at some point of time, the page fault would be handled by the guest operating system. We'll talk about how it does that later on. But once it handles that page fault for P1 and says, "oh, this particular virtual page of this process is now going to correspond to a physical frame that I own. I'm going to tell the hypervisor that the mapping in the page table, has to be set for this translation that I just established for the faulted page for this process". So there's another hypercall that's available for updating a given page table data structure. And using this the guest operating system can deal with modifications to the base table data structure.</li>
   <li>All the things that an operating system would have to do in a normal setting on bare metal, you have to be able to do in the setting where you have the hypervisor sitting between the real hardware and the guest operating system. And the three things that are required to be done in the conflicts of memory management in a para virtualized setting, is being able to create a brand new hardware address space for a newly launched process which involves creating a page table. That's a hypercall that's available. When you do a conflict switch, you want to switch the page table. That's a hypercall that's available when you do a conflict switch in the guest operating system from P1 to P2, the guest can tell the hypervisor that the page table to use from now on is such and so. That's the way the guest can do a contact switch from one process to another. And, thirdly, since not all of the address space or the memory footprint of a process would be physical memory, if the currently running process were to incur at page four, that has to be handled by the guest operating system. In handling that, it establishes a mapping between the messy virtual page for this process and the physical frame in which the contents of the page is now contained. That mapping has been put into the page table for this particular process. Again that's something that only the hypervisor can do, because it is a privileged operation happening inside the hardware. And for that purpose, the hypervisor provides a hyper call that allows a guest operating system to update the base table data structure. So at the outset I said that handling virtual memory is a thorny issue. Doing the mapping from virtual to physical on every memory access, with all the intervention of the guest operating system is the key to good performance. And it can be done both in fully virtualized and paravirtualized setting by the tricks that we talked about just now.
   </li>
</ul>


<h2>11. Dynamically Increasing Memory</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108372-eb740aea-34f7-4ae0-a8c6-1c6138581250.png" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108397-fee33d4e-961d-4c53-965d-7104592b38eb.png" alt="drawing" width="500"/>
</p>


<ul>
   <li>The next thing we are going to talk about is how can we dynamically increase the amount of physical memory, that's available to a particular operating system running on top of the hypervisor? As I mentioned, memory requirements tend to be bursty, and therefore. The hypervisor has to be able to allocate real physical memory or machine memory on demand to the requesting guest operating systems on top of it. Let's look at this picture here. Let's assume that this is the total amount of machine memory that's available to the hypervisor. And the hypervisor has divvied up the available machine memory among these two operating systems. So the house, meaning the hypervisor has no spare memory at all. It is completely divided up and given to these two guys. What if the windows operating system experiences a burst in memory usage, and therefore its Requiring more memory from the hypervizor. It may happen because of some resource hungry application that had been started up in windows, maybe a video streaming application, that is gobbling up a lot of physical memory, and therefore windows needs more memory and comes to the hypervizor asking for additional hardware resources. But unfortunately, the bank is empty. What the hyper-visor could do is recognize that. Well maybe, this operating system doesn't need all of the physical resources I allocated it. So I'm going to grab back, a portion of the physical memory that Linux has. And once I get back this portion of physical memory that I previously allocated to Linux, I can then give it to Windows to satisfy its sudden hunger for more memory. Well, this principle of robbing Peter to pay Paul. Can lead to unexpected and anomalous behavior, of applications running on the guest operating system. A standard approach of course would be to coach one of the guest operating system in this case perhaps Linux, to give up some of its physical memory voluntarily, To satsify the needs of a peer that is currently experiencing memory pressure.
   </li>
</ul>


<h2>12. Ballooning</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108420-d9b947d3-f560-4593-8746-649c5a89b4e0.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>That's the idea behind a technique that I'm going to describe to you called ballooning. The idea is to have a special device driver installed in every guest operating system. So even if it is a fully virtualized setting, since device drivers can be installed on the fly, with the cooperation of the guest operating system. The hypervisor can install the device driver, which is called a balloon, in the operating system. And this balloon device driver is the key for managing memory pressures that maybe experienced by a virtual machine or a desktop operating system. In a virtualized setting. Let's say that the house needs more memory suddenly. And this may be a result of another guest operating system, saying that it needs more memory. So what the hypervisor would do, is It'll contact one of the guest operating system that currently is not actively using all of its memory, let's say. And talk to this balloon driver that it has installed through a private channel that exists between the hypervisor and the balloon driver. So this is something that only the hypervisor knows about because this is a special device driver that the hypervisor has installed. It knows how to get to this device driver and tell it to do something. And in this case, what the hypervisor is going to do is tell this balloon device driver to inflate. What that means is that this balloon device driver is going to make requests to the guest operating system saying I need more memory. I need more memory, I need more memory. And the guest operating system will of course, give this balloon driver the memory that it is requesting. And since the amount of physical memory that's available to the guest operating system is finite. If one process, in this case, the balloon driver, is making requests from the guest saying give me more memory. Guest has to necessarily make room for that by paging out to the disk unwanted pages from the total memory footprint of all the processes that are currently running in this guest operating system. And once it is done the swapping out of pages, and perhaps even entire processes out onto the disc. That's the way it can make room for the request that is coming from this balloon driver. Now, once the balloon driver has gotten all this extra memory out of this guest operating system. It can return those physical memories. The real physical memory, or the machine memory, back to the hypervisor. So we started with this house needing more machine memory. And the way, the house got that more machine memory is by contacting its balloon driver installed in one of the guest operating systems that is not actively using all of its resources. Asking the balloon to inflate and inflating the balloon is essentially meaning that we are acquiring more memory from the guest operating system. So, you can see visually that the footprint of the balloon goes up because of the inflation. That means, it's got a bigger memory footprint. And all of this memory footprint has extra resources that it can simply return to the hypervisor. It's not going to use it. It just wants to get it so that it can return it to the hypervisor. So that's this path here. So the opposite of what I've just described is a situation where the house needs less memory. Or, in other words, it has more memory to give away to guest operating system. So maybe it is this guest that requested more memory in the first place, And in that case, it wants the guest to get to the memory that it wanted. And the way it can do that is as follows. Once again the hypovisor through it's private channel will contact the balloon driver and tell the balloon driver to deflate the balloon. By deflate what is being instructed to the balloon driver is to contract its memory footprint. So, if it contracts its memory footprint by deflating, it is actually releasing memory into the guest operations. So, available physical memory. In the guest operating system is going to increase, because of the fact that the balloon is deflating and giving out the memory that is currently sitting on. So now the guest operating system has more memory to play with. That's the effect of the balloon deflating is that the guest operating system has more memory. Which means that it can page in from the disk, the working set of processes that are executing on this guest operating system. So that those processes can have more memory resources than they've been able to because of the balloon occupying a lot of the memory. So, this technique of ballooning assumes cooperation with the guest operating system. So that based on the need of the hour the hypervisor can work with the guest operating system implicitly. Without the guest really knowing about it because it is all happening through the balloon driver that has been installed. By the hypervisor in each of the guest operating systems. And this technique of ballooning is applicable to both fully virtualized and para-virtualized environments to ease the over commitment of memory by the hypervisor to the guests. I mean, it's sort of line airline reservations. You always Notice that in airline reservation, the airlines tend to sell more seats than what they have with the hope that someone is not going to show up. That's the same sort of thing that is being done here that there is a finite amount of physical resources available with the hypervisor. And it is doling it out to all the guest operating systems. >> And what it wants to do is, it wants to be able to reacquire some of those resources to satisfy the needs of another guest operating system that is experiencing a memory pressure at any point of time.
   </li>
</ul>


<h2>13. Sharing Memory Across Virtual Machines</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108491-75127ba4-482d-41f4-a344-513fe9f39a3b.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Memory is a precious resource. You don't want to waste it. You want protection for the virtual machines from one another. But at the same time if there's an opportunity to share memory so that the available physical resource can be maximally utilized you want to be able to do that. So the question is can we share memory across virtual machines without affecting the integrity of these machines? Because protection is important, but without affecting the protection, can we actually enhance the utility of the available physical memory in the hyperviser? And the answer is yes, think about it. You may have one instance of Linux contained in a virtual machine, maybe hosted by me. And maybe you have the same Linux hosted in another virtual machine, virtual machine number two. And in both of our virtual machines, the memory footprint of the operating system is exactly the same. And even the applications that run on it are going to be exactly the same, in terms of the contents. Because of the same operating system, it is just that we have two different instances. And let's say that we have Firefox running on this VM, and similarly this Firefox running on this VM. This copy of Linux is going to have a page table, unique for this particular Firefox instance. Similarly, this instance of Linux is going to have a piece-table deal structure unique for this instance of Firefox and if all things are equal, in terms of versions and so on, then a particular virtual page of, the Firefox instance running here and the Firefox instance running here is going to have the same content, whether we are talking about this VM or this VM. And so there is an opportunity to make both of those page table entires to point to the same machine page. If we can do that, then we are avoiding duplication. We're not comprimising safety, but we're avoiding duplication. This is particularly true for the core pages. The core pages are immutable. So, the core pages for this Firefox instance and this Firefox instance could actually share the same page in physical memory. And if it does that you're using the resources taht much more effectively in a virtualized setting. One way of doing the sharing is by a cooperation between the virtual machines and the hypervisor. In other words the guest OS has hooks that allows the hypervisor to mark pages, copy on write, and have the PPNs point to exactly the same machine page. And this way if a page is going to be written into by the operating system, it'll result in a fault and we can make a copy of the page that is currently being shared across two different virtual machines. That is one way of doing it. That is, with the cooperation between the hypervisor, and the virtual machines. So that we can, with their connivance, share machine pages across virtual machines, but mark the entries in the individual page tables as copy on write, meaning that, so long as they reach here, perfectly fine. The minute any one of these guys wants to write into a particular page, at that point, you make a copy of it and make these two instances point to different machine pages, so that is one way of doing it.
   </li>
</ul>


<h2>14. VM Oblivious Page Sharing</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108541-9af73075-a409-4e52-8a91-4fca7f45a354.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>An alternative is to achieve the same effect, but completely oblivious to the guest operating system, and this is used in VMware ESX server. The idea is to use content-based sharing. And in order to support that, VM Ware has a data structure which is a hash table kept in the hypervisor. And this hash table data structure contains a content hash of the machine pages. So, for instance, this entry is saying that. For virtual machine number three. For it's physical page which is at this address 43f8 there is a machine page that hosts this physical page number of VM3 and that's contained in machine page number 123b and the content hash that is... If you hash the contents of this memory page, you get a signature. That signature is the content hash. That content hash is stored in this data structure. Now let's see how this data structure is used for doing VM-oblivious page sharing in the ESX Server. We want to know if there is some page in VM2 which is content wise the same as this page contained in VM3. In particular we want to know if this physical page number.; PPN 2868 of VM 2, which is mapped to this machine page,1096 is content-wise the same as this guy. So how do we find that out? What we do is we take the contents of this machine page 1096 Create a content hash. So that's the content hash that you're going to generate, a particular algorithm that the hypervisor is going to run to create a content hash. So we create a content hash for this page 1096, that corresponds to PPN 2868 of VM 2. Now we take this content hash and look through hypervisor's data structure to see if there is a match between this content hash that I created for this page and any page currently in the machine memory. Well we have a match. We have a match between the content hash for this page and the content hash (no period) Of the page comtained in VM 3, 43f8, which is mapped to MPN 123b. So now we've got this match. Can we know that this page and this page are exactly the same? Well, we cannot. It's only a hint at this point that this pages content hash is the same as this because this content hash for 123b was taken at some point of time. And we created this data structure to represent this as a hint frame, which has a particular content hash. Now, VM3 could have been using this page actively and modified it, and if it has modified it, then this content hash that we have in this data structure may no longer be valid. And therefore, even though we've got a match, it's only a hint, not an absolute. It's only a hint. So once we have a match, then we want to do a full comparison between these two guys. Make sure that these two guys are exactly the same, full comparison upon match.
   </li>
</ul>


<h2>15. Successful Match</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108571-a5949249-c07b-42ee-a916-2ea8c8fb44be.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>If the content hash of 1096 and 123b are exactly the same, then we can modify the PPN to MPN mapping in VM2 for the page 2868, which used to point to 1096, we can now make it, point to 123b because they both are exactly the same content. And once we have done that, then we increment the reference count to this hash table entry to 2, indicating that there are 2 different virtual machines that map to the same machine page, 123b. And, we're also going to remember that these two mappings. Between PPN 2868 to 123b, 43f8 to 123b are copy on write entries, indicating that they can share this page, so long as these 2 virtual machines are reading it. But if any one of them tries to write it, at that point For the integrity of the system, you have to make a copy of this page and change the mappings for those PPNs to go to different MPNs, that's the reason that we want to do this copy on write. And now, we can free-up page number 1096. So there is one more page frame that's available for the house, in terms of allocation. because all of these things that I mentioned just now, are fairly labor-intensive operations. So you don't want to do this when there is active usage of the system. So, scanning the pages, that is. Going through all of a virtual machine's pages to see if pages that are contained in a virtual machine may already be present in the machine memory reflecting the contents of some of the virtual machine. That kind of scanning you want to do it as a background activity of the server, when it is lightly loaded. Looking for such matches and mapping the virtual machines to share the same machine page and freeing up machine memory for allocation by the hypervisor. And the important thing that you have to notice is that, as opposed to the earlier mechanism that I mentioned where I said that with the coninements of the guest operating system, the hypervisor can get into the page table data structures inside the guest operating systems. No such thing here. It is completely done oblivious to the guest operating systems and therefore, there is no change that needs to be made to the guest operating systems. In order to do the sharing in an oblivious way. And this technique is applicable to both fully virtualized, as well as the paravirtualized environments. Because basically, all that we are saying is that, let's go through the memory contents of a virtual machine, and see if. The memory contents of the virtual machine, any particular page frames can be shared with other virtual machines, and if so, let's do that. And free up some memory. That's the idea, can be applied to both fully virtualized and para virtualized settings.
   </li>
</ul>


<h2>16. Memory Allocation Policies</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108601-832e1b98-c4d7-44c9-a452-134edeac7ab9.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Up to now, we talked about mechanisms for virtualizing the memory resource. In particular for dealing with dynamic memory pressure and sharing machine pages across VM's. A higher level issue is the policies we have to use for allocating and reclaiming memory from the domains to which we've allocated them in the first place. Ultimately, the goal of virtualization is maximizing the utilization of the resources. Memory is a precious resource. Virtualized environments may use different policies for memory allocation. One can be a pure share based policy. The idea here is, you pay less, you get less. That's the idea. So if you have a service level agreement with the data center, then the data center gives you a certain amount of resources based on, the more of dollars you put on a table. So that is a pure share based approach. The problem with a share based approach, is of course the fact that it could lead to hoarding. If a virtual machine gets a bunch of resources and it's not really using it, it's just wasting it. Now, the desired behavior is if the working-set of a virtual machine goes up, you give it more memory. If it's working-set shrinks, get back the memory so that you can give it to somebody else. So working-set-based approach would be the saner approach. But at the same time, if I paid money, I need my resources. So, one thing that can be done is sort of put these two ideas together in implementing a dynamic idle-adjusted shares approach. In other words, you're going to tax the guys that are hoarders, so you tax the idle pages more than active pages. If I've given you a bunch of resources, if you're actively using it, more power to you. But if you're hoarding it, I'm going to tax you. I'm going to take away the resources that I gave you. And you may not even notice it because you're not using it anyway. So that's the idea in this dynamic idle-adjusted shares approach. And now what is this tax? Well, we could make the tax rate 0%, that is plutocracy, meaning you paid for it, you got it, you can sit on it, I'm not going to tax you, that's one approach. Or, I could make the tax 100%, meaning that if you've got some resources and you're not using it, I'm going to take all of it away. [LAUGH] So that's the wealth redistribution. Sort of a socialistic policy, use it or lose it. And in other words, if you make the tax 100%, we are ignoring shares altogether. Now something in between is probably the best way to do it, so for instance if you use a tax rate of 50% or 75% saying if you have idle pages, then the tax rate is 50%, there's a 50% chance I'll take it away. And that's what is being done in the VMware ESX server today in terms of how to allocate memory to the domains that need it. You are going to use share based approach, but if you're not actively using it, we're going to take it away from you. And of course we'll give it back to you if you start using it, and that's the reason why you don't want to make the tax 100%, but have it somewhat smaller. So by having a tax rate that is not quite 100% but maybe 50% or 75%, we can reclaim most of the idle memory from the VM's that are not actively using it. But at the same time, since we're not taxing idle pages 100%, it allows for sudden working set increases that might be experienced by a particular domain. Suddenly, a domain starts needing more memory, at that point it may still have some reserves in terms of the idle pages that I did not take away from that particular domain. So the key point is that, you don't want to tax at 100% because this allows for sudden working set increases that may be there, in a virtual machine that happens to be idle for some time, but suddenly work picks up.
   </li>
</ul>

# L03c: CPU & Device Virtualization
<h2>1. CPU & Device Virtualization Introduction</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151108852-3a3a837e-4786-49c8-aea8-1d6f636ae427.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>As we said at the outset of this course module, virtualizing the memory subsystem in a safe and performance conscious manner is the key to the success of virtualization. What remains to be vertualized are the CPU and the devices. That's what we'll talk about next. Memory virtualization is sort of under the covers. When it comes to the CPU and the devices, the interference among the virtual machines living on top of the hypervisor becomes much more explicit. The challenge in virtualizing the CPU and the devices is giving the illusion to the guest operating systems living on top, that they own the resources. Protected and handed to them by the hypervisor, on a need basis. There are two parts to CPU virtualization, we want to give the illusion to each guest operating system, that it owns the CPU, that is; it does not even know the existence of other guests on top the same CPU If you think about it, this is not very far removed from the concerns of a time shared operating system, which has to give the illusion to each running process that that process is the only one running on the processor. The main difference in the virtual setting is that this illusion is being provided by the hypervisor at the granularity of an entire operating system. That's the first part. The second part is we want the hyperviser to field events arising due to the execution of a process that belongs to a parent guest operating system, in particular during the execution of a process on the processor. There are going to be discontinuities that occur. And those program discontinuities, have to be fielded by the hypoviser, and passed to right guest operating system.
   </li>
</ul>




<h2>TBC</h2>
<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>
   </li>
</ul>
