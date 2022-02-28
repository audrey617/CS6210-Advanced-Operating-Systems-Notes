# Lesson outline
- [L05a: Definitions](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L05_Distributed%20Systems.md#1-definitions-introduction)
- [L05b: Lamport Clocks](-)
- [L05c: Latency Limits](-)
- [L05d: Active Networks](-)
- [L05e: Systems from Components](-)


# L05a: Definitions
<h2>1. Definitions Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/1.JPG?raw=true" alt="drawing" width="500"/>
</p>
<ul>
  <li>We now launch into the study of distributed systems. This is an exciting field. Part of the fun, but there's a lot of parallels between distributed systems and parallel systems. What fundamentally distinguishes a distributed system from a parallel system is the individual autonomy for the nodes of a distributed system as compared to a parallel system, and the fact that the interconnection network that connects all the nodes in a distributed system is wide open to the world, as opposed to being confined within a rack, or a room or a box. However, as the feature size of transistors in silicon continues to shrink due to advances in process technology and break throughs in VLSI technology, many of the issues that are considered to be in the domain of distributed systems and are surfacing even within a single chip. But I digress. In this lesson we are going to learn the fundamental communication mechanisms in distributed systems and what an operating system has to do to make communication efficient. As in the previous lessons, you will see the symbiotic relationship. Between hardware in the form of networking gear and the operating system software stack, particularly the protocol stack, to make the communication efficient. We'll start this lesson module with a definition and a shared understanding of what we mean by a distributed system. But first, a quiz to get you started.</li> 
</ul>

<h2>2. What is a Distributed System</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/2.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>The third choice is that, a distributed system is one in which events that are happening on the same node, here like A and B.The time between that is called the event time. And the event that is happening across nodes, which is a communication event, Node N1 sends a message to node N2, it's a communciation event from A to C. So, the third choice is saying that communication time Tm, is much more Significantly more than the event execution time.</li> 
</ul>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/3.JPG?raw=true" alt="drawing" width="500"/>
</p>


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>
 -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>
 -->
