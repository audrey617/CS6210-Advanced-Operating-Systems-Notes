# Lesson outline
- [L07a: Global Memory Systems](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L07_Distributed%20Subsystems.md#l07a-global-memory-systems)
- [L07b: Distributed Shared Memory](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L07_Distributed%20Subsystems.md#l07b-distributed-shared-memory)
- [L07c: Distributed File Systems](-)

# L07a: Global Memory Systems 
<h2>1. Global Memory Systems Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/1.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>We started the discussion of the distributive systems with definitions and principles. We then saw how object technology, with its innate concepts of inheritance and reuse, helps in structuring distributive services at different levels. In this next lesson module, we will discuss the design and implementation of some select distributed system services. Technological innovation come when one looks beyond the obvious and immediate horizon. Often this happens in academia because academicians are not bound by market pressures or compliance to existing product lines. You'll see this out of the box thinking in the three subsystems that we're going to discuss as part of this lesson module. Another important point, often the byproducts of a thought experiment may have more lasting impact than the original vision behind the thought experiment. History is ripe with many examples. We will not have the ubiquitous yellow sticky note but for space exploration. Now close to home, for this course, Java would not have happened but for the failed video-on-demand trials of the 90s. In this sense the services we are going to discuss as part of this lesson module, while interesting in their own right made themselves be not as important as the technololgical insights they offer on how to construct complex distributed services. Such technological insights are the reusable products of most thought experiments. 
</li> 
  <li>There's a common theme in the three distributed subsystems we're going to discuss. Memory is a critical, precious resource. With advances in network technology leveraging the idle memory of a peer in a Local Area Network may help overcome shortage of memory, locally available in a particular note. The question is how best to use the cluster memory, that is the physical memory of peers, tend to be idle at any particular point of time in a local area network. </li> 
  <li> Global memory system, GMS, asks the question, how can we use the peer memory for paging across the Local Area Network. And later on we will see DSM, which stands for distributed shared memory, asks a question, if shared memory makes the life of application programmers simple in a multiprocessor, can we try to provide the same abstraction in a cluster and make that cluster appear like a shared memory machine. And a third work that we will see is this distributed file system, in which we ask the question, can we use the cluster memory for cooperative caching of files. Anyhow, back to our first part of this three-part lesson module, namely the Global Memory System.
</li> 

</ul>

<h2>2. Context for Global Memory System</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/2.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Typically, the virtual address space of a process running on a processor, let's say your desktop, your PC, your laptop, and so on, is much larger than the physical memory that is allocated for a particular process. So the role of the virtual memory manager in the operating system is to give the illusion to the process that all of its virtual address space is contained in physical memory. But, in fact, only a portion of the virtual address space is really in the physical memory. And that's called the working set of the process. The virtual memory manager supports this illusion for the process by paging in and out from the disk the pages that are being accessed by a process at any particular point of time so that the process is happy in terms of having its working set contained in the physical memory. </li> 
  <li>When a node or a desktop is connected on a local area network to other peer nodes that are also on the same local area network it is conceivable that at any point of time, the memory pressure, meaning the amount of memory that is required to keep all the processes running on this node happy, may be different from the memory pressure on another nodes. In other words, this particular node may have a much higher memory pressure because the workload on this load consumes a lot of the physical memory whereas this work station may be idle and therefore all of this memory is not being utilized at this point for running anything useful because no applications are running on these nodes. This opens up a new line of thought.</li> 
  <li>Given that all these nodes are connected on a local area network and some nodes may be busy while other nodes may be idle. Is it possible if a particular node experiences memory pressure, can we use the idle cluster memory, and in particular, can we use the cluster memory available for paging in and out the working set of the processes on this node? Rather than going to the disk, can we page in and out to the cluster memories that are idle at this point of time? It turns out that with advances in local area networking gear, it's already made possible for gigabit ethernet connectivity to be available for desktops. And pretty soon, 10 gigabit links are going to be common in connecting desktops to the local area network. This makes sending a page to a remote memory or fetching a page from a remote memory faster than sending it to a local disk. Typically, the local disk access speeds are in the order of 200 megabytes per second in terms of transfer rate, but on top of that you have to add things like seek latency and rotation latency for accessing the data that you want from the disk. So all of this augers well for saying that perhaps paging in and out through the local area network to peer memories that are idle may be a good solution when we have memory pressure being experienced at any particular node in the cluster.</li> 
  <li>The global memory system or GMS for short uses cluster memory for paging across the network. In normal memory management, if virtual address to physical address translation fails then the memory manager knows that it can find this virtual address on the disk, meaning the page that contains this virtual address on the disk, it pages in that particular page from the disk. Now in GMS, what we're going to do is, if there is a page fault that is this virtual address to physical address translation, fails then that means that the page is not in the physical memory of this node. In that case, GMS is going to say, "well it might be there in the cluster memory in one of the nodes of my peers, or it could be on the disk if it is not in the cluster memory of my peers." So that's the idea that GMS is sort of integrating the cluster memory into this memory hierarchy.</li> 
  <li>Normally, when we think about memory hierarchy in a computer system, we say there's a processor, there's the caches, and there's the main memory, and then there is the virtual memory sitting on the disk. But now in GMS, we're sort of extending that by saying in addition to the normal memory hierarchy that exists in any computer system, that is processor, caches and memory, there is also the cluster memory. And only if it is not in the cluster memory, we have to think of going to the disk in order to get the page that we're looking for. That's sort of the idea.</li> 
  <li>So GMS trades network communication for disk I/O. And we are doing this only for reads or for reading across a network. GMS does not get in the way of writing to the disk, that is the disk always has copy of all the pages. The only pages that can be in the cluster memories are pages that have been paged out that are not dirty. And if there are dirty copies of pages, they are to be written onto the disk just like it will happen in a regular computer system. In other words, GMS does not add any new causes for worrying about failures because even if a node crashes all that you lose is clean copies of pages belonging to a particular process that may have been in the memory of this node. But those pages are on the disk as well. So the disk always has all the copies of the pages. It is just that the remote memories of the cluster is serving as yet another level in the memory hierarchy.</li> 
  <li>So just to recap the top level idea of GMS. Normally, in a computer system if a process page faults that means that the page it is looking for is not in physical memory, it'll go to the disk to get it. But in GMS, what we're going to do is if a process page faults, we know that it is not in physical memory but it could be in one of the peer memories as well. So GMS is a way of locating the faulting page from one of the peer memories, if it is in fact contained in one of the peer memories. If not, it's going to be found on the disk. So that's the idea behind that. So when the virtual memory manager that is part of GMS decides to evict a page from physical memory to make room for the current working set of processes running on this node, then what the virtual memory manager is going to do is, instead of swapping it out to the disk, it is going to go out on the network and find a peer memory that's idle, and put that page in the peer memory so that later on if that page is needed, it can fetch it back from the peer and memory. That's sort of the big picture of how the global memory system works.
</li> 
</ul>




<h2>3. GSM Basics</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/3.JPG?raw=true" alt="drawing" width="500"/>
</p>
<ul>
  <li>Let's introduce some basic terminologies. In GSM, when we talk about cache, what we mean is physical memory. We're not talking about the processor caches. We're talking about physical memory that is the dynamic random access memory, or DRAM for short, which is the physical memory. That's what we mean when we use the term cache.</li> 
  <li>There is a sense of community to handle page faults of a particular node. We'll get to that in a minute. </li> 
  <li>I mentioned that we are going to use peer memories as a supplement for the disk. In other words, we can imagine that the physical memory at every node to be split into two parts. One part is what we'll call "local", and local contains the working set of the currently executing processes at this node. So this is the stuff that this node needs in order to keep all the processes running on this node happy. Now, the "global" is similar to a disk. This global part is where community service comes in, that is, I'm saying that out of my total physical memory, this(Local) is the part that I need to keep all the processes happy in my node, and this is the part(global) that I'm willing to use a space for holding pages that are swapped out from my fellow citizens on the local data network. And this split of local and global is dynamic in response to memory pressure.</li>
   <li>As I mentioned earlier, the memory pressure is not something that stays constant, right? So over time, depending on what's going on in a particular node, you may have more need for holding the working set of all your processes, in which case the local part may keep increasing. On the other hand, if I go off for lunch, my workstation is not in use. And in that case, my local part is going to shrink, and I can house more of my peers swapped out pages in my global part of the physical memory.</li> 
  <li>The global part is a spare memory that I'm making available for my peers and local part is the part that I need for holding the working set of the currently active process at my node, and this boundary keeps shifting depending on what's going on at my node. Pretty simple.</li>
   <li>Normally if all the processes executing in the entire local area network are independent of one another, all the pages are private. You know, I'm running a process, my process, my pages, and the contents of that pages are private to my process. On the other hand, you could also be using the cluster for running an application that spans multiple nodes of the cluster, in which case it is possible that a particular page is shared, and in that case, that page will be in the local part of multiple peers, because multiple peers are actively using a page.</li> 
  <li>So we have two states for a particular page. It could be private, or it could be shared. If a page is in the global part of my physical memory, then it is guaranteed to be private. Because the global part is nothing different from a disk. So when I swap out something, I throw it onto the disk. Similarly, when I swap out something in GMS, I throw it into my peer memory as the global cache, and therefore what is in the global cache is always going to be private copies of pages, whereas what is in the local part can be private or can be shared, depending on whether that particular page is being actively shared by more than one node at a time.</li> 
  <li>Now one important point, the whole idea of GSM is to serve as a paging facility. In other words, if you think about even a uni processor, if it had multiple processes sharing a page, the virtual memory manager has no concern about the coherence about the pages that are being shared by multiple processes, that's the same semantic that is used in GSM, and that is coherence. For shared pages, it's outside GSM, it's an application problem. If there are multiple copies of the pages residing in the local parts of multiple peers, maintaining the coherence is the concern of the application. That is not GSM's problem. Only thing that the GSM is providing is a service for remote paging. That's important distinction that one has to keep in mind. In any workshop memory management system, what you do is when you have to throw out a physical page, you use a page replacement algorithm and the page replacement algorithm that is typically employed in computer systems is some variant of an LRU or a least recently used algorithm. GSM also does exactly the same thing, except it integrates cluster memory management of the lowest level across the entire cluster. So the goal in picking a replacement candidate in GSM is to pick the globally oldest place for replacement. If let's say that the memory pressure in the system is such that I have to throw out some page from the cluster memory onto the disk. The candidate page that I'll choose is the one that is oldest in the entire cluster.</li> 
  <li>Managing age information is one of the key technical contributions of GSM - how to manage the age information so that we pick a globally oldest page for replacement in the community service for handling page faults.
</li> 
</ul>


<h2>4. Handling Page Faults Case 1</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/4.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Before we dive into the nuts and bolts of how GMS is implemented and architected, let's first understand at the high level what's going on in terms of page fault handling. Now that we have this community service underlying GMS.</li> 
  <li>In this picture, I am showing you two hosts, host P and host Q. You can see that the physical memory on this host is divided into the local and the global part. Similarly, the physical memory on host Q is divided into the local part and the global part. And we already mentioned that these are not fixed in size but the size actually fluctuates depending on the activity and that's what we are going to illustrate through a example situations.</li> 
  <li>So the most common case is that I am running a process on P and that page faults on some page X. When that happens, you have to find out if this page X is in the global cache of some node in the cluster. Lets say that this page happens to be in the global cache of node Q.</li> 
  <li>So what will happen in order to handle this page fault is the GMS system will locate, oh this particular page it's on host Q. So it'll go to host Q. And the host Q will then send the page X over to node P and clearly, if there there was a page fault that means that the memory pressure on host P is increasing and therefore, it is going to add X to it's current working set. That is it's local allocation of the physical memory is going to go up by one but it cannot go up by one without getting rid of something here. Because the sum of the local and global is the total amount of physical memory available in this node and therefore, what P is going to do is, pick the oldest page that's available in the global part and send it over to node Q. So, in other words, what we are doing so far, as host Q is concerned is saying, well X happens to be currently in the working set, then resend it to host P. And host P says, well, my working set is increasing. Therefore, I have to shrink my community service and we going to reduce the global part by one. Pick the oldest page. Lets say it's Y. Send it over to host Q. So that the host Q can host this new page Y in the global cache on this node. </li> 
  <li>The key take away for you is that, for this particular common case, the memory pressure pressure on P is increasing, so the local allocation of the physical memory goes up by one, and the global allocation (the community service part) goes down by one on host P. Where as on host Q it remains unchanged because all that we have done is we have traded Y for X.
</li> 
</ul>

<h2>5. Handling Page Faults Case 2</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/5.JPG?raw=true" alt="drawing" width="500"/>
</p>
<ul>
  <li>The second case is simliar to the common case, but the memory pressure on P is exacerbated in this case. There's so much memory pressure that all of the physical memory that's available at P is being consumed by the working set, while hosting all the applications running on host P. There's no community service happening on host P. And now, if there is another page fault, for some new page on P, then there is no other option on host P except to throw out some page from its current working set in order to make room for this missing page.</li> 
  <li>So, we're going to do exactly similar to what we did in the previous case, with the only difference that the candidate that is being chosen as a victim or the replacement candidate (called a victim) in the management of virtual memory system, is coming from the local part of host P itself.</li> 
  <li>So we get the missing page and we send out the oldest page from local part of host P, recognizing that the local part is zero right now. So in this case you can see that there is no change in the distribution of local and global on P, because global is already zero, it's not going to be anything less than that. So the distribution remains unchanged. And as in the previous case, there's no change on host Q as well in terms of the split between local and global.
</li> 
</ul> 

<h2>6. Handling Page Faults Case 3</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/6.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>The third case is where the faulting page is not available in the cluster memories at all. In other words, the only copy exists on the disk.In this case, what has to happen is, when we have the page fault on node P for this x, we have to go to the disk to fetch it. And we're going to fetch it, which means that the working set on node P is growing, similar to the first case. And so the local part is going to go up by one. In order to make room for that, I have to necessarily shrink the global part as in the first case. So, I am going to shrink that global part by 1. By the way, I can pick any page from the global part, and both in the first case, as well as in this case, we can pick any page from the global part and send it out to a peer memory. And that's what we're doing here, so we are saying, "Here is a page that I have to get rid of. Who do I send it to? Well, I am going to send it to the guy that happens to have the globally oldest page in the entire cluster."</li> 
  <li>Let's say there is a host R that has the globally oldest page in the entire cluster, and that globally oldest page in the host R could be on the local part or the global part of this host. So what we're going to do is to tell that guy, "hey, I'm going to give you a page to hold, because this used to be in my global part, I don't have room anymore, because my local is increasing by one because of this page fault and adding x to my working set now. So please hold on to this page that I'm going to give you in your global cache." Now this guy has a split like this. So if it has to make room for this new page that is coming in from its peer, clearly it has to get rid of something. Where it will get rid of? Well, it has to throw it on the disk.</li> 
  <li>The interesting part is, if the oldest page on host R happens to be in the global cache of R, what can we say about that page z? Well, it has to be cleaned, because global part is nothing but a paging device. And therefore if it is here, it must be cleaned, and therefore, I can discard it without worrying about it. Just simply dump it. Drop it on the floor. That's what I'm going to do. On the other hand, if it is on the local part, it could be dirty. That is, if the oldest page happens to be on host R, and it also happens to be in the local part of host R, it is conceivable that this page has been modified, in which case, that modified copy has to be written out to the disk. So in other words, when we pick host R to send this replacement page from my host(Host P), this guy(Host R), what he's going to do is, he's going to say, "I have to get rid of a page in order to make room for this incoming global page, because I have the globally oldest page. And if I have the globally oldest page, then let me get rid of it by throwing it out onto the disk if it happens to be dirty. If it happens to be clean, simply drop it on the floor, because I know that all the pages are contained on the disk." </li> 
  <li>That's the fundamental assumption we started with, that all the pages of the currently active processes are on the disk. It is just that the global cache of every node is acting as a surrogate for the disk, because it can be faster to access from the peer memory than from the disk. So similar to the first case, in this case also, the local portion of the physical memory allocation on host P is going to go up by one, and the global portion is going to go down by one. What about host R, well it really depends. If the oldest page on host R happens to be in the global cache, then there is no change, because I am reading the old page z for another page that is coming in from host P, that is y that is coming in. In that case there is no change in the allocation between local and global on host R. But on the other hand, if the globally oldest page happens to be from the local part of host R, what that means is that even though originally we thought z to be part of the working set of host R, clearly, the processes that were using it are either no longer active or they're complete or whatever, and therefore, we're throwing out this page. The local part is shrinking. If the local part on host R is shrinking, what that means is, I can use more of the memory that's available in host R for community service. That's the important message I should get out. That's the important message I want you to get out of looking at this particular page fault scenario.
</li> 
</ul>


<h2>7. Handling Page Faults Case 4</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/7.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So far the cases that we considered are cases in which a page that we are faulting on is private to a process. Let's consider the case where a page is actively shared, that means that both of host P and host Q are actively sharing a page X, and currently some processes on host Q is using that page X, so it is in the local part in the working set portion of host Q. And for host P, the process is page faulting on the same page X. So when a page faults happens, GMS says, "oh, let me find out what it is." Well, it finds that x is in the local part of host Q and because it is in the current working set of host Q. We don't want to yank it away from there. We want to leave it there. We want to make a copy of this page into the local part of host P so that the faulting process on host P will have access to the page x.</li> 
  <li>Again, we have the same situation that the working set on host P is increasing. The local part has to allocate one more page in physical memory for the active working set for the processes on P. So the global part has to shrink by one. So, this local goes up by one, this global goes down by one. Pick again some arbitrary page from the global part. It doesn't have to be the LRU page, because all that your going to do is you're going to put this into the peer memory somewhere. In that sense, what we are doing is we're going to say "well, take this y and host in some of the peer cache". So who are we going to send it to? Well, we're going to send it to the host R that happens to have the globally oldest page.</li> 
  <li>Remember that in this situation, the total memory pressure of the entire cluster is going up by one, not just this host P, because x is going to be still present in the working set of host Q, and x is also going to be present in the working set of host P, which means the total memory impression in the entire cluster is going up by 1. And that is the case, then I have to necessarily pick a page from the entire cluster and throw it all under the disk, and that's what we're doing here. What we are saying is that host R happens to have the globally oldest page, and this page that I am replacing from host P in order to accommodate the missing page and bring it into my local cache on host P is to throw out why over to host R and host R in order to make room for this guy in it's global cache has to necesarily pick a victim from its physical memory and the victim it picks is going to be The LRU candidate, and that could come from the Local part or from the Global part. Again, if it comes from the Global part, we know it's private and we throw it away. if it comes from the Local part, it could be that this is a dirty copy, in which case we have to write all to the disk, or drop it on the floor if it is a clean copy because the disk always has all the copies of the pages. In either case the important message is that if the LRU candidate that we're going to throw out onto the disk or drop on the floor comes from the local part of R. That means similar to the previous case, the working set on local R is shrinking. It is coming down by one, which means host R can do more community service in the future. </li> 
  <li>Now the x is present in the working set of both host Q and host P which means actively some processor on host P, some process on host Q are accessing this page X. I mentioned earlier that this is not something that GSM worries about. If coherence is to be maintained for the copies of the same page existing on multiple nodes, that's not in the domain of GSM because GSM is simply a paging device. So it doesn't worry about coherence maintenance. There has to be something that the application or some higher level of the software has to worry about. So we can talk about the relative split between local and global. In this particular case, well, in this case host P local goes up by one, global goes down by one. In host Q nothing changes because there is an actively shared page which may relieve the copy here, just give a copy over to P, so the balance between local and global doesn't change. On the other hand in host R if the replacement candidate came out of the global part, then then there is no change in the split between local and global and host R either. On the other hand, if the replacement candidate came from the local cache of host R, what that means is that the working set, memory pressure on host R is decreasing. And therefore L will go down by one and G will go up by one.</li> 
  <li>I want you to think through all these four cases very carefully. The first three cases that we looked at were cases in which the pages were all private. And this is the only case where the page is potentially shared because it is in the local cache of Host Q and in the local cache of Host P as well, and now it is time for a quiz.
</li> 
</ul>


<h2>8. Local and Global Boundary</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/8.JPG?raw=true" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/9.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>ANS: I'm sure your memory recall capability is very good and you would have come up with the right answers for all of these entries. Let me just quickly summarize what I'm sure that most of you have gotten already.</li> 
  <li>In all cases except where the global part of the faulting node Ps cache is empty, the local part of course is going to go up by one, the global part is going to come down by one. Only when the global part is empty already, there's no change.</li> 
  <li>And in both cases (in Q's global), there's no change to the balance between L and G on both the node Q, which is supplying the page X. And the node R that happens to have the LRU page, because it is not even part of the action, in terms of the page fault handling, for these two cases.</li> 
  <li>Now the third case, is where it is on the disk and therefore Node Q is immaterial. Right? Because it's not in any cluster memory right now. It is on the disk, so it's not applicable. Well, the question is what happens to the Node R that has the globally oldest page. If it is on the disk and we have to bring it into the faulting node, then necessarily we have to make space in the cluster memory as a whole for this extra page. Because this guy's balance is going to shift, and I have to throw this global page somewhere. I am picking node R as the replacement candidates, because he has the globally oldest page.</li> 
  <li>But what is going to happen on Node R? Well, it really depends on whether the LRU page comes from the global part or the local part. If it comes from the global part, then there is no change on R, because you're basically throwing this into the global part, and he's throwing one of those global pages out onto the disk, right? If on the other hand, the global page that I sent over here results in replacing a local page on node R, because the local page happens to be the LRU candidate then L goes down by one and G goes up by one. So we had able to do more community service in this case</li> 
  <li>In the last case when it is actively shared, again, even though we find the missing page in note Q, there's no change in the balance of L and G because it is coming from the active part of L, because it is shared. It indicates if it is shared it has to be, from the L part of Q. Since it is actively shared, there's no change in the split between L and G and of course, this guy has to send one of its global pages to some other node because the memory pressure as a whole is increasing on node P. This guy is going to send it to node R that has the LRU page, and just like in this case, where I have to make room for the cumulative memory pressure on the cluster by throwing out a page onto the disk. And similar to this case, if the candidate replacement page comes from the local part of Node R, then it is going to result in the local part shrinking by one and the global part increasing by one to accommodate the page that came from here.
</li> 
</ul>


<h2>9. Behavior of Algorithm</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/10.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>From the high level description of what happens on page faults and how it is handled, you can see that the behavior of this GMS global memory management is as follows:</li> 
  <li>So overtime, you can see that if there is an idle node in the LAN, then that idle node's working set is continuously going to decrease as it accommodates more and more of its peers pages that are swapped out to fit in its global cache. And eventually a completely idle node, becomes a memory server for peers on the network. So that's the expected behavior of this logarithm.</li> 
  <li>The key attribute of the algorithm is the fact that the split between Local and Global is not static but is dynamic, depending on what is happening at a particular node. For instance, even the same node, if it was serving as a memory server for my peers, because I had gone out to lunch, I come back, and start actively using my workstation. In that case, I'm going to go back, and the community service part of my machine is going to start decreasing. So the Local Global split is not static, but it shifts depending on what local memory pressure exist at that node.
</li> 
</ul>


<h2>10. Geriatrics!</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/11.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now that we understand the high level goal of GMS, the most important parameter in the working of GMS system is age managemet because we need to identify what is the globally oldest page whenever we want to do replacements. Let's see how that algorithm works and how it is managed.</li> 
  <li>In any distributed system, one of the goals is to make sure that any management work is not bringing down the productivity of that node. In other words, you want to make sure that management work is distributed and not concentrated on any one node and you know in the GMS system, since we are working with an active set of nodes. Over time the node that is active may become inactive and a node that was inactive may become and so on. So you don't want the management of age information to be assigned to any given node. But it is something that has to shift over time. And that's sort of the fundamental tenant of building any distributed system is to make sure that we distribute the management work so that no single node in the distributed system is overburdened. That's a general principle, and we'll see how that principle is followed in this particular system.</li> 
  <li>So we break the management both along the space axis and time axis into what is called epoch. There are two parameters that govern an epoch. One is T which is the maximum duration of an epoch. That is an epoch is in some sense a granularity of management work done by a node. And that management work done by a node is either time bound, maximum T duration, or space bound, maximum M replacements. So if in the working of the system, if M space replacements have happened, then that epoch is complete, so we go to a new epoch. There'll be a new manager for age management. Or if the duration T is complete, then again, the epoch is complete, and so we pick a new manager to manage the next epoch. We'll see in a minute how we pick the new manager. And T maybe on the order of a few seconds. And M, which is the number of replacements, maybe on the order of thousands of replacements.</li> 
  <li>So at the start of each epoch what happens is every node is going to send the age information to the initiator. That is every node is going to say what is the age of the pages that is resident at this node - all the local pages and all the global pages /what is the age information associated with the universe of all the pages that exist at this node. Remember that the smaller the age, the more relevant the page is. So the higher the age, the older the page. So, in picking a candidate, we're always going to pick an old page to replace. So, that's the age information that each of these node is sending to the initiator. So, N1 sends its set of pages, N2 sends its set of pages and so on. Everybody is sending to the manager node that I'm calling the initiator, the age information.</li> 
  <li>What the initiator node is going to do is two things.
     <ul>
      <li>It's going to find out what is the minimum age of all the M pages that are going to be replaced. Remember that smaller the age, the better. So what it is going to say is out of all the pages that exist in the entire cluster, what are the oldest M pages that are going to be replaced in the upcoming epoch, and for those M old pages, out of those M old pages, what is the minimum age? So any page with less than the minimum age are pages that are active and that are going to survive this upcoming epoch. Whereas, any page whose age is older than that minimum age is part of this set of M pages that are going to be replaced in the upcoming epoch, and those are the replacement candidates. That's minimum age. So it computes the minimum age.</li>
      <li>It also computes given the minimum age and given the distribution of the age demographics coming from N1, I know out of these pages coming from N1 what fraction of the pages that belong to N1 are going to be replaced in the upcoming epoch. And that is the weight parameter for this particular node. For instance, if it turns out that N1 has 10% of the candidate pages that are going to be replaced in the next epoch, then its weight is 0.1. If N2 is going to account for 30% of the replacements, 30% of the M replacements in the upcoming epoch. Then N2's weight is going to be W2 and so on.</li>
    </ul>
     So what this initiative does is it computes this min age and it also computes the weight for each one of the nodes, and that's what is sent back to each node. So each node is going to see the min age and also the weight. Each load is not only receiving its own weight, that is its own fraction of the M pages that are going to be replaced, but it is also getting the fraction of the pages that are going to be replaced from each of its peer nodes in the entire cluster. We'll see how this information is going to be used by each one of these nodes. And of course, we don't know the future, all that the initiator is doing. That is saying that it is expected replacement. W1 is expected share of replacement that's going to happen in N1. W2 is expected share of replacement that's going to happen in N2 and so on. And when the next epoch starts actually, that can be different, depending on what happens in these nodes. But, that's the best that we can do is use the past to predict the future. That's what the initiates doing. It is using the past, the age information that it got from all the notes, in order to predict the future in terms of where these N replacements are going to happen. What is the minimum age of the pages that are going to be replaced in the next epoch. So that's what is being done by the initiator.
</li> 
</ul>

<h2>11. Geriatrics! (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/12.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>I mentioned that this management function that this initiator is doing, you don't want it to be statically assigned to one node, because that would be a huge burden, and besides this node will suddenly become very active, and if that happens, then you must time in the computations that are going to happen here to do this management function.</li>
   <li>Now intuitively if you think about it, if a particular node, lets say node N2 has a weight of point eight, what does that mean? What that means is that in the upcoming Epoch, 80% of the M pages are going to be replaced from the node N2, in other words, N2 is hosting a whole bunch of senior citizens, so it's a node that's not very active. Right? If an inactive node, then the replacement's candidates are probably not going to come from that guy and in predicting the future the initiator is saying that well, most of the replacements are going to come from here, so it's weighted very high. So this weight information for all the nodes is coming to every one of these guys. So intuitively if we think about it, the guy that has the highest weight is probably the least active. Why not make him the manager? So the initiator for the next Epoch is going to be the node with the maximum weight. Now how do you determine that? Well, every one of these nodes is receiving not only the min age of the candidate pages that are going to be replaced in the next epoch but also the weight distribution that says what is the percentage of these replacements that are going to happen in each one of these nodes. So locally each one of these guys can look at the Wi vector that it got back from the initiator and say, "I know, given that node N3 has the maximum rate, that's going to be the initiative of the next epoch." So locally you can make the determination so that for the next epoch, in order to send the age information, you know locally who to send it to.</li> 
  <li>Let's look at how this minimum age information is going to be used by each node. Remember that min age represents the minimum age of the M oldest pages. The smaller the age, the more recent that page is. And so those are the active pages. So if you look at this as the age distribution of all the pages, then if you draw a line here, then the pages to the left of the minimum age line are the active pages, and the pages to the right of this is the M oldest pages that are the candidate pages going to be replaced in the next epoch. This is the information that we're going to use in the way we manage page replacements at every node.</li> 
  <li>Let's see what the action is at a node when there is a page fault. If upon a page fault, I have to evict a page y, then what I'm going to do is to look at the age of this page y. If the age of this page y is more than the minimum wage, I know it's going to be replaced. Even if I sent it to a peer node, it's going to be replaced because that is part of the candidate M pages that are going to be replaced in the upcoming epoch. And therefore locally I'll make a decision that page y has an age older than minimum age, and therefore, I'm going to simply discard it. Don't worry about sending it to a peer. Remember that, in the description of how a page fault is handled, I said that, when you have a page fault in the node, I pick one of the replacement candidates, and I send it to a peer node to store it in the global cache of the peer node. Well, we don't want to do that. If that page is going to be eventually replaced during this upcoming epoch, meaning it has to be thrown out onto the disk. In that case, you might as well discard it right now. So if the age of the page that you're evicting from your node happens to be greater than MinAge, simply discard it.</li> 
  <li>On the other hand, if the page happens to be less than the MinAge, then you know that it is part of the active set of pages for the entire cluster, you cannot throw it away. You send it to Peer Node Ni. How do you pick Ni? This is where the weight distribution comes in. I know the weight of the nodes. At the end of this computation, the manager sent me the weight distribution for the upcoming epoch of all the nodes. So I can pick a node to send this page to based on the weight distribution. Of course I could say, well, send it to the guy that has the highest weight because that's the guy that is likely to replace, but remember that there's only an estimate of what is going to happen in the future. So, you don't want to hard code that. Instead, we're going to use some information drawn from these weights. We're going to factor that weight information into making a choice as to which peer we want to send this eviction page to. Chances are that the node has a higher weight is a likely candidate that'll pick. But it will not always be the node with the highest weight. Because if that decision is made by everybody, then we are going to have a situation where if the prediction was not exactly accurate, we would be making wrong choices.</li> 
  <li>Basically you can see that this age management, geriatric management, is approximating a global LRU. Not exactly a global LRU, because global LRU computing that on every page fault is too expensive, so we don't want to do that. Instead, we are doing an approximation to global LRU by computing this information at the beginning of an epoch, and using that information locally in order to make decisions. So we think globally in order to get all the age information, and compute the minimum age and compute the weight for all the nodes as to how much of the fraction of the replacements are going to come from each one of these nodes. But once that computation has been done, for the duration of the epoch, all the decisions that are taken at a particular node is local decisions in terms of what we want to do, with respect to a page that we are choosing as an eviction candidate. Do we discard it, or do we store it in a peer, global cache? This might sound like a political slogan but the important thing in any distributed system is as much as possible to use local information in decision making. So think globally but act locally. So that's the key and that's what is ingrained in this description of the algorithm.
</li> 
</ul>


<h2>12. Implementation in Unix</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/13.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>From algorithm description, we go to implementation. Now this is where rubber meets the road in systems research. In any good systems research, the prescription is as follows: You identify a pain point. Once you identify the pain point. You think of what may be a clever solution to that. Then a lot of heavy lifting actually happens in taking that solution, which may be a very simple solution, but implementing that is the hard part. if you take this particular lesson that we're looking at. The solution idea is fairly simple. The idea is that instead of using the disk as a paging device, use the cluster memory. But implementing that idea requires quite a bit of heavy lifting, and one might say that in systems research these technical details of taking an idea and working out the technical details of implementing that idea is probably the most important nugget. Even if the idea itself is not enduring, the implementation tricks and techniques that are invented in order to do their implementation of their idea maybe reusable knowledge for other systems research. So that's a key takeaway in any systems research and this true for this one as well.</li> 
  <li>In this particular case, the authors of GSM used a DEC Digital Equipment Corporation operating system as the base system. The operating system is called OSF/1 operating system. And there are two key components in the OSF/1 memory system, which is what we're talking about here.</li> 
  <li>First component VM: The first one is the virtual memory system, and this is the one that is responsible for mapping process virtual address space to physical memory, and worrying about page faults that happen when a process is trying to access the stack and heap and so on. So that it can bring those missing pages perhaps from the disk. And these pages are sometimes referred to as anonymous pages because a page is housed in a physical page frame and when a page is replaced, that same physical page frame may host some other virtual page and so on. So the virtual memory system is devoted to managing the page faults that occur for process virtual address space, in particular the stack and the heap.</li> 
  <li>Second component UBC:The unified buffer cache (UBC) is the cache that is used by the file system. Remember the file system is also getting stuff from the disk. But the file system wants to cache it in physical memories so that it is faster, so the unified buffer cash is serving as the extraction for the disk resident files when it gets into physical memory and user processes are going to do explicit access to files. When they do that, they're actually accessing unified buffer cache. So reads and writes of files go to this unified buffer cache. In addition to that, Unix systems offer the ability to map a file into memory, which is called memory mapped files. And if you have a memory mapped file, you can also have page faults to a file that has been mapped into memory. So the unified buffer cache is responsible for handling page faults to map files, as well as explicit read and write calls that an application process may make to the file system.</li> 
  <li>Normally this is the picture of how the memory system hangs together in any typical implementation of an operating system. You have the virtual memory manager, you have the unified buffer cache, you have the disk, and reads & writes from the virtual memory manager go to the disk, and similarly reads & writes from the unified buffer cache go to the disk</li>
   <li>Free list: when a physical page frame is freed, you throw it into the free list. So that it is available for future use by either the virtual memory manager or the unified buffer cache.</li>
   <li>Pageout Daemon: Pageout Daemon's role is to look at every once in a while what are pages that can be swapped out to the disk so that you make room for page faults to be handled without necessarily running an algorithm to free up pages.</li> 
  <li>So that's the structure of a memory management system, and what the authors of GMS did was to integrate GMS into the operating system, and this is where I said that there is heavy lifting to be done, because you are modifying the operating system to integrate an idea that you had for global memory system.
</li> 
</ul>


<h2>13. Implementation in Unix (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/14.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>This calls for modifying both the virtual memory part as well as the UBC (unified buffer cache) part of the original system to handle the needs of the global memory system. In particular, what you notice is that the VM and the unified buffer cache, when it wanted a missing page, it used to go to the disk. But here we're going to make it go to the GMS Because GMS knows whether a particular page that is missing in the address space of a process is present in some remote GMS. And similarly if a page is missing from the file cache, it knows whether that page is perhaps in the remote GMS. So that's why we modify the virtual memory manager and unified buffer cache manager to get their calls for getting and putting pages.</li> 
  <li>Getting page is when there is a page fault I need to get that page. I would go to the disk normally, but now I go to GMS. That GMS worry about getting it for me. Similarly, if a page is missing in the unified buffer cache, I go to the GMS to say "get me that page for me." And he will then do the work of finding out whether it is in remote GMS or if it is not anywhere in the cluster as we've seen in some of the cases. It could be under disk, it will get it from the disk.</li> 
  <li>Notice writes to disk is unchanged. Originally, whenever the VM manager decides to write to the disk, it's because a page is dirty. It writes to the disk. That part we don't want to mess with, because we are not affecting the reliability of the system in any way. Writes remain unchanged. Only when you have a page fault, you have to get it from the disk, you don't go to the disk anymore, but you come to GMS, and GMS then is integrated into the system so that it figures out where to get the missing page from.</li> 
  <li>Remember that the most important thing that we have to do in implementing this global LRU/approximation to global LRU is collecting age information. That's pretty tricky.</li>
   <li>Collecting age information with UBC: The case of file cache is pretty straightforward, because it is explicit from the process when it is making read & write calls, we can insert code as part of integrating GMS into this unified buffer cache, to see what pages are being accessed based on the region writes that are happening to the unified buffer cache. Because these calls are explicit from the application going into the operating system. And therefore, we can intercept those calls as part of integrating DMS to collect age information for the pages that are housed in the unified buffer cache.</li> 
  <li>Collecting age information with VM: On the other hand, VM is very complicated. Because memory access that a process does is happening in hardware on the CPU. The operating system does not see the individual memory access that a user program is making, so how does GMS collect age information for the anonymous? That's why these pages are called anonymous pages. In this case(UBC), the pages are not anonymous because we know exactly what pages are being reached in by a particular read or write by a file system, but the reads and writes that a user process is making to its virtual address space are the pages that are anonymous so far as the operating system is concerned. So, how does the operating system collect the age information? In order to do that, the authors of GMS what they did was to have a daemon part of the memory manager in the OSF/1 implementation. The daemon's job is to dump information from the TLB. If you recall, TLB, the translation lookaside buffer, contains the recent translations that have been done on a particular node. So the TLB contains information about the recently accessed pages on a particular processor. Periodically, say every minute, it dumps the contents of the TLB into a data structure that the GMS maintains so that it can use that information in deriving the age information for all the anonymous pages that are being handled by the VM. So that is how GMS collects the age information at every node. So this is where I meant, the technical details are very important. Because this idea of how to gather age information is something that might be useful if you're implementing a completely different facility.</li> 
  <li>Similarly, when it comes to the pageout daemon. The pageout daemon, normally, what it would do is, when it is throwing out pages. If they are dirty pages, it'll write it on to the disk. And if it is clean pages, it can simply discard it. But now, when it has a clean page and wants to discard a clean page, a pageout daemon will give it to GMS, because GMS will say, "well, don't throw it out. I'll put it into cluster memory so that it is available later on for retrieval rather than going to the disk."</li> 
  <li>So this is how GMS is integrated into this whole picture, interacting with the Unified Buffer Cache, interacting with the VM Manager, interacting with the Pageout Daemon and the Free List Maintenance as well in terms of allocating and freeing frames on every node.
</li> 
</ul>


<h2>14. Data Structures</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/15.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
   <li>Some more heavy lifting. Let's talk about the distributed data structures that GMS has in order to do this virtual memory management across the entire cluster.</li>
   <li>First thing that GMS has to do is to convert a virtual address, which is a local thing so far as a single processor's concerned. And convert that virtual address into a global identifier, or what we'll call as a universal ID (UID). And the way we derive the universal ID from the virtual address is fairly straightforward, if you think about it. We know which node this virtual address emanated from, IP address. We know which disk partition contains a copy of the page that corresponds to the virtual address. That we know. What are the i-node data structure that corresponds to this page? And what is the offset? So if you put all of these entities together, you get a universal ID that uniquely identifies a virtual address. This is the offset within a page. So given a virtual address, the first three parts uniquely identify the page, and the fourth part identifies the offset within that page for that virtual address. And this we can derive it from the virtual memory system as well as the UBC.</li> 
  <li>There are three key data structures: PFD (Page frame Directory), GCD (Global Cache Directory) and POD (Page Ownership Directory). These three data structures are the workhorses that make this cluster wide memory management possible. Let's talk about each one of these things.</li> 
  <li>PFD: PFD is like a page table. Normally in a page table what you do is you give it a virtual address and the page table says "oh, I know what is the physical frame that backs this particular virtual address." That is the translation between a virtual page number and a physical page frame that hosts that virtual page is contained in a page table. Similar to that, this PFD, it's called the page frame directory. It has a mapping between a UID, because your virtual address has been converted to a UID. Given a UID, it says, what is a page frame that backs that particular UID? That's this data structure. Because we're doing cluster-wide memory management, the page itself can be in one of four different states. It could be in the local part of that node. And if it is in a local part of a node. Then that page could be a private page or it could be a shared page. These are two possibilities, there is living in the local part of the physical memory of a node. If it is in the global part, we know by definition, global part is only hosting clean pages, so the content of global cache is always going to be private pages and so the state of the page that happens to be the global cache of a particular node is guaranteed to be a private state. And the last possibility is that it's not in the physical memory of a node, but it is on the disk. So the page frame directory just like a page table says that either this page that you are looking for go from VA to UID, that page is in physical memory and it is one of these three states or its not in the physical memory, it's on the disk.
</li> 
</ul>

<h2>15. Data Structures (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/15.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>GCD: Given a UID, I know that some PFD in the entire cluster has the mapping for the UID saying "What is the physical frame number that corresponds to it if it happens to be on that node or it's on the disk." That information is contained in some PFD in the entire cluster. So, if I have a page fault, I convert my VA to UID, then which PFD will I go lookup? I could broadcast to everybody and say, "how do you have it?" That will be very inefficient. You don't want to do that. Or we can say," There is some way of magically mapping, given a UID, which node will have the PFD for me to look up?" And find out the physical backing page. But we don't want to do a static binding of UID to the node that manages that UID. Because if we make a static mapping, then it pushes the burden on one node to take that if some page has become hot and everybody wants to use that. What we want to do is distribute this management function. Just like the age management, we did not want to concentrate it on a single node. We want to distribute the management of giving this mapping between UID and which node has the PFD that can tell me information about the missing page. And that data structure is this Global Cache Directory, GCD. GCD is a hash table, it's a cluster-wide hash table, so it's a distributed data structure. And the rule that GCD performs is given a UID, It will tell you which node the PFD has that corresponds to this UID. That's the role of this data structure. It's a partition hash table, so given a UID, I can say, "Well, I go to the GCD." And the GCD will say, "what is the PFD that has this UID?" Because of the partition hash table, even though a part of this GCD is on every node, every node may not be locally able to determine where the PFD is. Given a UID, it can go and it has to know which GCD it has to consult. </li> 
  <li>POD: To know which node has the PFD that corresponds to this UID, we need another data structure that tells us, given a UID, which node has the GCD that corresponds to this UID. And that is the Page Ownership Directory. So, the page ownership directory says, "Given a UID, which node has the GCD that corresponds to this UID." And this data structure the page ownership directory is replicated on all the nodes. It's an identical replica that is on all the nodes.</li>
   <li>So, when I have a page fault, first thing that I'm going to do, is go to my POD and that is a replicated data structure and it's completed & up to date information. So I go to this POD and asked this question, "Given this UID, how do I find out the global cache directory that has information about the PFD that can help me to map my virtual address to a physical address?" Remember that we could have simply gone from here(PFD) to here(POD), but that would have been a static mapping, and this one level of indirection (GCD) is giving a way by which we don't have to statically map a PFD to a UID, but this intermediate step, allows us to move the responsibility of hosting a particular PFD to different nodes, using this intermediary which is a distributive hash table.</li>
   <li>I said that this page ownership directory is replicated data structure. Can it change? Well it can change over time because what this page ownership directory is saying is the following: The UID space is something that spans the entire cluster. If you take the virtual addresses of all the possibilities of the entire cluster. That universe of all the virtual addresses is this UID space because it is being mapped from a a virtual address of a single process to this UID space and this spans the whole cluster and what we have done is we have partitioned that UID space into set of regions of ownership and that's what is called the page ownership. So every node is responsible for a portion of the UID space and that is this global cache directory. Now if the LAN never revolves. In other words, if the set of nodes on the LAN is fixed, the page ownership director also remains the same. But if nodes, if new nodes are added and deleted and so on, that's when the page ownership directory is going to change. And if that happens, then we have to replicate again, we have to redistribute the page ownership directly. But this is not something that's going to happen too often. It's very rare that node is going to come down or new node is going to be inserted into a LAN. And therefore this page ownership directory does not change very often. And that's where it's replicated data structure that you can believe at every node. But if it changes, there is also a way of handling that. We'll see that in a minute.</li> 
  <li>The path for page fault handling is if you have a page fault you convert VA to UID and once you have this UID then you can go to your page ownership directory that's on your node. And ask the question, "please tell me who has the PFD that corresponds to this UID?" And GCD is going to tell me "oh here is the note that contains the PFD for the UID that you're looking for". Then I can go to that PFD, and from that PFD I can get the page that I am looking for which might be in that note or it might say "well, it's not in my note any more. It's on the disk". So this is the path for page fault handling.
</li> 
</ul>

<h2>16. Putting the Data Structures to Work</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/16.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So now that we understand the data structures, let's put the data structures to work. Let's first look at what happens on a page fault.</li> 
  <li>On a page fault, the node first coverts the virtual address to a UID. And once it converts it to the UID, it goes to the page ownership directory. And as I mentioned, the page ownership directory is something that I can trust. It'll tell me, given this UID, I'll tell you who the owner of this page is. And you go to him because he has the GCD data structure for this. So, node A finds out the identity of the owner for this UID, and that happens to be node B. So it sends the UID over to node B.</li> 
  <li>Node B, because it is the owner for this UID, it looks up its GCD data structure and says, "oh, the PFD that can translate this UID which actually represents this virtual address is actually contained in this particular node, node C." So, that's the content of this data structure, given a UID what is the node ID that contains the PFD. Remember that PFD is equivalent of a page table in the normal system, and therefore that's the node that I want to send this UID to so that we can do the translation for this virtual address.So node B sends the UID over to node C.</li> 
  <li>Node C contains the PFD that has the mapping between the UID and the page frame number that is backed by this node (NodeC) for this UID, retrieves the page. It's a hit. Sends it over to node A. Node A is happy. It can then map this virtual address into its internal structure and the page fault service is complete and it can resume the process that was blocked for this page fault.</li> 
  <li>You can see that, potentially, when a page fault happens I can have three level of network communication. Of course the first lookup of the POD is local to my node because this POD data structure is replicated on every node. So from the UID, I can find the owner for the UID. But then I have to send a network message over to the node that contains the GCD for this UID. And then he'll then send it to the node that has this page so that that page can come back.</li>
   <li>Now this network communication (Node B -> Node C -> Node A) I am willing to tolerate because it is equivalent to performing the role of what the disk would have done. And maybe it is much better than going to a disk in order to get the missing page. So, it is happening only on page fault and since it is on a page fault, this network communication is okay. But, this (Node A -> Node B) is an extra network communication.</li>
   <li>Fortunately, the common case is a page fault is servicing the request of a local process on node A, and so the page is a non-shared page and if it is a non-shared page, most likely the UID space that corresponds to the missing page is also managed by this node (Node A). In other words, both the POD and the GCD corresponding to a particular UID is mostly on this node itself. And this is true for non-shared pages, and so A and B are exactly the same node. So there is no network communication to go from the POD to GCD to find out the PFD. So hopefully the only network communication that happens on every page fault is a network communication to go to this node that is serving the role of a disk and get the page from him. That's okay to incur because it probably is much lesser than going to the disk in order to do the page access. So the page fault service for the common case is going to be pretty quick.</li>
   <li>So the important message that I want you to get out of this is that even though these distributed data structures may make it look like you're going to have a lot of network communication, the important thing to note is that it happens only when there is a page fault. And since most of the page faults, the common case, is going to be non-shared pages. The data structure POD and GCD are probably co-resident on the same node. So, even though I've shown two different nodes here, A and B may be exactly the same node. So looking up the PFD that currently is backing this particular UID is going to be local to this node and so we can directly go to the node that contains the PFD and make the page fault service pretty quick.
</li> 
</ul>

<h2>17. Putting the Data Structures to Work (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/17.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now the interesting question is, once I go to the node that I think is going to give me the page that I'm looking for, or at least information about the page that I'm looking for. Whether it is, if it has it, it's going to give it to me, or if it doesn't have it, it's going to tell me that it is on the disk. But, in either case, I'm hoping I'll get exact information about this missing page from this node which is supposed to have the page foame directory, the PFD that corresponds with this UID. Is that possible that I go to this guy, and he says "no, I don't have it."? Yes, it is possible in two cases.</li> 
  <li>One case is, let's say, while this guy was sending this request over, this guy has made a decision to evict that page that corresponds to this UID because it had to make space for itself. In that case, that UID may have been thrown away from the PFD. And if it has been thrown away from the PFD, what he would have done is to inform the guy who has the ownership for this UID, this node is the owner for this UID. If he evicts that page this guy has to tell this node that, "hey, you know what, I used to back this UID in my PFD but I got rid of it. And I got rid of it by sending it to some other node, let's say, node D". So that is something that I have to communicate to this GCD, but it's a distributed system. Things are happening asynchronously. He may not have communicated that yet, that information is not there in the GCD of this node. This is the owner for this UID space. But the owner doesn't yet know that the PFD that corresponds to a particular UID has moved to some other node out here somewhere. And if it has moved to some other node, he will know about eventually, but he doesn't have it at this point. That's why this request was routed here, and this guy says "I don't have it." It can have a miss. That's one possibility.</li> 
  <li>Second possibility is the uncommon case that the POD information that I had is stale. When can that happen? That is when the POD is being recomputed for the local area network as a whole, either because there are new additions or new deletions of nodes. And therefore we are recomputing the redistribution of the UID space and deciding which node is responsible for which UID. That can happen. And in that case, it is possible that the information that I started with was incorrect. Because I went here thinking that he has the GCD, he did have it at that point, but it is changing. And eventually I'm going to find out. So if there is a mess, either case.</li>
   <li>The first case is, this guy replaced that page, or the second case is, my POD information misled me. Both cases, I'll have a miss. And I'll say, "oh, it was a miss. And I know that is probably the uncommon case. I'm going to retry that by looking up my POD again. And by that time, the POD may have been updated, I'll go to the right GCD this time." Or, "the GCD would have been updated and so I'll go to the same GCD, but the GCD will have the more relevant information of which PFD is currently hosting it. So, I'll go to him in order to get the page that I'm looking for."</li> 
  <li>The important point I want to leave you with is that the common case is when both the POD and GCD are coresident on the same node. And in that case, you don't have a network communication to look up the GCD, and also the miss happening when you do reach the PFD. That is also uncommon. It can because happen because of replacement that has happened on that node, or because the POD has changed. And this is something that is going to happen relatively infrequently compared to the activities that we're talking about in terms of page faults.
</li> 
</ul>

<h2>18. Putting the Data Structures to work (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/18.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>The next thing I want to talk about is what happens in the system on Page Evictions. Remember that on Page Eviction, when a Node decides that it wants to throw out a page, it sends it in the algorithm that I described to the Node, which is candidly ignored for hosting that page, and it might use the weight information to make the decision, of which node to send that page that it is discarding from it's own node. When a page fault happens, the key thing is to make sure that we service the page fault so that we get the page, and restart the execution that has been stalled on this node. That's the important thing to do.</li> 
  <li>The less important thing, but something that needs to happen in the universe of things that is being managed by GMS, is to also send the victim page from this node to the target node. That's part of the algorithm for eviction on page fault. However, we don't want to do it on every page fault, but we want to do it in an aggregated manner.For that reason every node has a paging daemon. This is typical of virtual memory systems that when a page fault happens, that's not the time the virtual memory manager is running around trying to find the page free. It always has a stash of free page frames to allocate to service this particular page fault. </li> 
  <li>As I mentioned earlier, the paging daemon in the virtual memory manager is integrated with the GMS system. And what the paging daemon is going to do is, when the free list falls below a threshold, then the paging daemon is going to do put page of the oldest pages on this node. And remember that in the integration of GMS with the virtual memory manager in the put page, I said that the Paging Daemon is also modified to work with the GMS. This is where the modification comes in. Normally what the Paging Daemon would've done is when the free list goes below threshold, it would take a bunch of pages, LRU candidate pages, and put it all onto the disk. But with the integration with GMS, what the paging demon is going to do is, for the same condition when the freelist falls below threshold, it's going to basically do putpage of the oldest pages that it has on this node. And when we do the putpage, we're going to choose the candidate node. Where we going to do the putpage based on the weight information that we got as part of the geriatric management that we talked about in the beginning. And so we do a putpage of UID into this PFD of this node so that this guy will be the one that will be backing this particular UID. And once I do this, I also have to update the GCD to indicate that the new PFD for this particular UID is C. So this update message is being sent to the owner of this UID. That is this node, and the owner the guy that has the portion of the UID space that is managed by this node. So the GCD data structure contains the mapping of the UID to the node that contains the PFD for that particular UID. And so I send this update message saying please update the GCD to indicate that this particular UID is now backed by PFD that sites on note C. So that's the information I'm sending to this guy. And this is not being done on every page eviction, but it is done by the paging demon in a aggregated, coordinated manner. When the free list falls below a threshold.</li> 
  <li>So we've covered a lot of ground from just sort of the principal behind the thought experiment. That is, using network memory as a paging device, rather than disc. Because the networks have gotten faster. And we came up with an algorithm for age management globally in the entire cluster, and how to have that age management done in a manner that doesn't burden any one node. But it picks the node that is lightest in terms of load at any point of time. And we also saw given the solution for cluster wide memory management for paging, how to go about implementing it. And all of the heavy lifting that needs to be done in order to take an idea and put it into practice.
</li> 
</ul>


<h2>19. Global Memory Systems Conclusion</h2>
<ul>
  <li>You'll see that in every systems research paper, there is heavy lifting to be done to take a concept to implementation. Working out the details and ensuring that all the corner cases are covered is a non-trivial intellectual exercise in building distributed subsystems. Like this one, the global memory system. What is enduring in a research exercise like this one? The concept of paging across the network is an interesting thought experiment, but it may not be feasible exactly for the environment in which the authors carried out this research, namely workstation clusters connected by a local area network. Each workstation in that setting is a private resource owned by an individual who may or may not want to share his resources, memory in particular. On the other hand, data centers today are powered by large scale clusters on the order of thousands of processes connected by a local area network. No node is individually owned in that setting. Could this idea of paging across the local area network be feasible in that setting? Perhaps. Even beyond the thought experiment itself what is perhaps more enduring are the techniques, distributed data structures and the algorithms, for taking the concept of implementation. In the next part of this lesson module, we will see another thought experiment to put cluster memory to use.
</li> 
</ul>

# L07b: Distributed Shared Memory

<h2>1. Distributed Shared Memory Introduction</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/19.JPG?raw=true" alt="drawing" width="500"/>
</p>


<ul>
  <li>In an earlier part of this lesson module, we saw how to build a subsystem that takes advantage of idle memory in peer nodes on a local area network, namely, using remote memory as a paging device, instead of the local disk. The intuition behind that idea was the fact that networks have gotten faster. And therefore access to remote memory may be faster than access to an electromechanical local disk.</li> 
  <li>Continuing with this lesson, we will look at another way to exploit remote memories, namely, software implementation of distributed shared memory. That is, create an operating system abstraction that provides an illusion of shared memory to the applications, even though the nodes in the local area network do not physically share memory.</li> 
  <li>So distributed shared memory asks the question, if shared memory makes life simple for application development in a multiprocessor, can we try to provide that same abstraction in a distributed system, and make the cluster look like a shared memory machine?
</li> 
</ul>


<h2>2. Cluster as a Parallel Machine (Sequential Program)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/20.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now suppose the starting point is a sequential program. How can we exploit the cluster? We have multiple processors, how do we exploit the cluster if the starting point is a sequential program?</li>
   <li>One possibility is to do what is called automatic parallelization. That is, instead of writing an explicitly parallel program, we write a sequential program. And let somebody else do the heavy lifting in terms of identifying opportunities for parallelism that exist in the program and map it to the underlying cluster. And this is what is called an implicitly parallel program. There are opportunities for parallelism, but the program itself is not written as a parallel program. And, now it is the onus of the tool, in this case an automatic parallelizing compiler, to look at the sequential program and identify opportunities for parallelism and exploit that by using the resources that are available in the cluster. </li> 
  <li>High Performance Fortran is an example of a programming language that does automatic parallelization, but it is user-assisted parallelization in the sense that the user who is writing the sequential program is using directives for distribution of data and computation. And those directives are then used by this parallelizing compiler to say, "oh, these are opportunities for mapping these computations onto the resources of a cluster." So it puts it on different nodes on the cluster and that way it exploits the parallelism that is there in the hardware, starting from the sequential program and doing the heavy lifting in terms of converting the sequential program to a parallel program to extract performance for this application.</li>
   <li>This kind of automatic parallelization, or implicitly parallel programming, works really well for certain classes of program called data parallel programs. In such programs, for the most part, the data accesses are fairly static, and it is determinable at compile time. So in other words, there is limited potential for exploiting the available parallelism in the cluster if we resort to implicitly parallel programming.
</li> 
</ul>


<h2>3. Cluster as a Parallel Machine (Message Passing)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/21.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So we write the program as a truly parallel program, or in other words, the application programmer is going to think about his application and write the program as an explicitly parallel program. And there are two styles of writing explicitly parallel programs. And correspondingly, system support for those two styles of explicitly parallel programs.</li> 
  <li>One is called message passing style of explicitly parallel program. The run time system is going to provide a message passing library which has primitives for an application thread to do sends and receives to its peers that are executing on other nodes of the cluster. So this message passing style of explicitly parallel program is true to the physical nature of the cluster. The physical nature of the cluster is the fact that every processor has its private memory. And this memory is not shared across all the processors. So the only way a processor can communicate with another processor is by sending a message through the network that this processor can receive. This processor cannot directly reach into the memory of this processor. Because that is not the way a cluster is architected. So, the messaging passing library is true to the physical nature of the cluster that there is no physically shared memory.</li> 
  <li>Lots of examples of message passing libraries that have been written to support explicit parallel programming in a cluster. They include MPI, message passing interface, MPI for short, PVM, CLF from digital equipment corporations. So these are all examples of message passing libraries that have been built with the intent of allowing application programmer to write explicitly parallel programs using this message passing style. And to this day, many scientific applications running on large scale clusters in national labs like Lawrence Livermore, and Argonne National Labs and so on, use this style of programming using MPI as the message passing fabric.</li> 
  <li>The only downside to the message-passing style of programming is that it is difficult to program using this style. If you're a programmer who's written sequential programs, the transitions paths to writing an explicitly parallel program is easier if there is this notion of shared memory, because it is natural to think of shared data structures among different threads of an application. And that's the reason making the transition from sequential program to parallel programming, using for instance the Pthread library on SMP is fairly intuitive and easy pathway. On the other hand, If the programmer has to think in terms of coordinating the activities on different processes by explicitly sending and receiving messages from their peers. That is calling for a fairly radical change of thinking in terms of how to structure a program.
</li> 
</ul>


<h2>4. Cluster as a Parallel Machine (DSM)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/22.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>This was the motivation for coming up with this abstraction of distributed shared memory in a cluster. The idea is that we want to give the illusion to the application programmer writing an explicitly parallel program that all of the memory that's in the entire cluster is shared. They are not physically shared, but the DSM library is going to give the illusion to the threads running on each one of these processes that all of this memory is shared. And therefore they have an easier transition path, for instance, from going from a sequential program or going from a program that they've written on an SMP to a program that runs on the cluster, because they don't have to think in terms of message passing. But they can think in terms of shared memory, sharing pointers across the entire cluster, and so on.</li>
   <li>Since we are providing a shared memory semantic in the DSM library for the application program, there is no need for marshaling and unmarshaling arguments that are being passed from one processor to another and so on. All of that is being handled by the fact that there is shared memory. So when you make a procedure call, and that procedure call is touching some portion of memory that happens to be on a remote memory. That memory is going to magically become available to the thread that is making the procedure call.</li>
   <li>In other words, the DSM abstraction gives the same level of comfort to a programmer who's used to programming on a true shared memory machine when they moved to cluster. Because they can use same set of primitives, like locks and barriers for synchronization, and the Pthread style of creating threads that will run on different nodes of the cluster. And that's the advantage of DSM style of writing an explicitly parallel program.
</li> 
</ul>


<h2>5. History of Shared Memory Systems</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/23.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now having introduced distributed shared memory, I want to give sort of a birds eye view of the history of shared memory systems over the last 20+ years. My intent is not to go into the details of everyone of these different systems, because that can take forever, but it is just to give you sort of the space occupied by all the efforts that have gone on in building shared memory systems both in hardware and in software. I encourage you to surf the web to identify papers and literature on these different systems that have been built over time, just to get a perspective on how far we have come in the space of building shared memory systems.</li> 
  <li>Software DSM: A few thoughts on the software side, software DSM was very first thought of in the mid 80s. The Ivy system that was built at Yale University and the Clouds Operating System that was built at Georgia Tech and there were similar systems built at UPenn. This I would say is the beginning of Software Distributed Shared Memory.Later on, in the early' 90s, systems like Munin and TreadMarks were built. I would call them perhaps a second generation of Distributed Shared Memory systems.In the later half of the 90s, there were systems like Blizzard, Shasta, Cashmere and Beehive. That took some of the ideas from the early 90s even further.</li> 
  <li>Structured DSM: And in parallel with the software DSM, I would say there was also a completely different track that was being pursued. And that is, providing structured objects in a cluster for programming. And systems such as Linda and Orca, were done in the early 90s. Stampede at Georgia Tech was done in concert with the Digital Equipment Corporation in the mid 90s and continued on, later on, into Stampede RT and PTS, and in fact, in a later lesson, we'll talk about Persistent Temporal Streams. And this particular axis of development of structured distributed shared memory is attractive because it gives a higher level abstraction than just memory to computations that needs to be built on a cluster.</li> 
  <li>Hardware DSM: Early hardware shared memory systems such as BBN Butterfly and Sequent Symmetry appeared in the market in the mid 80s and, the synchronization paper that we saw earlier by Mellor-Crummey and Scott used BBN Butterfly and Sequent Symmetry as the experimental platform for the evaluation of the different synchronization algorithms. KSR-1 was another shared memory machine that was built in the early 90s. Alewife was a research prototype that was built at MIT, DASH was a research prototype that was built at Stanford and both of them looked at how to scale up beyond an SMP, and build a truly distributed shared memory machine. And commercial versions of that started appearing. SGI silicon graphics built SGI origin 2000 as a scalable version of a distributed shared memory machine. SGI Altix later on took it even further, thousands of processors exist in SGI Altix as a large-scale shared memory machine. IBM Bluegene is another example. And today, if you look at what is going on in the space of high performance computing. It is clusters of SMPs which have become the work horses in data centers.</li> 
  <li>I very much want you to reflect on the progress that has been made in shared memory systems. And invite you to look at some of the details of machines that have been built in the past, either in the hardware or in software, so that you can learn the progress that has been made.
</li> 
</ul>


<h2>6. Shared Memory Programming</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/24.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>I've already introduced you to shared memory synchronization. Lock is a primitive and particularly the mutual exclusion lock is a primitive that is used ubiquitously in writing shared memory parallel programs to protect data structure so that one thread can exclusively modify the data and release the lock so that another thread can inspect the data later on and so on. And similarly, barrier synchronization is another synchronization primitive that is very popular in scientific programs and we have covered both of these in fairly great detail in talking about what the operating system has to do in order to have efficient implementation of locks as well as barriers.</li>
   <li>Now the upshot is, if you are writing a shared memory program, there are two types of memory accesses that are going to happen. One type of memory access is the normal reads and writes to shared data that is being manipulated by a particular thread. The second kind of memory access is going to be for synchronization variables that are used in implementing locks and barriers by the operating system itself. It may be the operating system, or it could be a user level threads library that is providing these mutual exclusion locks, or barrier primitives, but in implementing those synchronization primitives, those algorithms are going to use reads and writes to shared memory.</li>
   <li>So there are two types of shared memory accesses going on in the execution of a parallel program. One is access to normal shared data and the other is access to synchronization variables.
</li> 
</ul>


<h2>7. Memory Consistency and Cache Coherence</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/25.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Recall that in one of our earlier lectures, we discussed memory consistency model and the relationship of memory consistency model to cache coherence, in the context of shared memory systems. Memory consistency model is a contract between the application programmer and the system. It answers the when question, that is, when a shared memory location is modified by one processor, when, that is how soon that change is going to be made visible to other processes that have the same memory location in their respective private caches. That's the question that is being answered by the memory consistency model.</li>
   <li>Cache coherence, on the other hand, is answering the how question, that is, how is the system, by system we mean the system software plus the hardware working together, implementing the contract of the memory consistency model? In other words, the guarantee that has been made by the memory consistency model, to the application programmer has to be fulfilled by the cache coherence mechanism. So coming back to writing a parallel program, when accesses are made to the shared memory, the underlying coherence mechanism has to ensure that all the processes see the changes that are being made to shared memory, commensurate with the memory consistency model.
</li> 
</ul>


<h2>8. Sequential Consistency</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/26.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>I want you to recall one particular memory consistency model that I've discussed with you before, that is sequential consistency. And in sequential consistency, the idea is very simple. The idea is that every process is making some memory accesses, all of these, let's say, are shared memory accesses. And from the perspective of the programmer, the expectation is that, these memory accesses are happening in the textual order that you see here and that's the expecation so far as this programmer is concerned. Similarly, if you see the set of memory accesses that are happening on a different process of P2. Once again, the expectation is that the order in which these memory accesses are happening are the textual order.</li>
   <li>Now, the real question is, what happens to the accesses that are happening on one processor with respect to the accesses that are happening on another processor if they are accessing exactly the same memory location? For instance, P1 is reading memory location a, P2 is writing to memory location a. What is the order between this read by P1 and this write by P2? This is where sequential consitency model says that the interleaving of memory accesses between multiple processors, here I'm showing you two, but you can have n number of those processors, making accesses to shared memory all in parallel. When that happens you want to observe the textual program order for the accesses and the individual processes but the interleaving of the memory accesses coming from the different processors is arbitrary.</li>
   <li>So in other words, the sequential memory consistency model builds on the atomicity for individual read-write operations and says that, individual read-write operations are atomic on any given processor, and the program order has to be preserved. And, in order to think about the interleaving of the memory axises that are happening on different processors. That can be arbitrary and that should be consistent with the thinking of the programmer. I also gave you the analogy of a card shark to illustrate what is going on with a sequential consistency model. So the card shark is taking two splits of a card deck and doing a perfect merge shuffle of the two splits, and that's exactly what's going on with sequential consistency. If you can think of these memory accesses on an individual processor as the card split but instead of a two-way split you have an n-way split, and we are doing a merge way shuffle of all the n-ways. Splits off the memory accesses to get the sequentially consistent memory model.
</li> 
</ul>


<h2>9. SC Memory Model</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/27.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>With the sequentially consistent memory model, let's come back to a parallel program. So, a parallel program is making read write accesses to shared memory, some of them offer data, and some of them offer synchronization. Now, so far as the sequentially consistent memory model it does not distinguish between accesses coming from the processors as data accesses  or synchronization accesses. It has no idea. It only looks at the read write accesses coming from an individual processor and honoring them in the order in which it appears and making sure that they can merged across all these processors to preserve the SC guarantee.</li>
   <li>So the upshot is that there's going to be coherence action on every read write access that the model sees. If this guy writes to a memory location, then the sequentially consistent memory model has to ensure that this write is inserted into this global order somewhere. In order to insert that in the global order somewhere, it has to perform the coherence action with respect to all the other processors. That's the upshot of not distinguishing between normal data accesses and synchronization accesses that is inherent in the SC memory model.
</li> 
</ul>


<h2>10. Typical Parallel Program</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/28.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now, let's see what happens in a typical parallel program. In a typical parallel program that you might write, you probably get a lock, and you have, mentally, an association between that lock and the data structures that are governed by that lock. Or in other words, in writing your parallel program, you decided that access to variables a and b are governed by this lock. So if I wanted to read or write variables a and b, I'll get a lock and then I will mess with the variables that are governed by this lock. Once I'm done with whatever I want to do with these shared variables, I'll unlock indicating that I'm done. And this is my critical section. So within the critical section, and we're allowed to do whatever I want on these data structures that are governed by this particular lock, because that is an association I as the programmer has made in writing the parallel program.</li>
   <li>So if another processor let's say P2 gets the same lock. It's going to get the lock only after I release it. So only after I release the lock, this guy can get this lock because the semantics of the lock, it is a mutually exclusive lock. And therefore, only one person can have the lock at a time. And consequently, if you look at the structure of this critical section for P2, it gets a lock. And it is messing with the same set of data structures that I was messing with over here. But, by design, we know that either P1 or P2 can be messing with the data structure at any point of time. And that's a guarantee that I know comes from the fact that I designed the pilot program. And the lock is associated with these data structures. So, in other words, P2 is not going to access any of the data that is inside this critical section until P1 releases the lock.</li> 
  <li>We know this because we designed this program, but the SC memory model does not know about the association between these data structures and this lock. And, in particular, doesn't even know that memory accesses emanating from the processor due to this lock primitive is a different animal compared to the memory accesses coming from the processor as a result of accessing normal data structures.</li> 
  <li>So the cache coherence mechanism that is provided by the system for implementing the memory consistency model is going to be doing more work than it needs to do because it's going to be taking actions on every one of these accesses, even though the coherence actions are not warranted for these guys until I release the lock. So what that means is that there's going to be more overhead for maintaining the coherence commensurate with the SC memory model, which means it's going to lead to a poorer scalability of the shared memory system. So in this particular example since P2 is not going to access any of these data structures until P1 has released the lock there's no need for coherence action for a and b until the lock is actually released.
</li> 
</ul>


<h2>11. Release Consistency</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/29.JPG?raw=true" alt="drawing" width="500"/>
</p>
<ul>
  <li>This is the motivation for a memory consistency model, which is called release consistency. I'm sure just from the keyword release some of you may have already formed a mental model of what I'm going to say. Basically, we're going to look at the structure of the program as follows that the Parallel program consists of several different parallel threads P1 is one such, and if it wants to mess with some shared data structures, it is going to acquire a lock, we'll call it a1, and in the mind of the programmer there is an association between this lock and the data structures governed by it. So, so long as they hold the lock, they can modify the data structure and r1 is the release of this lock. So every critical section you can think of as composed of and acquire followed by data accesses governed by the lock and then release. If the same lock is used by some other process at P2, and if the critical section of P1 preceded the critical section of P2. Or in other words, P1's release operation P1-r1. The release operation and P1 happened before. This most be familiar to you from our discussion of Lampert's logical clock. P1-r1 happens before P2-r2, that is the acquire operation that is being done by P2 if this acquire operation for the same lock happened after the release by P1-r1. All we have to ensure is that all the coherence actions prior to this release of the lock by P1 has to be complete before we allow P2 to acquire this lock before we allow P2 to acquire the same lock L. That's the idea of release consistency.</li>
   <li>So we take the synchronization operations that are provided by the system whether it is hardware or software and we label them as either an acquired operation or a release operation. So, it's very straightforward when you think about mutual exclusion lock, acquiring the lock primitive is an acquire operation. And the unlock primitive is a release operation. So if there is a lock primitive and there is a preceding unlock primitive, so we have to ensure that all the coherence actions happen before I do the unlock, so that when this guy gets the lock and accesses the data, the data that he is going to see are going to be data that is consistent with whatever modifications may have been made over here. That's the idea behind the least consistent memory operation.</li>
   <li> Other synchronization operations can also be mapped to acquiring release. If you think about barrier, arriving at a barrier is equivalent to an acquire, and leaving the barrier is equivalent to a release. So, before leaving the barrier, we have to make sure that any changes that we made to shared data structures is reflected through all the other processes through the cache coherence mechanism. Then we can leave the barrier. So, leaving the barrier is a release operation, in the case of barrier synchronization. So, what that means is that, if I do a shared memory access within this group of sections, and that shared memory access would normally result in some coherence actions on the interconnect reaching to the other processes and so on, and if we use the SC memory model, you will block process P1 until that particular memory access is complete with respect to all the processors and the shared memory machine. But if we use the least consistent memory model, we do not have to block P1 in order for coherence actions to be complete to let the processor continue on with its computation. We only have to block a processor at a release point to make sure that any coherent actions that may have been initiated up until this point are all complete before we perform this release operation. That's the key point that I want you to get out of this release consistent memory model. So the release consistent memory model allows exploitation of computation on P1, with communication that may be happening through the coherence mechanism for completing the coherence actions corresponding to the memory accesses that you're making inside the critical section.
</li> 
</ul>


<h2>12. RC Memory Model</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/30.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So now we come back to our original parallel program and the parallel program is making normal data accesses and synchronization accesses. There are different threads running on all these processors. They're all making these normal data accesses and synchronization accesses.</li>
   <li>And if the underlying memory model is an RC memory model, it distinguishes between normal data accesses and synchronization accesses. And it knows that if there are normal read/write data accesses, it doesn't have to do anything in terms of blocking the processes. It may start initiating coherence actions corresponding to these data accesses, but it won't block the processor for coherence actions to be complete until it encounters a synchronization operation, which is of the release category. If a synchronization operation which is a release operation hits this RC memory model, it's going to say, "ah-ha. In that case all the data accesses that I've seen from this guy, I want to make sure that they're all complete globally communicated to all the processors." It's going to ensure that before allowing the synchronization operation to complete. So the coherence action is only when the lock is released.
</li> 
</ul>


<h2>13. Distributed Shared Memory Example</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/31.JPG?raw=true" alt="drawing" width="500"/>
</p>


<ul>
  <li>So let's understand how the RC memory model works with a concrete example. So let's say the programmer's intent is that one thread of his program is going to modify a structure A. And there is another thread that is going to wait for the modification, and then it is going to use the structure A. So this is the programmer's intent. So P2 is going to wait for the modification to use it, and this guy P1 is the guy that is modifying that particular structure A. And these are running different on processors, and therefore we don't know who may be getting to their code first.</li>
   <li>Let's say that P2 executes the code that corresponds to this semantic. That is, it wants to wait for the modification. So in order to do that, it has a flag, and this flag has a semantic with 0 indicating the modification is not done and 1 when the modification is actually done. To make sure that we don't do busy waiting, we use a mutual exclusion lock. We lock a synchronization variable, let's call it L. And if the flag is 0, then it is going to execute the equivalent of a pthread_cond_wait. pthread_cond_wait has the semantic that you're waiting on a condition variable and you're also releasing the lock that is associated with this condition variable. So you execute this pthread wait call, and the semantic you know is that at this point, thread P2 is blocked here, the lock is released, and he's basically waiting for a signal on this condition variable c. Who's going to do that, well, P1 is the guy that is modifying the structure, so it is the responsibility of P1 to signal him (P2). So let's see what happens.</li>
   <li>So P1 is executing the code for modifying the data structure A, and once it is done with all the modification, then it is going to inform P2. So in order to inform P2, what it does is acquires this lock L, and it sets the flag to 1. And the flag is 1 that I inspected over here (In P2) to know that, "oh, the modification is not yet done here, and I'm waiting on this condition variable." So, P1 sets the flag to 1 and signals on the condition variable c. And you know that signaling on the condition variable is going to wake up P2. And, of course, it cannot start executing here until P1 has released the lock, and once the lock has been released, that lock will be acquired implicitly by the operating system on behalf of P2, because that is a semantic of this condition wait here. So when I wake up, I'll go back, and as a defensive mechanism, I'll recheck the flag to ensure that the flag is now not 0 indicating that the modification has been done, so I'm now ready to get out of this critical section. I unlock L, come out of the critical section. Now I can use this modified data structure. So that's the semantic that I wanted, and I got that with this code fragment that I'm showing you here.</li>
   <li>So the important thing is, if you have an RC memory model, then all the modifications that I'm making here that are modifying shared data structures can go on in parallel. With all this waiting that may be going on here, I don't have to block the processor to do every one of these modifications. The only point at which I have to make sure that these modifications have been made globally visible is when I hit the unlock point in my code. So just before I unlock L, I have to make sure that all the read write accesses to shared variables that I've made here in my program have all been taken care of in terms of the coherence actions being communicated to all my peers. Only then, I have to unlock it. So, in other words, this code fragment is giving you pictorially the opportunity for exploiting computation in parallel with communication. If the model was an SC memory model, then for every read-write accesses that are being done in modifying this data structure A, there would have been coherence actions that would have gone on, and those coherence actions, each of them has to complete before you can do the next one and so on. But with the RC memory model, what it is allowing you to do is, you can do the data structure modification you want, and the coherence actions inherent in those modifications may be going on in the background, but you can continue with your computation until you hit this unlock point. At this point, the memory model will ensure that all the coherence actions are complete before releasing the lock, because once the lock is released, this guy's going to get it, and immediately he'll start using the data structure that has been modified by me. So it is important that all the coherence actions be complete prior to unlocking. So that's the intent of the RC memory model. And that's how you can exploit computation going on in parallel with communication if the memory model is an RC memory model.
</li> 
</ul>


<h2>14. Advantage of RC over SC</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/32.JPG?raw=true" alt="drawing" width="500"/>
</p>


<ul>
  <li>So to summarize the advantage of RC over SC, is that, there is no waiting for coherence actions on every memory access. So you can overlap computation with communication. So the expectation is that you will get better performance in a shared memory machine if you use the RC memory model compared to an SC memory model.
</li> 
</ul>


<h2>15. Lazy RC</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/33.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Now I'm going to introduce you to a lazy version of the RC memory model. And it's called LRC, stands for lazy RC. Now this is the structure of your pilot program so a thread acquires a lock, does the data accessing, releases the lock. Another thread may acquire the same lock. And if the critical section for P1 precedes the critical section for P2, then the RC memory model requires that, at the point of release, you ensure that all the modifications that have been made on processor P1, that is, the coherence actions. That are commensurate with those data acceses, are all communicated to all the peer processes including P2. Then you release the lock. That's the semantic. And this is what is called eager release consistency, meaning that at the point of release you're insuring that the whole system is cache coherent. The whole system is cache coherent at the point of release, then you release from the lock. And the cache coherence is with respect to the set of data accesses that have gone on on this process up to this point, that's what we are ensuring has been communicated and made consistent on all the processes.</li>
   <li>Now let's say that the timeline looks like this and P1's release happened at this point. And P2's acquire of the same lock happened at a much later point in time. That's the luck of the draw in terms of how the computation went, and so there is this time window between P1's release of the lock and P2's acquisition of the same lock. Now if you think about it, there's an opportunity for procrastination. Now we saw that procrastination often helps in system design. We've seen this in mutual exclusion locks. If you insert delay between successive trials of trying to get the lock, that actually results in better performance. We saw that in processes scheduling too. Instead of eagerly scheduling the next available task, maybe you want to wait for a task that has more affinity to your processor. That results in performance advantage. So procrastination often is a good friend of system design. So here again there is an opportunity for procrastination.</li>
   <li>So Lazy RC is another instance where procrastination may actually help in optimizing the system performance. The idea is that, rather than performing all the coherence actions at the point of release. Don't do it, procrastinate. Wait till the acquire actually happens. At the point of acquire, take all the coherence actions before allowing this acquire to succeed. </li>
   <li>So the key point is that you're deafening the point at which you ensure that all the coherence actions are complete to the point of acquisition as opposed to the point of release. Even if all the coherence actions commensurate with the data accesses that have gone on up until this release point, are not yet complete when we hit the release, go ahead. Release the lock, but if the next lock acquisition happens, at that point, make sure that all the coherence actions are complete. So in other words, you get an opportunity to overlap computation with communication once again in this window of time between release of a lock, and the acquisition of the same lock.
</li> 
</ul>


<h2>16. Eager vs Lazy RC</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/34.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So the Vanilla RC is what is called the eager release consistent memory model and the new memory model is called LRC or Lazy release consistent memory model. Let's see the pros and cons of LRC with respect to Vanilla RC, or Lazy RC with respect to Eager RC.What I am showing you here are timelines of processor actions on three different processors, P1, P2, and P3. </li> 
  <li>This (top) picture is showing you what happens in the Eager version of the RC model in terms of communication among the processors. So when processor P1 has completed its critical section, does the release operation, at the release point what we're going to do is all the changes that we made, in this example I'm showing you to make it simple, I'm showing you that in this critical section that I wrote in this variable x, so the changes to x is going to be communicated to all the processors P2 and P3. It could be depending on whether it is an invalidation based protocol or an update based protocol, what we are saying is we are communicating the coherence action to all the other processors. That's what these arrows are indicating. Now then P2 acquires the lock, and after it acquires the lock it does its own critical section. Again, let's say we're writing to the same variable x, and it releases the lock. And at the point of release once again we broadcast the changes that we made. Notice what is going on. P1 makes modifications, broadcasts it to everybody. But who really needs it? Well, only P2 needs it. But unfortunately the RC memory model is Eager, and it says "I'm going to tell everybody that has a copy of x that I have modified x." And so it's going to tell it to P2. It's going to tell it to P3 as well. P3 doesn't care, because it's not using that variable yet, and P2 cares, and it of course is using that. But when it releases its critical section, it's once again going to do exactly the same thing that happened over here, and that is it's going to broadcast the changes it made to shared memory locations to all the other processes, in this case P1 and P2 (Actually should be P1 & P3). And then, finally, P3 does its acquire and then reads the variable. So, all these arrows are showing you the coherence actions that are inherent in the completion of shared memory accesses that are happening in the critical section of programs.</li> 
  <li>Now let's move over to the Lazy version. In the Lazy version, what we are doing is when we release a lock, we are not doing any global communication. We simply release a lock. Later on, the next process that happens to acquire that same lock. The RC memory model. The first thing it's going to say is, "oh, you want to get this lock? I have to go and make sure that I complete all the coherence actions that I've associated with that particular lock. In this case the previous lock holder had made changes to the variable x, so I'm going to pull it from this guy and then I can execute my critical section." And then when P3 executes its critical section, it's going to pull it from P2 and complete what it needs to do. So, the important thing that you see is that there is no broadcast anymore. It's only point-to-point communication that's happening between the processors that are passing the lock between one to the other. So, in other words, the number of arrows that you see are communication events. You can see that there's a lot more arrows here. Forget about the arrows that I introduced. But the black arrows that you see are the arrows that are indicating communications commensurate with the coherence actions needed for this set of critical section actions. And correspondingly, the black arrows here are showing the communication actions for the same set of critical section actions shown in both the top and the bottom half of this particular figure.</li> 
  <li>You can see, there's a lot less communication happening with the Lazy model. It's also called a pull model, because what we're doing is at the point of acquisition, we're pulling the coherence actions that need to be completed over here. Whereas, this is the push model in the sense that we're pushing all the coherence actions to everybody at the point of release. Having introduced the Eager and the Lazy RC models, it's time for a quiz.
</li> 
</ul>


<h2>17. Pros and Cons of Lazy and Eager</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/35.JPG?raw=true" alt="drawing" width="500"/>
</p>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/36.JPG?raw=true" alt="drawing" width="500"/>
</p>

<br>
<br>
<p><b>Note: Regarding lazy & eager RC model</b></p>
<p> Paper: Keleher, P., Cox, A. L., & Zwaenepoel, W. (1992). Lazy release consistency for software distributed shared memory. ACM SIGARCH Computer Architecture News, 20(2), 13-21.</p>
<p>A system is release consistent if: 1) before an ordinary access is allowed to perform with respect to any other processor, all previous acquires must be performed 2)before a release is allowed to perform with respect to any other processor, all previous ordinary reads and writes must be performed 3) special accesses are sequentially consistent with respect to one another </p>
<br>
<p>Eager release consistency: A processors delays propagating its modification to shared data untile it comes to a release. At that time, it propagates the modifications to all other processors that cache the modified pages. No consistency related operations occur on an acquire. </p>
<br>
<p>Lazy release consistency: The propagation of modifications is further postponed until the time of the acquire. At this time,the acquiring processor determines which modifications it needs to see according to the definition of RC. It is an algorithm for implementing release consistency that lazily pulls modifications across the interconnect only when necessary. The simulation in the paper indicates that lazy release consistency reduces both the number of messages and the amount of data transferred between processors. These reductions are especially significant for programs that exhibit false sharing the make extensive use of locks.</p>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/note1.JPG?raw=true" alt="drawing" width="500"/>
</p>
<br>
<br>

<h2>18. Software DSM</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/37.JPG?raw=true" alt="drawing" width="500"/>
</p>
<ul>
  <li>So far, we've seen three different memory consistency models. One is the sequential consistent memory model, the release consistent memory model. Strictly speaking, I would say, the eager version and the lazy version are just variants of the same memory model, namely the release consistent memory model. Now we're going to transition and talk about software distributed shared memory, and how these memory models come into play in building software distributed shared memory.</li> 
  <li> We're dealing with a computational cluster, that is, in the cluster, each node of the cluster has its own private physical memory, but there is no physically shared memory. And therefore, the system, meaning the system software, has to implement the consistency model to the programmer. In a tightly coupled multiprocessor, coherence is maintained at individual memory access level by the hardware. Unfortunately, that fine-grain of maintaining coherence at individual memory access level will lead to too much overhead in a cluster. Why? Because on every load or store instruction that is happening on any one of these processors, the system software has to butt in and implement the coherence action in software through the entire cluster. And this is simply infeasible. So what do we do to implement software distributed shared memory?</li> 
  <li>The first thought is to implement this sharing and coherence maintenance at the level of pages. So the granularity of coherence maintenance is at the level of a page. Now, even in a simple processor or in a true multiprocessor, the unit of coherence maintenance is not simply a single word that a processor is doing a load or a store on. Because in order to exploit spatial locality, the block size used in caches in processors tend to be bigger than the granularity of memory access that is possible from individual instructions in the processor. So we're taking this up a level and saying, "if you're going to do it all in software, let's keep the granularity of coherence maintenance to be an entire page." And you're going to maintain the coherence of the distributed shared memory in software by cooperating with the operating system that is running on every node of the processor. So what we're going to do is, we're providing a global virtual memory abstraction to the application program running on the cluster. So the application programmer views the entire cluster as a globally shared virtual memory. Under the cover, what the DSM software is doing is partitioning this global address space into chunks that are managed individually on the nodes of the different processors of the cluster. From the application point of view, what this global virtual memory abstraction is giving is address equivalence. And that is, if I access a memory location x in my program, that means exactly the same thing whether I access the memory location x from processor 1, processor 2, and so on and so forth. That's the idea in providing a global virtual memory abstraction. And the way the DSM software is going to handle maintenance of coherence is by having distributed ownership for the different virtual pages that constitute this global virtual address space. So you can think of this global virtual address space as constituted by several pages, and we're going to say some number of these pages are owned by processor 1. Some number of these pages are owned by processor 2. Some number by processor 3, and so on. So we split the ownership responsibility into individual processors. Now what that means, is that the owner particular page is also responsible for keeping complete coherence information for that particular page and taking the coherence actions commensurate with that page. And the local physical memories are available in each one of this processors is being used for hosting portions of the global virtual memory space in the individual processors commensurate with the access pattern that is being displayed by the application on the different processors. So for instance, if processor 1 accesses this portion of the global virtual memory space, then this portion of the address space is mapped into the local physical memory of this processor. So that a thread that is running on this processor can access this portion of the global address space. And it might be that same page is being shared with some other processor n over here. In that case, a copy of this page is existing in both this processor, as well as this processor. Now it is up to the processor that is responsible for the ownership of this particular page to worry about the consistency of this page, that is now resident in multiple locations. For instance, if this node, let's say, is the owner for this page. Then this node will have metadata that indicates that this particular page is currently shared by both P1 and Pn. So that is the directory that is associated with the portion of the global virtual memory space that is being owned and managed by this particular processor. So statically, we are making an association between a portion of the address space and the owner for that portion of the address space in terms of coherence maintenance for that portion of the global virtual memory space.
</li> 
</ul>


<h2>19. Software DSM (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/38.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So this is the abstraction layer seen by the application that is giving this illusion of a global virtual memory. This layer is the DSM software implementation layer that implements this global virtual memory abstraction. In particular, this DSM software layer, which exists on every one of these processors, knows that the point of access to a page by a processor, who exactly to contact, as the owner of the page, to get the current copy of the page. For instance, let's say that there was a page fault on this processor one. For a particular portion of the global address space. That portion of the global address space is currently not resident here in the physical memory of processor one. So, there is a page fault over here and there is cooperation, as I mentioned earlier, between the operating system and the DSM. So, when the page fault happens, that page fault is going to be communicated by the operating system to the DSM software saying that here is the page fault, you handle it. What the DSM software is going to do is, it knows the owner of the page, and so it's going to contact the owner of the page. And ask the owner of the page to get the current copy of the page. So the current copy of the page resides over here. So the owner, either it itself has the current copy of the page or it knows which node currently has the current copy of the page, and so it's going to send the page over to the node that is requesting it. The current copy of the page is over here. It's going to come over to this guy, and recall what I said about ownership of the pages. The residency of this page doesn't necessarily mean that this is the owner of the page. The owner of this page could have been this node, so the DSM software would have contacted the owner. And the owner would have said, oh you know what. That particular place to cut and copy is on this node. So the DSM software would go to this note, and fetch this page, and put it into this processor. So that this processor is happy. So once the page has been brought in to the physical memory, then the DSN software contacts the virtual memory manager and says, I've completed now, processing the page fault, brought the page that is missing and put it into a particular location in the physical memory. Would you please update the page table for this guy, so that he can resume execution? Well, then the VM manager gets into action. And updates to page table for this thread to indicate that the faulty virtual page is now mapped to a physical page, and then the process or the thread can resume its execution. So this is a way coherence is going to be maintained by the DSM software. The cooperation between DSM software and the VM manager. And the coherence maintenance is happening at the level of individual pages. An early examples of systems that built software DSM include Ivy from Yale, Clouds from Georgia Tech Mirage from UPenn, and Munin from Rice. All of these are distributed shared memory systems and they all used coherence maintenance at the granularity of an individual page, and they used a protocol which is often referred to as a single writer protocol. That is, I mentioned that the directory associated with the portion of the virtual memory space managed by each one of these nodes and the directory has information as to who all are sharing a page at any point of time. Multiple readers can share a page at any point of time, but a single writer is only allowed to have the page at any point of time. So, if there is the writer for a particular page, let's say that loose page, which is now currently in the memory of two different processors, if this guy wants to write to this page, then he has to inform through the DSM software abstraction, inform the owner for this page. Let's say this guy is the owner for this page, that I want to write to this page and at that point the owner is going to invalidate all copies of that page that exist in the entire system, so that this guy has exclusive access to that page so that they can make modifications to it. So this is what is called single writer multiple reader protocol. Easy to implement. At the point of right to a page, what you do is, you go through the DSM software, contact the owner. And the owner says, I know who all have copies of the page, I'll invalidate all of them. Once it has invalidated all the copies, then the guy who wants to write to that page can go ahead and write to it, because that'll be the only copy. Now the problem with the single writer protocol is that there is potential for what is called fault sharing. We've talked about this already in the context of shared memory multiprocessors. Basically the idea of fault sharing, or the concept of fault sharing, is that data appears to be shared even though programmatically, they are not. Let's consider this page-based coherence maintenance. In the page-based coherence maintenance, the coherence maintenance that is done by the software, DSM software, is of the granularity of a single page. A page may be 4K bytes or 8K bytes,depending on the architecture we're talking about. And within a page, lots of different data structures can actually fit. So, if the coherence maintenance is being done at the level of an individual page, then we're invalidating copies of the page in several nodes in order to allow one guy to make modifications to some portion of that page. And that can be very severe in a page based system due to the coarse granularity of the coherence information. So for example, this one page may contain ten different data structures, each of which is governed by a distinct lock. So far as the application programmer is concerned. But, even if I get a lock, which is for a particular data structure that happens to be in this page. And this guy has a lock for a different data structure which is also on the same page. When I get the lock that is going to manipulate a particular data structure in this page, and if I want to make modifications for it, I am going go and invalidate all the other copies. When he wants to make a change, he's going to come and invalidate my copy of the page. So the page can be ping-ponging between, between multiple processes. Even though they are modifying different portions of the same page, still the coherence granularity being a page, will result in this page shuttling back and forth between these two guys, even though the application program is perfectly well behaved in terms of using locks to govern access to different data structures. Unfortunately, all of those data structures happened to fit within the same page. Resulting in this fault sharing. So, page level granularity and single writer multiple reader protocol don't live happily together. They will lead to fault sharing. And they will lead to ping ponging of the pages, due to the fault sharing among the threads of the application across the entire network.
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

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->






<!-- <h2></h2>

<p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->
