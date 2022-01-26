# Lesson outline
- [Intro to Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03a-intro-to-virtualization)
- [Memory Virtualization](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L03_Virtualization.md#l03b-memory-virtualization)
- CPU & Device Virtualization

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
   <li>The second type of hypervisor, what is called the hosted hypervisor, the hosted ones run on top of a host operating system and allows the users to emulate the functionality of other operating systems. So the hosted hypervisor is not running on top of the bare metal, but it is running as an application process on top of the host operating system. And the guest operating systems are clients of this hosted hypervisor. Some examples of hosted hypervisors include VMware, Workstation, and Virtual Box. Both of these terminologies you may have heard of. If you don't have access to a computer that's running Linux operating system in this course you're likely to be doing your course projects on a virtual box or a VMWare workstation that's available to run on Windows platform.</li>
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
   <li>One idea for this virtualization framework is what is called full virtualization, and in full virtualization the idea is to leave the operating system pretty much untouched. So you can run the unchanged binary of the operating system on top of the hypervisor. This is called full virtualization because the operating system is completely untouched. Nothing has been changed. Not even a single line of code is modified in these operating systems in order to run on the hypervisor simultaneously. But we have to be a little bit clever to get this to work, however. Operating systems running on top of the hypervisor are run as user-level processes. They're not running at the same level of privilege as a Linux operating system that is running on bare metal. But if the operating system code is unchanged, it doesn't know that it does not have the privilege for doing certain things that it would do normally on bare metal hardware. In other words, when the operating system executes some privileged instructions, meaning they have to be in a privileged mode or kernel mode to run on bare metal in order to execute those instructions. Those instructions will create a trap that goes into the hypervisor and the hypervisor will then emulate the intended functionality of the operating system. And this is what is called the trap and emulate strategy. Essentially, each operating system thinks it is running on bare metal. And therefore, it does exactly what it would have done on a bare-metal processor, meaning that it'll try to execute certain privileged instructions thinking it has the right privilege. But it does not have the right privilege, because it's run as a user-level process on top of the hypervisor. And therefore, when they try to do something that requires a high level of privilege than the user level, it will result in a trap into the hypervisor, and the hypervisor will then emulate the intended functionality of the particular operating system. There are some thorny issues with this trap and emulate strategy of full virtualization, and that is. In some architectures, some privilege instructions may fail silently. What that means is, you would think that the instruction actually succeeded, but it did not. And you may never know about it. And in order to get around this problem, in fully virtualized systems, the hypervisor will resort to a binary translation strategy, meaning. It knows what are the things that might fail silently in the architecture. Look for those gotchas in each of these individual binaries of the unmodified guest operating systems. And through binary editing strategy. They will ensure that those instructions are dealt with careful, so that if those instructions fail silently, the hypervisor can catch it and take the appropriate action. And this was a problem in early instances of Intel architecture. Both Intel and AMD have since started adding virtualization support to the hardware, so that such problems don't exist any more. But in the early going, when virtualization technology was experimented with, in the late 90's and the early 2000s, this was a problem that virtualization technology had to overcome in order to make sure that you can run operating systems as unchanged binaries on a fully virtualized hypervisor. Full virtualization is the technology that is employed in the vmware system.
   </li>
</ul>


<h2>8. Para Virtualization</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087303-e71c4c92-dc94-4e20-8b1c-c20dd8358b8c.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>Another approach to virtualization is to modify the source code of the guest operating system. If we can do that, not only can we avoid problematic instructions, as I mentioned earlier with full virtualization, but we can also include optimizations. For instance, letting the guest operating system, see real hardware resources, underneath the hypervisor, access to real hardware resources and also being able to employ tricks such as page coloring. Exploiting the characteristics of the underlaying hardware. It is important to note, however, that so far as the applications are concerned, nothing is changed about the operating system because the interfaces that the applications see is exactly the interfaces provided by. The operating system, if there is an application that is running on window, it sees the same API. If the application is running on top of Linux, it sees exactly the same API as it would if this Linux operating system was running on native hardware. In this sense, there's no change to the application's themselves. But, the operating system has to be modified in order to account for the fact that it is not running on bare metal, but it is running as a guest of the hypervisor. And this is why this technology is often referred to as Para Virtualization, meaning it is not fully virtualized, but a part of it is modified to account for being a guest of the hypervisor. The Zen product family uses this para virtualization approach. Now this brings up an interesting question. I said that, in order to do this paravisualization we have to modify the operating system, but how big is this modification?
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
   <li>This table is showing you the lines of code that the designers of Xen hypervisor had to change in the native operating systems. The two native operating systems that they implemented on top of Xen hypervisor are Linux and Windows XP. And you see that, in the case of Linux, for instance, the total amount of the original code base that had to be changed is just about 1%, 1.36%. And in the case of XP, it is miniscule, almost an annoyance. So in other words, even though, in para virtualization we have to modify the operating system to run on top of the hypervisor, the amount of code change that has to be done in the operating system in order to make it run on top of the hypervisor can be bound to a very small percentage of the total code base of the original operating system. Which is good news.
   </li>
</ul>


<h2>11. Big Picture</h2>
<p align="center">
   <img src="https://user-images.githubusercontent.com/62491253/151087855-39ac676a-bd1b-48f1-9aef-0344be23b9de.png" alt="drawing" width="500"/>
</p>

<ul>
   <li>So, what is the big picture with virtualization? In either of the two approaches that I mentioned, whether it is full virtualization or parel virtualization, we have to virtualize, the hardware resources and make them available safely to the operating systems that are running on top of the hypervisor. And when we talk about hardware resources, we're talking about the memory hierarchy, the CPU, and the devices that are there in the hardware platform. How to virtualize them and make them available in a transparent manner for use by the operating systems that live above the hypervisor? And how do we affect? Data and control transfer between the guest operating systems and the hyper-visor. So these are all the questions that we will be digging deeper into in this course module. That wraps up the basic introduction to virtualization technology. And now it is time to roll up our sleeves and look deeper into the nuts and bolts of virtualizing the different hardware elements in the hypervisor. 
   </li>
</ul>

# L03b: Memory Virtualization
TBC
<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="500"/>
</p>
