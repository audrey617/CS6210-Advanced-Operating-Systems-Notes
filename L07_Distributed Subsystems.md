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
  <li>Welcome back. We started the discussion of the distributive systems with definitions and principles. We then saw how object technology, with its innate concepts of inheritance and reuse, helps in structuring distributive services at different levels. In this next lesson module, we will discuss the design and implementation of some select distributed system services. Technological innovation come when one looks beyond the obvious and immediate horizon. Often this happens in academia because academicians are not bound by market pressures or compliance to existing product lines. You'll see this out of the box thinking in the three subsystems that we're going to discuss as part of this lesson module. Another important point, often the byproducts of a thought experiment may have more lasting impact than the original vision behind the thought experiment. History is ripe with many examples. We will not have the ubiquitous yellow sticky note, but for space exploration. Now close to home, for this course, Java would not have happened but for the failed video-on-demand trials of the 90s. In this sense the services we are going to discuss as part of this lesson module, while interesting in their own right, may themselves be not as important as the technololgical insights they offer on how to construct complex distributed services. Such technological insights are the reusable products of most thought experiments. 
</li> 
  <li>There's a common theme in the three distributed subsystems we're going to discuss. Memory is a critical, precious resource. With advances in network technology leveraging the idle memory of a peer in a Local Area Network may help overcome shortage of memory, locally available in a particular note. The question is how best to use the cluster memory, that is the physical memory of peers, tend to be idle at any particular point of time in a local area network. </li> 
  <li> Global memory system, GMS, asks the question,how can we use the peer memory for paging across the Local Area Network. And later on we will see DSM, which stands for distributed shared memory, asks a question, if shared memory makes the life of application programmers simple in a multiprocessor, can we try to provide the same abstraction in a cluster and make that cluster appear like a shared memory machine. And a third work that we will see is this distributed file system, in which we ask the question, can we use the cluster memory for cooperative caching of files. Anyhow, back to our first part of this three-part lesson module, namely the Global Memory System.
</li> 

</ul>

