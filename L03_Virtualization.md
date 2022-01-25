# Lesson outline
- [Intro to Virtualization]()
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
   <li> tbc
   </li>
</ul>

TBC
<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="500"/>
</p>
