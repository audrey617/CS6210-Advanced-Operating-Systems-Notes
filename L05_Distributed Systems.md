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


<h2>3. Distributed Systems Definition</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/4.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So a distributed system is a collection of Nodes which are interconnected by a Local Area Network or a Wide Area Network. This Local Area Network may be implemented using a twisted pair, coaxial cable and optical fiber And if it is a Wide Area Network, it could be implemented using a satellite communication microwave links and so on. And the media access protocols that may be available for communication of these nodes on a Local Area Network or a Wide Area Network, maybe ATM or Ethernet and so on and so forth. That's sort of the picture of What a distributed system is, number one. </li> 
   <li>Number two, there's no physical memory that is shared between nodes of the distributed system. So the only way nodes can communicate with one another is by sending messages on the local area network to one another. So there is no shared memory for communication between the nodes of the distributed system. No physical memory for communication between the nodes of the distributed system. </li> 
   <li> And the third property is the fact the even computation time, that is the time it takes on a single node to do some meaningful processing, that computation time is what we are calling as the event computation time. That is Te. And a node may also communicate With other nodes in the system and that's what we're calling as communication time or the messaging time, Tm. And the third property of the distributed system is that the time for communication between nodes in the system, Tm is much more significantly larger than the event communication.</li> 
   <li>So these are the three properties which I would like to think of to make sure that we have a shared understanding of what we mean by distributed systems. That they are connected by some sort of local area network or wide area network. A collection of nodes. And their own physically shared memory, so the only communication, only way they can communicate with one another is via messages that are sent between the nodes using the local area network. And the third property, is the fact that the message communication time is significantly larger and even computation time that happens on a single node. You probably remember a good friend, Leslie Lamport. I introduced you to him when we talked about parallel systems, and I said that we will see him again. In parallel systems, he's the one who gave us the notion of sequential consistency, and the same person that we're going to be talking about in this lecture, Leslie Lamport. And in particular Lamport has a definition for a distributed system, and the definition of a distributed system verbatim goes like this, "A system is distributed if the message transmission time, Tm, is not negligible to the time between events in a single process." Cause there's a time between events in a single process, there's a message transmission time, and so the definition that Lesley Lamport gives is that "a system is distributed is a message transmission time tm, is not negligible compared to the time between events in a single process". What is the implication of this definition? Interestingly, even a cluster is a distributed system by this definition. We've been talking about clusters a lot when we discussed parallel systems and I told you that clusters are the work horses of data centers today. Even a cluster is a distributed system by this definition because processors have become blazingly fast, so the event computation has shrunk quite a bit. On the other hand the message communication time is also becoming better but not as fast as the computation time that happens on a single processor and therefore even on a cluster which is all contained in a single rack in a data center, the message transmission time is significantly more than The event time. And so even a cluster is a distributed system by this definition. The importance of this inequality is in the design of algorithms that are going to span the nodes of the network. What we want to make sure, because the message transmission time is so significantly larger than the event computation time on a single node In structuring applications that run on distributed nodes of a system like this, one has to be very careful to make sure that the computation time in the algorithms that you're designing is significantly more than the communication time. Otherwise, we are not going to reap the benefits of parallelism. If most of the time you're communicating. That's the reason why this definition of distributed system is extremely important.
</li> 
</ul>

<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="500"/>
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