<h2>2. Context for Global Memory System</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/2.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Typically, the virtual address space of a process running on a processor, let's say your desktop, your PC, your laptop, and so on, is much larger than the physical memory that is allocated for a particular process. So the role of the virtual memory manager in the operating system is to give the illusion to the process that all of its virtual address space is contained in physical memory. But, in fact, only a portion of the virtual address space is really in the physical memory. And that's called the working set of the process. And the virtual memory manager supports this illusion for the process by paging in and out from the disk the pages that are being accessed by a process at any particular point of time so that the process is happy in terms of having its working set contained in the physical memory. </li> 
  <li>When a node, a desktop, is connected on a local area network to other peer nodes that are also on the same local area network it is conceivable that at any point of time, the memory pressure, meaning the amount of memory that is required to keep all the processes running on this node happy, may be different from the memory pressure on another nodes. In other words, this particular node may have a much higher memory pressure because the work load on this load consumes a lot of the physical memory whereas this work station may be idle and therefore all of this memory is not being utilized at this point for running anything useful because no applications are running on these nodes. So this opens up a new line of thought.</li> 
  <li>Given that all these nodes are connected on a local area network and some nodes may be busy while other nodes may be idle. Is it possible if a particular node experiences memory pressure, can we use the idle cluster memory, and in particular, can we use the cluster memory that's available for paging in and out the working set of the processes on this node? Rather than going to the disk, can we page in and out to the cluster memories that are idle at this point of time? It turns out that with advances in local area networking gear, it's already made possible for gigabit ethernet connectivity to be available for desktops. And pretty soon, 10 gigabit links are going to be common in connecting desktops to the local area network. This makes sending a page to a remote memory or fetching a page from a remote memory faster than sending it to a local disk. Typically, the local disk access speeds are in the order of 200 megabytes per second in terms of transfer rate, but on top of that, you have to add things like seek latency and rotation latency for accessing the data that you want from the disk. So all of this augers well for saying that perhaps paging in and out through the local area network to peer memories that are idle may be a good solution when we have memory pressure being experienced at any particular node in the cluster.</li> 
  <li>The global memory system, or GMS for short, uses cluster memory for paging across the network. In normal memory management, if virtual address to physical address translation fails then the memory manager knows that it can find this virtual address on the disk, meaning the page that contains this virtual address on the disk, it pages in that particular page from the disk. Now in GMS, what we're going to do is, if there is a page fault, that is, this translation, virtual address to physical address, fails then that means that the page is not in the physical memory of this node. In that case GMS is going to say, well it might be there in the cluster memory, in one of the nodes of my peers, or it could be on the disk if it is not in the cluster memory of my peers. So that's the idea that GMS is sort of integrating the cluster memory into this memory hierarchy.</li> 
  <li>Normally, when we think about memory hierarchy in a computer system, we say there's a processor, there's the caches, and there's the main memory, and then there is the virtual memory sitting on the disk. But now in GMS, we're sort of extending that by saying in addition to the normal memory hierarchy that exists in any computer system, that is processor, caches and memory, there is also the cluster memory. And only if it is not in the cluster memory, we have to think of going to the disk in order to get the page that we're looking for. That's sort of the idea.</li> 
  <li>So GMS trades network communication for disk I/O. And we are doing this only for reads, for reading across a network. GMS does not get in the way of writing to the disk, that is the disk always has copy of all the pages. The only pages that can be in the cluster memories are pages that have been paged out that are not dirty. And if there are dirty copies of pages, they are to be written onto the disk just like it will happen in a regular computer system. In other words, GMS does not add any new causes for worrying about failures because even if a node crashes all that you lose is clean copies of pages belonging to a particular process that may have been in the memory of this node. But those pages are on the disk as well. So the disk always has all the copies of the pages. It is just that the remote memories of the cluster is serving as yet another level in the memory hierarchy.</li> 
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
  <li>I mentioned that we are going to use peer memories as a supplement for the disk. In other words, we can imagine that the physical memory at every node to be split into two parts. One part is what we'll call "local", and local contains the working set of the currently executing processes at this node. So this is the stuff that this node needs in order to keep all the processes running on this node happy. Now, the "global" is similar to a disk. This global part is where community service comes in, that is, I'm saying that out of my total physical memory, this is the part that I need to keep all the processes happy in my node, and this is the part that I'm willing to use a space for holding pages that are swapped out from my fellow citizens on the local data network. And this split of local and global is dynamic in response to memory pressure. As I mentioned earlier, the memory pressure is not something that stays constant, right? So over time, depending on what's going on in a particular node, you may have more need for holding the working set of all your processes, in which case the local part may keep increasing. On the other hand, if I go off for lunch, my workstation is not in use. And in that case, my local part is going to shrink, and I can house more of my peers swapped out pages in my global part of the physical memory.</li> 
  <li>So the global part is a spare memory that I'm making available for my peers and local part is the part that I need for holding the working set of the currently active process at my node, and this boundary keeps shifting depending on what's going on at my node. Pretty simple, normally if all the processes executing in the entire local area network are independent of one another, all the pages are private. You know, I'm running a process, my process, my pages, and the contents of that pages are private to my process. On the other hand, you could also be using the cluster for running an application that spans multiple nodes of the cluster, in which case it is possible that a particular page is shared, and in that case, that page will be in the local part of multiple peers, because multiple peers are actively using a page.</li> 
  <li>We have two states for a particular page. It could be private, or it could be shared. If a page is in the global part of my physical memory, then it is guaranteed to be private. Because the global part is nothing different from a disk. So when I swap out something, I throw it onto the disk. Similarly, when I swap out something in GMS, I throw it into my peer memory as the global cache, and therefore what is in the global cache is always going to be private copies of pages, whereas what is in the local part can be private or can be shared, depending on whether that particular page is being actively shared by more than one node at a time.</li> 
  <li>Now one important point, the whole idea of GSM is to serve as a paging facility. In other words, if you think about even a uni processor, if it had multiple processes sharing a page, the virtual memory manager has no concern about the coherence about the pages that are being shared by multiple processes, that's the same semantic that is used in GSM, and that is coherence. For shared pages, it's outside GSM, it's an application problem. If there are multiple copies of the pages residing in the local parts of multiple peers, maintaining the coherence is the concern of the application. That is not GSM's problem. Only thing that the GSM is providing is a service for remote at. That's important distinction that one has to keep in mind. In any workshop memory management system, what you do is when you have to throw out a physical page, you use a page replacement algorithm and the page replacement algorithm that is typically employed in computer systems is some variant of an LRU or a least recently used algorithm. GSM also does exactly the same thing, except it integrates cluster memory management of the lowest level across the entire cluster. So the goal in picking a replacement candidate in GSM is to pick the globally oldest place for replacement. If let's say that the memory pressure in the system is such that I have to throw out some page from the cluster memory onto the disk. The candidate page that I'll choose is the one that is oldest in the entire cluster.</li> 
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
  <li>The key take away for you is that, for this particular common case, the memory pressure pressure on P is increasing, so the local allocation of the physical memory goes up by one, and the global allocation (the community service part) goes down by one on host P. Where as on host Q, it remains unchanged because all that we have done is we have traded Y for X.
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
  <li>The interesting part is, if the oldest page on host R happens to be in the global cache of R, what can we say about that page z? Well, it has to be cleaned, because global part is nothing but a paging device. And therefore if it is here, it must be cleaned, and therefore, I can discard it without worrying about it. Just simply dump it. Drop it on the floor. That's what I'm going to do. On the other hand, if it is on the local part, it could be dirty. That is, if the oldest page happens to be on host R, and it also happens to be in the local part of host R, it is conceivable that this page has been modified, in which case, that modified copy has to be written out to the disk. So in other words, when we pick host R to send this replacement page from my host, this guy, what he's going to do is, he's going to say, "I have to get rid of a page in order to make room for this incoming global page, because I have the globally oldest page. And if I have the globally oldest page, then let me get rid of it by throwing it out onto the disk if it happens to be dirty. If it happens to be clean, simply drop it on the floor, because I know that all the pages are contained on the disk." </li> 
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
  <li>What the initiator node is going to do is two things. It's going to find out what is the minimum age of all the M pages that are going to be replaced. Remember that smaller the age, the better. So what it is going to say is out of all the pages that exist in the entire cluster, what are the oldest M pages that are going to be replaced in the upcoming epoch, and for those M old pages, out of those M old pages, what is the minimum age? So any page with less than the minimum age are pages that are active and that are going to survive this upcoming epoch. Whereas, any page whose age is older than that minimum age is part of this set of M pages that are going to be replaced in the upcoming epoch, and those are the replacement candidates. That's minimum age. So it computes the minimum age. And it also computes given the minimum age and given the distribution of the age demographics coming from N1, I know out of these pages coming from N1 what fraction of the pages that belong to N1 are going to be replaced in the upcoming epoch. And that is the weight parameter for this particular node. For instance, if it turns out that N1 has 10% of the candidate pages that are going to be replaced in the next epoch, then its weight is 0.1. If N2 is going to account for 30% of the replacements, 30% of the M replacements in the upcoming epoch. Then N2's weight is going to be W2 and so on. So what this initiative does is it computes this min age and it also computes the weight for each one of the nodes, and that's what is sent back to each node. So each node is going to see the min age and also the weight. Each load is not only receiving its own weight, that is its own fraction of the M pages that are going to be replaced, but it is also getting the fraction of the pages that are going to be replaced from each of its peer nodes in the entire cluster. And we'll see how this information is going to be used by each one of these nodes. And of course, we don't know the future, all that the initiator is doing. That is saying that it is expected replacement. W1 is expected share of replacement that's going to happen in N1. W2 is expected share of replacement that's going to happen in N2 and so on. And when the next epoch starts actually. That can be different, depending on what happens in these nodes. But, that's the best that we can do is use the past to predict the future. That's what the initiates doing. It is using the past, the age information that it got from all the notes, in order to predict the future in terms of where these N replacements are going to happen. What is the minimum age of the pages that are going to be replaced in the next epoch. So that's what is being done by the initiator.
