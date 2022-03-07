# Lesson outline
- [L05a: Definitions](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L05_Distributed%20Systems.md#1-definitions-introduction)
- [L05b: Lamport Clocks](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L05_Distributed%20Systems.md#l05b-lamport-clocks)
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
   <li>So these are the three properties which I would like to think of to make sure that we have a shared understanding of what we mean by distributed systems. That they are connected by some sort of local area network or wide area network. A collection of nodes. And their own physically shared memory, so the only communication, only way they can communicate with one another is via messages that are sent between the nodes using the local area network. And the third property, is the fact that the message communication time is significantly larger and even computation time that happens on a single node. You probably remember a good friend, Leslie Lamport. I introduced you to him when we talked about parallel systems, and I said that we will see him again. In parallel systems, he's the one who gave us the notion of sequential consistency, and the same person that we're going to be talking about in this lecture, Leslie Lamport. And in particular Lamport has a definition for a distributed system, and the definition of a distributed system verbatim goes like this, "A system is distributed if the message transmission time, Tm, is not negligible to the time between events in a single process." Cause there's a time between events in a single process, there's a message transmission time, and so the definition that Leslie Lamport gives is that "a system is distributed is a message transmission time tm, is not negligible compared to the time between events in a single process". What is the implication of this definition? Interestingly, even a cluster is a distributed system by this definition. We've been talking about clusters a lot when we discussed parallel systems and I told you that clusters are the work horses of data centers today. Even a cluster is a distributed system by this definition because processors have become blazingly fast, so the event computation has shrunk quite a bit. On the other hand the message communication time is also becoming better but not as fast as the computation time that happens on a single processor and therefore even on a cluster which is all contained in a single rack in a data center, the message transmission time is significantly more than The event time. And so even a cluster is a distributed system by this definition. The importance of this inequality is in the design of algorithms that are going to span the nodes of the network. What we want to make sure, because the message transmission time is so significantly larger than the event computation time on a single node In structuring applications that run on distributed nodes of a system like this, one has to be very careful to make sure that the computation time in the algorithms that you're designing is significantly more than the communication time. Otherwise, we are not going to reap the benefits of parallelism. If most of the time you're communicating. That's the reason why this definition of distributed system is extremely important.
</li> 
</ul>


<h2>4. A Fun Example</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/5.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>We're going to look at a fun example. This is me, and I'm going to India for Christmas holidays. And I'm going to make an airline reservation. I'm going to use Expedia to make the airline reservation. So what I'm doing on my computer, I'm sending a message to Expedia, saying, "hey, make a reservation for me". And Expedia chooses to make the reservation using Delta, so it sends a message. a to b is a message that I sent to Expedia saying I need a ticket to go to India, preferences, and so on. Expedia then sends a message to Delta booking the reservation that I want. Delta confirms by this message, e to f, that yes, Kishore's reservation is in. And once Expedia has received this confirmation from Delta, it sends me a message, g to h. And this message is telling me that I've go the airline reservation booked. So all of these are messages. a to b is a message, a is the sending of the message, b is the receipt of the message. And c is the sending of the message from Expedia to Delta. e is the confirmation that my reservation is in from Delta to Expedia and finally g to h is the message from Expedia to me saying that "yes, you have your reservation, you can go to India in December". That's good. And then, what I'm doing is, I'm directly contacting Delta, message from me, me to Delta, asking for my preference for food. Fortunately, it's an international trip, so I'm going to get a little bit more than peanuts on the Delta flight to India. So I sent a message asking for my meal preference and Delta confirms that "yes, you have your meal preference". That's the message k to l, is the message that confirms that I have my meal preference, I'm all set. So everything that I've described here is what you probably do on a routine basis, every time you're making any travel plans. Either contacting Expedia or some other web portal to make your airline reservation. All of this makes logical sense, right?</li> 
   <li>There are several beliefs that are ingrained in this picture here about the ordering of events in the distributed system that makes all of this work. In particular, when we look at the set of events that you're seeing here as events that I'm responsible for, we think that these events are happening in sequential order. So for instance, if you look at what Expedia is doing, it is receiving my message saying that I want an airline reservation to be made, does a bunch of bookkeeping. Then sends this message over to Delta saying that well, go ahead and make this booking for him, gets the acknowledgement back from Delta. And then it does a bunch of other bookkeeping, once it gets the acknowledgement from from Delta and then it tells me that, okay, you've got it. And after that, it does some more bookkeeping to say that, well, you know to show if booking is done and I'm going to make some internal notes on the details of this booking. And so those are all things that are happening as events within Expedia.</li> 
   <li>So the beliefs that we have is that processes are sequential, that is, the events that we see happening in a given process, these are the events that are happening in a given process, we expects these events to be totally ordered, right? So for instance, you wouldn't expect given this ordering of events, that you see in Expedia's profile that this event m happened before sending this message c. Right? So that's the mental model that you have, that events are totally ordered within a single process, and that's why we're calling process sequential. That is, the execution of a process is a textual order that you see. At least the apparent effect of the execution of the process that you, as a user, experience is sequential. So if I look at this particular process, h happens before i, f happens before g, and d happens before e, so all of these are things that are ingrained in our mental model of processes being sequential.</li>
   <li>The other belief is that you cannot have a receipt of a message before the send is complete, right? So you have to send the message before it can be received. In other words, the receipt of a message, which is b here, has to happen after the messages are being sent from here. Similarly, this message reception f must have happened after the message was sent from Delta. So those are the core beliefs that we have about what is happening with events in a distributed system. That events within the process are sequential. And across processes, when you have communication events, send happens before receive. So these are two core beliefs that we have about the working of a distributed system. And we call these beliefs, as they happened before relationship.
</li> 
</ul>

<h2></h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/6.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So let's dig a little deeper into what we mean by the Happened Before Relationship. I'm going to denote the Happened Before Relationship with this arrow. A happened before B. That's what this notation means. What this notation is implying is one of two things, either A and B are events in the same process which means given a belief that a process is sequential, A must have happened before B. If it was a textual order A is here and B is here then A must have happened before B and that's one possibility. Or if you're asserting that A happened before B and A and B are not events on the same process but A is an event in one process, B is an event in a different process. Then there must be a communication event that connects A and B. In other words, if A is a communication event of a message, and B is a receipt of that same message. Then, A happened before B, where A is the sender of the message and B is the receiver of the message. So, this is the implication of saying that an event in a distributed system A happened before B, and these events can be anywhere in the system. Anywhere in the system an event could be happening A, and another event could be happening B, and if we are asserting that A happened before B, what we are implying is one of these two possibilities. One is that A and B are events in the same process or A is the act of a sending a message and B is the act of receiving the same message on a different node of the distributed system.</li> 
   <li>The other property of the happened before relationship is that it is transitive. What I mean by that is, if we're asserting that there is an event A that happened before B. And this event B happened before C. The implication is this relationship is transitive ad therefore A happened before C. So that's the transitivity of the Happened Before Relationship. Now that I introduced to you the Happened Before Relationship it is time for another fun quiz.</li> 
</ul>


<h2>6. Relation</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/7.JPG?raw=true" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/8.JPG?raw=true" alt="drawing" width="500"/>
</p>

<h2>7. Happened Before Relation (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/9.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now that we understand the" happened before" relationship and the transitivity of" happened before" relationship. I also want to introduce this notion of concurrent events. That is, concurrent events are events in which there is no apparent relationship between the events. So, for instance, I'm showing you two nodes of the disterbent system. A is an event on one node, and b is an event on another node, and you can see that since a and b are not events on the same node we cannot apply the sequential process condition to say that there is an ordering between a and b, and by the same token, since a is an event here, b is an event here And there is no communication between these two guys that connections these events in any shape or form, either directly or transitively. There is no ordering between a and b, and therefore these two events are concurrent events, not sequential events, not connected where they happened before relationship, but they're concurrent events. So, in other words, We cannot say anything about the ordering of a and b in the distributed system. So, this is the fun thing about a distributed system is that has "Happened Before" relationship which looks at either events on the same process or events across process is connected by communication and the transitivity of "Happened Before" relationship Through the native happened-before relationships give at best a partial order for all the events that are happening in the system, so there's no way for us to derive a total order by looking at the events that are happening on the same process or just looking at the events that are happening on the different processes. In the distributed system and this is a very good example why it's impossible to get a total order for all the events that are happening in the distributed system. Because there are events they are going to be concurrent, that's the nature of the game that these procesors are executing asynchronously with respect to one another and therefore The event that is happening over here. You know, if I want to look at wall clock time in one execution of the distributed program, it's possible that a in real time, happened before b but the same program when I execute it again, the second time around, it could be that this event b happened before a. And therefore, these 2 events are called concurrent events. So the important point about these concurrent events that happened before relationship is that And structuring a distributed algorithm. It's important to recognize what events are connected by the happened before relationship, and what events are concurrent events, and once you have an understanding of these two concepts, then you can build robust distributed systems and robust applications. Because if you have any assumptions about the ordering of events that are unconnected by communication like this, that can lead to an erronious program. So one of the bane of distributed programs is synchronization and communication bugs and timing bugs. And this is a classic example. Of a timing bug that you can have if you mentally think that A happened before B and that's the way you want it to happen. It may not happen. Because these two events are concurrent events. Now that I've introduce to you all important basics of events and ordering of events in distributed system. It's time for another quiz.</li> 
</ul>


<h2>8. Identifying Events</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/10.JPG?raw=true" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/11.JPG?raw=true" alt="drawing" width="500"/>
</p>

<h2>9. Example of Event Ordering</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/12.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Returning to our original example of me ordering a ticket to go to India via Expedia and Delta. Let's now identify all the events that are connected directly by the happened-before relationship. So, if these are the events in my process, then we know that E happened before H. And, we know that H happened before I. And, we know that I happened for L. So that's the textual ordering, and we know the process is sequential. This is the ordering of events, in my process. And similarly, we can see that, if these are the events in Expedia's process. Then all of these events have to be sequentially ordered. So, B should have happened before C C should have happened before F, F should have happened before G, and G should have happened before M. So these are the orderings of the events in Expedia's process and similarly we can derive the order of events in the delta process, as sequential. And these are all the communication events that are directly relating events happening between any two processes that I'm showing you in this picture. So, for instance, E to F is a message from Delta back to Expedia confirming my reservation. So E is the act of sending the message from Delta, and F is the act of receiving the same message from Delta on Expedia. So, those are all the communication events. And now you can also look at transitive events, so for instance. What is the relationship between, let's say, event E and event A? Well, it turns out that A must have happened before event E. And the reason is, if you look at A, it happened before B, B happened before C, C happened before D. All of these are communication events, pretty straight forward. So from here to here, it's not a communication event, but since the process is sequential, B should have happened before C, and of course C happened before D. So, since it's a communication event, sequential process D should have happened before E. And that's what gives a transitive relationship between A and E, that A must have happened before E. Similarly, we can identify other events that are transitively connected to one another. Because of the happen-before relationship. So, for instance, D and M apparently don't have any direct connection. But, through the transitivity of events that are happening sequentially, and through the communication, and sequentiality of a process, we know that D must have happened before M. So those are transitive events. And finally, let's look at concurrent events. If you think about this event M that's happening in Expedia. Basically Expedia has confirmed to me that I have the booking that I want. Then it is doing some internal bookkeeping, to record some information about me, maybe my preferences in terms of airlines and so on and so forth. And from that point of view, it is making some internal bookkeeping and that's is event M. And now if you look at this event M, it has no relationship to any of the events that are happening here. I'm showing G, this event H must have happened after G, but what about H and M? There is no relationship between these two guys. This could've happened much later than, than this in wall clock time. Or it could've happened much sooner than event M. So you can see that, H is concurrent with M, and in fact, all the events that you see here, are going to be concurrent with M. And similarly, all the events that you are seeing over here, they're concurrent with So in fact, they're concurrent with M. After this, if you look at the Delta process, after Delta has sent this message to Expedia confirming my booking, it may have done a whole bunch of events, over here. All of those events are concurrent with M, because there is no ordering between these events, and the events over here. But there is an ordering between the event, G and the event J here. Because G happened before H, H happened before I, I happened before J. So, you can see that transitivity connects events across machines. But they could be events that are happening in the distributed system, that are unconnected to other events. And those are concurrent events. That completes discussion of the basics of distributed system. Next we're going to start talking about Lamport's clock.
</li> 
</ul>


# L05b: Lamport Clocks
<h2>1. Lamport Clocks Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/13.JPG?raw=true" alt="drawing" width="500"/>
</p>

<h2>2. Lamport's Logical Clock</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l5/14.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>What does each node in the distributed system know? Every node knows its own events, meaning, what are the computational events that are happening in its own execution. It also knows about its communication events with the rest of the nodes in the distributed system. So, for instance, if this process Pi sends a message to process Pj, that's a send event. Pi knows about that. And similarly Pj when it gets this event, it's a receive event. It knows about that. So, these are the only two kinds of events that every node knows about, its own computational events, and its communication events with its peers. For instance, process Pj has no idea about the local computational events of Pi. Lamport's logical clock builds on this very simple idea, and the idea is that we want to associate a timestamp with every one of the events that are happening in every process in the entire distributed system. How are we going to do that? Well, we're going to have a local clock, Ci over here, And Cj over here. And this local clock can be anything, can be counter, it can be a simple counter and what we are doing is when I want to associate a timestamp with a particular event that's happening in my process what I'm going to do is I'm going to look at this counter and see what the value of the counter is, associate that counter value as a timestamp for this event A. So for this instance here I've said that timestamp for a is two. And this counter monotonically increases as we pile up events in our system. So once I've associated this timestamp two with this event a, I cannot associate the same timestamp with other meaningful events that are happening in my process. And therefore, I'll increment the timestamp. It's completely up to your implementation as to whether you want to increase this counter value by one, or two, by a thousand, it doesn't really matter. So, in this case for instance I've incremented the timestamp counter by two, and so the next even that I'm going to have in my process B is going to have a timestamp of 4. So that's the idea behind having a monotonically increasing counter to associate logical timestamps with the events that are happening in my process. Now, what about these communication events? Well, in particular, let's look at this case here. A is a communication event on process Pi. And this communication event is going to have a timestamp of two associated with it, because that's the time that I generated this communication event, sent this message over. Comes over to process Pj. When process Pj receives this, that's an event. And so let's say we call it event number d. And we have to associate now a timestamp with, with event d. How do I go about assigning a timestamp with this? Well, we know that this timestamp that I'm going to associate with this event d, has to be greater than the timestamp associated with the send of that message, right? Obviously you cannot receive a message that has not been sent yet. And therefore, we're going to say that d should have a timestamp which is at least greater than a for sure. Now, what else does d depend on? Well, it depends on other things that are happening in my own process. For that, I need to know what the current state of my local counter is. So for instance, in my execution as I'm showing you here I haven't done anything meaningful yet and therefore my local counter is still pointing to zero indicating that there have been no meaningful events here. So when this message comes, that's the first time I'm going to do something meaningful in this process. And I have to associate a timestamp with d but I cannot associate the timestamp of zero with it because the timestamp that I associate with d has got to be greater than the timestamp that is associated with the send event on process Pi. And since the send event has this timestamp two, I have to associate something higher than that. And so I associate a timestamp three, with the receipt of this message. That is, this particular event is going to have a timestamp of, of three. So, the two conditions that we have talked about. One is that events that are happening in the same process I'm going to have a monotonically increasing counter and I'm going to use that to associate timestamps with the events that are happening in the same process. That's the first thing. That is if I have two events a and b in the same process and I know that a happened before b, because, textually, in the process because the process is sequential, a happened before b and therefore the condition is that the timestamp associated with a has got to be less than the timestamp associated with b. Pretty straightforward. And similarly the second condition is that if I have two events, a and b, and a happens to be a send event on some process and d happens to be the receive event on another process then we know that the receive event has to be after the send event. And therefore the second condition is that Ci(a), that is a timestamp associated with a send event, has got to be less than the timestamp associated with the receipt of the same message, Cj(d). So in order to make this condition, the second condition, valid, we're going to choose the timestamp to associate with the receipt of a message d as the max of the timestamp that is associated with the send event incremented by some quantity. Ci(a) plus plus. So in this case, it happens to be two. So increment it and then say three and then you want to know what the local counter is saying. So, Cj, so what we pick as a timestamp to associate with d is the max of the incoming timestamp with the message and the local counter, whatever it is pointing to. So that's how I'll pick the timestamp to associate with a receive event. This brings up a very interesting question and that is, what about timestamps of events are happening concurrently in a distributed system, and before I talk about that, I want to give you a little quiz.
</li> 
  <li></li> 
  <li></li> 

</ul>




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