</li> 
</ul>

<h2>11. Geriatrics! (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/12.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>I mentioned that this management function that this initiator is doing, you don't want it to be statically assigned to one node, because that would be a huge burden, and besides this node will suddenly become very active, and if that happens, then you must time in the computations that are going to happen here to do this management function. Now intuitively if you think about it, if a particular node, lets say node N2 has a weight of point eight, what does that mean? What that means is that in the upcoming Epoch, 80% of the M pages are going to be replaced from the node N2, in other words, N2 is hosting a whole bunch of senior citizens, so it's a node that's not very active. Right? If an inactive node, then the replacement's candidates are probably not going to come from that guy and in predicting the future the initiator is saying that well, most of the replacements are going to come from here, so it's weighted very high. So this weight information for all the nodes is coming to every one of these guys. So intuitively if we think about it, the guy that has the highest weight is probably the least active. Why not make him the manager? So the initiator for the next Epoch is going to be the node with the maximum weight. Now how do you determine that? Well, every one of these nodes is receiving not only the min age of the candidate pages that are going to be replaced in the next epoch but also the weight distribution that says what is the percentage of these replacements that are going to happen in each one of these nodes. So locally each one of these guys can look at the Wi vector that it got back from the initiator and say, "I know, given that node N3 has the maximum rate, that's going to be the initiative of the next epoch." So locally you can make the determination so that for the next epoch, in order to send the age information, you know locally who to send it to.</li> 
  <li>Let's look at how this minimum age information is going to be used by each node. Remember that min age represents the minimum age of the M oldest pages. The smaller the age, the more recent that page is. And so those are the active pages. So if you look at this as the age distribution of all the pages, then if you draw a line here, then the pages to the left of the minimum age line are the active pages, and the pages to the right of this is the M oldest pages that are the candidate pages going to be replaced in the next epoch. This is the information that we're going to use in the way we manage page replacements at every node.</li> 
  <li>So let's see what the action is at a node when there is a page fault. If upon a page fault I have to evict a page y, then what I'm going to do is to look at the age of this page y. If the age of this page y is more than the minimum wage. Then I know it's going to be replaced. Even if I sent it to a peer node, it's going to be replaced because that is part of the candidate M pages that are going to be replaced in the upcoming epoch. And therefore locally I'll make a decision that page y has an age older than minimum age, and therefore, I'm going to simply discard it. Don't worry about sending it to a peer. Remember that, in the description of how a page fault is handled, I said that, when you have a page fault in the node, I pick one of the replacement candidates, and I send it to a peer node to store it in the global cache of the peer node. Well, we don't want to do that. If that page is going to be eventually replaced during this upcoming epoch, meaning it has to be thrown out onto the disk. In that case, you might as well discard it right now. So if the age of the page that you're evicting from your node happens to be greater than MinAge, simply discard it.</li> 
  <li>On the other hand, if the page happens to be less than the MinAge, then you know that it is part of the active set of pages for the entire cluster, you cannot throw it away. You send it to Peer Node Ni. How do you pick Ni? This is where the weight distribution comes in. I know the weight of the nodes. At the end of this computation, the manager sent me the weight distribution for the upcoming epoch of all the nodes. So I can pick a node to send this page to based on the weight distribution. Of course I could say, well, send it to the guy that has the highest weight because that's the guy that is likely to replace, but remember that there's only an estimate of what is going to happen in the future. So, you don't want to hard code that. Instead, we're going to use some information drawn from these weights. We're going to factor that weight information Into making a choice as to which peer we want to send this eviction page to. Chances are that the node, that has a higher weight is a likely candidate that'll pick. But it will not always be the node with the highest weight. Because if that decision is made by everybody, then we are going to have a situation where if the prediction was not exactly accurate, we would be making wrong choices.</li> 
  <li>Basically you can see that this age management, geriatric management, is approximating a global LRU. Not exactly a global LRU, because global LRU computing that on every page fault is too expensive, so we don't want to do that. Instead, we are doing an approximation to global LRU by computing this information at the beginning of an epoch, and using that information locally in order to make decisions. So we think globally in order to get all the age information, and compute the minimum age and compute the weight for all the nodes as to how much of the fraction of the replacements are going to come from each one of these nodes. But once that computation has been done, for the duration of the epoch, all the decisions that are taken at a particular node is local decisions in terms of what we want to do, with respect to a page that we are choosing as an eviction candidate. Do we discard it, or do we store it in a peer, global cache? This might sound like a political slogan but, the important thing, in any distributed system, is as much as possible to use local information in decision making. So think globally but act locally. So that's the key and that's what is ingrained in this description of the algorithm.
</li> 
</ul>


<h2>12. Implementation in Unix</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/13.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>So from algorithm description, we go to implementation. Now this is where rubber meets the road in systems research. In any good systems research, the prescription is as follows: You identify a pain point. Once you identify the pain point. You think of what may be a clever solution to that. Then a lot of heavy lifting actually happens in taking that solution, which may be a very simple solution, but implementing that is the hard part. if you take this particular lesson that we're looking at. The solution idea is fairly simple. The idea is that instead of using the disk as a paging device, use the cluster memory. But implementing that idea requires quite a bit of heavy lifting, and one might say that in systems research these technical details of taking an idea and working out the technical details of implementing that idea is probably the most important nugget. Even if the idea itself is not enduring, the implementation tricks and techniques that are invented in order to do their implementation of their idea maybe reusable knowledge for other systems research. So that's a key takeaway in any systems research and this true for this one as well.</li> 
  <li>In this particular case, the authors of GSM used a DEC (Digital Equipment Corporation) operating system as the base system. The operating system is called OSF/1 operating system. And there are two key components in the OSF/1 memory system, which is what we're talking about here.</li> 
  <li>First component: The first one is the virtual memory system, and this is the one that is responsible for mapping process virtual address space to physical memory, and worrying about page faults that happen when a process is trying to access the stack and heap and so on. So that it can bring those missing pages perhaps from the disk. And these pages are sometimes referred to as anonymous pages because a page is housed in a physical page frame and when a page is replaced, that same physical page frame may host some other virtual page and so on. So the virtual memory system is devoted to managing the page faults that occur for process virtual address space, in particular the stack and the heap.</li> 
  <li>Second component:The unified buffer cache is the cache that is used by the file system. And remember the file system is also getting stuff from the disk. But the other system wants to cache it in physical memories so that it is faster, so the unified buffer cash is serving as the extraction for the disk resident files when it gets into physical memory and user processes are going to do explicit access to files. When they do that, they're actually accessing unified buffer cache. So reads and writes of files go to this unified buffer cache. In addition to that, Unix systems offer the ability to map a file into memory, which is called memory mapped files. And if you have a memory mapped file, you can also have page faults to a file that has been mapped into memory. So the unified buffer cache is responsible for handling page faults to map files, as well as explicit read and write calls that an application process may make to the file system.</li> 
  <li>Normally this is the picture of how the memory system hangs together in any typical implementation of an operating system. You have the virtual memory manager, you have the unified buffer cache, you have the disk, and region writes from the virtual memory manager go to the disk, and similarly reads and writes from the unified buffer cache, go to the disk and when a physical page frame is freed, you through it into the free list. So that it is available for future use by either the virtual memory manager or the unified buffer cache. And the Pageout Daemon, its role is to look at every once in a while what are pages that can be swapped out to the disk so that you make room for page faults to be handled without necessarily running an algorithm to free up pages.</li> 
  <li>So that's the structure of a memory management system, and what the authors of GMS did was to integrate GMS into the operating system, and this is where I said that there is heavy lifting to be done, because you are modifying the operating system to integrate an idea that you had for global memory system.
</li> 
</ul>


<h2>13. Implementation in Unix (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/14.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>This calls for modifying both the virtual memory part as well as the UBC (unified buffer cache) part of the original system to handle the needs of the global memory system. And particular what you notice is that the VM and the unified buffer cache, when it wanted a missing page, it used to go to the disk. But here what we're going to do is we're going to make it go to the GMS Because GMS knows whether a particular page, that is missing in the address space of a process, is present in some remote GMS and similarly if a page is missing from the file cache, it knows whether that page is perhaps in the remote GMS. So that's why we modify the virtual memory manager and unified buffer cache manager to get their calls for getting and putting pages.</li> 
  <li>Getting page is when there is a page fault I need to get that page. I would go to the disk normally, but now I go to GMS. That GMS worry about getting it for me. Similarly if a page is missing in the unified buffer cache, I go to the GMS to say get me that page for me, and he will then do the work of finding out whether it is in remote GMS or if it is not anywhere in the cluster as we've seen in some of the cases. It could be under disk, it will get it from the disk.</li> 
  <li>Notice writes to disk is unchanged. Originally, whenever the VM manager decides to write to the disk, it's because a page is dirty. It writes to the disk. That part we don't want to mess with, because we are not affecting the reliability of the system in any way. Writes remain unchanged. Only when you have a page fault, you have to get it from the disk, you don't go to the disk anymore, but you come to GMS, and GMS then is integrated into the system so that it figures out where to get the missing page from.</li> 
  <li>Remember that the most important thing that we have to do in implementing this global LRU at approximation to global LRU is collecting age information. That's pretty tricky. And the case of file cache is pretty straightforward, because it is explicit from the process where it is making regenerate calls, we can insert code as part of integrating GMS into this unified buffer cache, to see what pages are being accessed based on the region rights that are happening to the unified buffer cache. Because these calls are explicit from the application, going into the operating system and therefore, we can intercept those calls as part of integrating DMS to collect age information for the pages that are housed in the unified buffer cache.</li> 
  <li>VM on the other hand is very complicated. Because memory access, that a process does, is happening in hardware on the CPU. The operating system does not see the individual memory access that a user program is making, so how does GMS collect aid information for the anonymous, that's why these pages are called anonymous pages. In this case, the pages are not anonymous because we know exactly what pages are being reached in by a particular read or write by a file system, but the reads and writes that a user process is making to its virtual address space are the pages that are anonymous so far as the operating system is concerned. So, how does the operating system collect the age information? In order to do that, the authors of GMS what they did, was to have a daemon part of the memory manager in the OSF/1 implementation. The daemon's job is to dump information from the TLB. If you recall, TLB, the translation lookaside buffer, contains the recent translations that have been done on a particular node. So the TLB contains information about the recently accessed pages on a particular processor. Periodically, say every minute, it dumps the contents of the TLB into a data structure that the GMS maintains so that it can use that information in deriving the age information for all the anonymous pages that are being handled by the VM. So that is how GMS collects the age information at every node. So this is where I meant, the technical details are very important. Because this idea of how to gather age information is something that might be useful if you're implementing a completely different facility.</li> 
  <li>Similarly, when it comes to the pageout daemon. The pageout daemon, normally, what it would do is, when it is throwing out pages. If they are dirty pages, he'll write it on to the disk. And if it is clean pages, he can simply discard it. But now, when it has a clean page, it wants to discard a clean page, a pageout daemon. We'll give it to GMS, because GMS will say, well, don't throw it out. I'll put it into cluster memory so that it is available later on for retrieval rather than going to the disk.</li> 
  <li>So this is how GMS is integrated into this whole picture, interacting with the Unified Buffer Cache, interacting with the VM Manager, interacting with the Pageout Daemon and the Free List Maintenance as well in terms of allocating and freeing frames on every node.
</li> 
</ul>


<h2>14. Data Structures</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/15.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Some more heavy lifting. Let's talk about the distributed data structures that GMS has in order to do this virtual memory management across the entire cluster. First thing that GMS has to do, is to convert a virtual address, which is a local thing so far as a single processor's concerned. And convert that virtual address into a global identifier, or what we'll call as a universal ID (uID). And the way we derive the universal ID from the virtual address is fairly straightforward, if you think about it. We know which node this virtual address emanated from, IP address. We know which disk partition contains a copy of the page that corresponds to the virtual address. That we know. What are the i-node data structure that corresponds to this page? And what is the offset? So if you put all of these entities together, you get a universal ID that uniquely identifies. A virtual address. This is the offset within a page. So given a virtual address, the first three parts uniquely identify the page, and the fourth part identifies the offset within that page for that virtual address. And this we can derive it from the virtual memory system as well as the UBC.</li> 
  <li>There are three key data structures: PFD, GCD and POD. These three data structures are the workhorses that make this cluster wide memory management possible. Let's talk about each one of these things.</li> 
  <li>PFD is like a page table. Normally in a page table what you do is you give it a virtual address and the page table says oh, I know what is the physical frame that backs this particular virtual address. That is the translation between a virtual page number and a physical page frame that hosts that virtual page is contained in a page table. Similar to that is PFD, it's called the page frame directory. It has a mapping between a UID, because your virtual address has been converted to a UID. Given a UID, it says, what is a page frame that backs that particular UID? That's this data structure. Because we're doing cluster-wide memory management, the page itself can be in one of four different states. It could be in the local part of that node. And if it is in a local part of a node. Then that page could be a private page or it could be a shared page. These are two possibilities, there is living in the local part of the physical memory of a node. If it is in the global part, we know by definition, global part is only hosting clean pages, so the content of global cache is always going to be private pages and so the state of the page that happens to be the global cache of a particular node is guaranteed to be a private state. And the last possibility is that it's not in the physical memory of a node, but it is on the disk. So the page frame directory just like a page table says that either this page that you are looking for go from VA to UID, that page is in physical memory and it is one of these three states or its not in the physical memory, it's on the disk.
</li> 
</ul>

<h2>15. Data Structures (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/15.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>>GCD: Now given a UID I know that some PFD in the entire cluster has the mapping for the UID saying what is the physical frame number that corresponds to it if it happens to be on that node or it's on the disk. That information is contained in some PFD in the entire cluster. So, if I have a page fault. I convert my VA to UID then which PFD will I go look up? I could broadcast to everybody and say, how do you have it? That will be very efficient. You don't want to do that, or we can say that there is some way of magically mapping, given a UID which node will have the PFD for me to look up. And find out the backing physical page. But we don't want to do a static binding of UID to the node that manages that UID. Because if we make a static mapping, then it pushes the burden on one node to take that, if some page has become really hot and everybody wants to use that. What we want to do is distribute this management function, just like the age management, we did not want to concentrate it on a single node. We want to distribute the management of giving this mapping between UID and which node has the PFD that can tell me information about the missing page. And that data structure is this Global Cache Directory, GCD. GCD is a hash table, it's a cluster-wide hash table, so it's a distributed data structure. And the rule that GCD performs is given a UID, it will tell you which node has the PFD that corresponds to this UID. That's the role of this data structure, it's a partition hash table, so given a UID, I can say well. I go to the GCD, and the GCD will say, what is the PFD that has this UID? Because of the partition hash table, even though a part of this GCD is on every node, every node may not be locally able to determine where the PFD is. Given a UID, it can go, it has to know which GCD it has to consult. In order to know which node has the PFD that corresponds to this UID.</li> 
  <li>POD: So, we need another data structure that tells us, given a UID, which node has the GCD that corresponds to this UID. And that is the page ownership directory. So, the page ownership directory says, given a UID. Which node has the DCD that corresponds to this UID. And this data structure the page ownership directory is replicated on all the nodes. It's an identical replica that is on all the nodes. So, when I have a page for it first thing that I'm going to do, is go to my POD and that is. A replicated data structure and it's completed information, up to date information. So I go to this POD and asked this question. Given this new id, how do I find out the global cache directory that has information about the PFD that can help me to map my virtual address to a physical address. Remember that we could have simply gone from here(PFD) to here(POD), but that would have been a static mapping, and this one level of indirection is giving a way by which we don't have to statically map. A PFD to a UID, but this intermediate step, allows us to move the responsibility of hosting a particular PFD to different nodes, using this intermediary which is a distributive hash table. I said that this page ownership directory is replicated data structure. Can it change? Well it can change over time because what this page ownership directory is saying is the following. The UID space is something that spans the entire cluster. If you take the virtual addresses of all the possibilities of the entire cluster. That universe of all the virtual addresses is this UID space because it is being mapped from a a virtual address of a single process to this UID space and this spans the whole cluster and what we have done is we have partitioned that UID space. Into set of regions of ownership and that's what is called the page ownership. So every node is responsible for a portion of the UID space and that is this global cache directory. Now if the LAN never revolves. In other words, if the set of nodes on the LAN is fixed. Then the page ownership director also remains the same. But if nodes, if new nodes are added and deleted and so on, that's when the page ownership directory is going to change. And if that happens, then we have to replicate again, we have to redistribute the page ownership directly. But this is not something that's going to happen too often. It's very rare that node is going to come down or new node is going to be inserted into a LAN. And therefore this page ownership directory does not change very often. And that's where it's replicated data structure that you can believe at every node. But if it changes, there is also a way of handling that. We'll see that in a minute.</li> 
  <li>The path for page fall handling is if you have a page fall you convert VA to UID and once you have this UID then you can go to your page ownership directory that's on your node. And ask the question, please tell me who has the PFD that corresponds to this UID? And GCD is going to tell me "oh here is the note that contains the PFD for the UID that you're looking for". Then I can go to that PFD, and from that PFD, I can get the page that I am looking for which might be in that note or it might say "well it's not in my note any more. It's on the disk". So this is the pack for page four handling.
</li> 
</ul>

<h2>16. Putting the Data Structures to Work</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/16.JPG?raw=true" alt="drawing" width="500"/>
</p>

<!-- <ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->


<h2>17. Putting the Data Structures to Work (cont)</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/17.JPG?raw=true" alt="drawing" width="500"/>
</p>


<!-- <ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>
 -->

<h2>18. Putting the Data Structures to work (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l7/18.JPG?raw=true" alt="drawing" width="500"/>
</p>

<!-- <ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>
 -->

<h2>19. Global Memory Systems Conclusion</h2>

<!-- <p align="center">
   <img src="" alt="drawing" width="500"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul> -->

# L07b: Distributed Shared Memory

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
