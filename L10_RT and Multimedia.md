# Lesson outline
- [L10a: TS-Linux](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L10_RT%20and%20Multimedia.md#l10a-ts-linux)
- [L10b: PTS](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L10_RT%20and%20Multimedia.md#l10b-pts)

# L10a: TS-Linux
<h2>1. TS-Linux Introduction</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/1.JPG?raw=true" alt="drawing" width="500"/>
</p>

<ul>
  <li>Traditionally general purpose operating systems have catered to the needs of throughput oriented applications, such as databases and scientific applications. But now more and more applications, such as synchronous AVplayers, video games and so on, need soft real-time guarantees. That is they are latency sensitive. How do we mix the two kinds of applications in a general purpose operating system? Also, what are the pain points in developing complex, multimedia applications that need such real-time guarantees? How can they be alleviated?</li> 
  <li>In this module, we will discuss approaches to addressing these questions. In this lesson, we will discuss time-sensitive Linux, an extension of the commodity general purpose Linux operating system that addresses two questions. The first question is how to provide guarantees for real-time applications in the presence of background throughput oriented applications, such as databases and so on. The second question is how to bound the performance loss of throughput oriented applications in the presence of latency sensitive applications.
</li> 
</ul>

<h2>2. Sources of Latency</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/2.JPG?raw=true" alt="drawing" width="600"/>
</p>


<ul>
  <li>Time sensitive applications require quickly responding to an event. Think of playing a video game and shooting at a target. You want the instant that you shoot at the target for action to appear on the screen. Now, problem is there are three sources of latency for time sensitive events, in any typical general purpose operating system. The first source of latency is what is called the timer latency. That is coming because of the inaccuracy of the timing mechanism. For instance, the timer event went off at this point, but the timer interrupt actually happens only here. That is, this is the event that should have resulted in an interrupt, but because of the inaccuracy of the timer, there's a latency between the point at which an even happened, an external event. And the point at which the timer goes off indicating that external event. And this is primarily coming up due to the granularity of the timing mechanism that's available in general purpose operating systems. For instance, periodic timers tend to have a ten millisecond granularity in Linux operating system. So that's the first source of inaccuracy or latency for time sensitive applications. The second source of latency is what we call as the as the preemption latency. And this is because of the fact that the interupt could have happened when the kernel was in the middle of doing something from which it can not be preempted. For instance, when the kernel is modifying some critical data structure. In that case it may have turned off the interrupt and therefore even though the interrupt went off at this point the kernel cannot be preempted until this point. That may be the second source of latency or it could be that the kernel itself is in the middle of handling another higher priority interrupt. And in that case, this time interrupt is going to be delayed. So that the second source of latency for time sensitive applications running on commodity operating systems. Okay, finally the timer interrupt has been delivered to the system and now the scheduler can actually schedule the process that is waiting for this timer interrupt so that that application can take the appropriate action for this external interrupt. But wait, there is another high priority task that's already in the scheduler's queue and therefore the app that was waiting for this timer event cannot yet be scheduled because of this higher priority tasks that has dibs on the processor at this point of time. So this is the third source of latency, that is the scheduler latency. That is preventing this external event from being delivered to the application that is waiting for this particular event. Now finally the high priority app is done, and therefore now it is time for the app that is actually waiting for this event. So this is the point of activation of the Event app, that is, the app that is waiting for this timer event. So you can see that this is the distance TA to Th. The difference between TA and Th is the event to activation distance. The event happened here, but the app that can react to this event only gets scheduled at this point, and this is the latency that is coming in for time-sensitive applications between the actual event happening. And the activation of the app that has to deal with that particular event. For time sensitive applications to do well, it is extremely important that we shrink this distance between event happening, and event activation.
</li> 
  <li></li> 
  <li></li> 

</ul>


<h2>3. Timers Available</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/3.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li>Let's review the kinds of timers that are avaialbe in an operating system, and the Pro's and Con's. The timer that, many of you may be familiar with is a periodic timer that is standard in, in most Unix operating systems. And the periodic timer, the pro is, periodicity. That's the pro of a periodic timer that. The operating system is not going to be interrupted willy-nilly, but it is going to be interrupted at regular periods. But the con is the event recognition latency, because the event may have happened at a particular point in time, but because of the granularity of the periodic timer. The event is actually recognized at a much later point in real time, and that's the con for periodic timer. And the maximum time of latency is equal to the period itself, so for instance, if the event happened just after a periodic interrupt, then the event will have to be delivered by the next periodic interrupt. So the worst case latency is going to be the periodicity of the periodic timer itself. The other extreme to periodic timer, is what we call one-shot timers. And what one-shot timers are, they are exact timers. That is, you can program these timers to go off at exactly the point at which you want the entropy delivered. To the processor, so the pro is a timeliness of the one-shot timer. And secondly, there is also fielding these one shot timers as and which they occur, that's an extra overhead that the operating system has to deal with. If you're concerned with the interrupt overhead, one extreme position to take is to get rid of timer interrupts all together, and simply use soft timers. That is, there is no timer interrupt, but the operating system is going to poll at strategic times. To see if there is an external event. What would be strategic times? Typically any application is going to make system calls. And when a system call gets you into the kernel, at that point the kernel can see if there are any events that need attention at that point of time. And that is a polling method, but the downside of the polling method Is the fact that, there is latency associated with that and there is also, the fact that, you have to pull all the events to see if any of them have expired, and there is an overhead associated with that. These are, the two cons, for a soft timer but the pro is, the fact that you have reduced overhead for soft timers since there are no. Timer interrupts, per se, but the kernal is using strategic points during it's execution such as system calls or other external interrupts. For instance a network packet arrival or something like that. As a trigger for looking at the time related structures to see if any of the events may have expired at that point of time. Firm timer is a new mechanism that is proposed in TS Linux and as we will see shortly it combines the advantages of all the three. Timers that I just mentioned, the periodic timer, the one-shot timer, and the soft timer. Essentially, what firm timer is trying to do is, take the pros of all of these types of timers in its implementation so that it can avoid the cons associated with each one of these individual choices.
</li> 
  <li></li> 
  <li></li> 

</ul>


<h2>4. Firm Timer Design</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/4.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li>The fundamental idea behind the firm timer design is to provide accurate timing with very low overhead. And what it does is it combines the good points of the one shot timer and the soft timers. As I mentioned, the one shot timer, its virtue is the fact that we can have the timer interrupt happen exactly at the time that we want. In other words, we get rid of the timer inaccuracy that we mentioned earlier. That plagues normal timer design in general purpose operating systems. The con that we said with the oneshot timer, is that there is overhead associated with processing oneshot timer events. As well as reprogramming them. So for that reason, what they do in TS Linux with the firm timer design is to have a knob, which is called the overshoot knob, and the idea is the one shot timer expired here. This is the point at which the event happened, but what we're going to do is. We're going to have a parameter associated with the one shot timer called the overshoot parameter and what that overshoot parameter is allowing you to do is, even though this is the point at which the event happened, the one-shot timer is programmed to go off at this point. So there is a distance between the actual event happening and the point at which the one-shot timer has been programmed to interrupt the CPU. This is the overshoot parameter. You may wonder, what is the advantage of this overshoot parameter? Well, within this overshoot parameter window, there could be a system call, which is a soft interrupt. We mentioned that applications typically make system calls, or there could be an external interrupt. Any of those things can bring you into the kernel, and so you may be already in the kernel at this point during this overshoot window. So what we could do now is, we'll dispatch the expired timers at this point of time and also we will reprogram the one-shot timer that expired for the next time that we need the one-shot timer to interrupt. Normally, the one shot timer would have gone off here because this is the overshoot window, but even before that, since the system call happened, at this point, we'll going to see that oh, there is this one shot timer that has already expired, its going to interrupt me not in the too distant future, might as well dispatch that expired timer right now and reprogram that one shot timer so that it is now ready for the next one-shot timer interrupt. The upshot of doing this, dispatching the expired timer at the point of the system call that is happening within this overshoot window, is that at the point at which this one-shot timer has been programmed to interrupt, that is gone. It'll not cause an interrupt because we've reprogrammed this one-shot timer to interrupt at the next one-shot timer expiry event. Because whatever action that needed to be taken with respect to this particular one-shot timer expiration has already been taken at this point and therefore at this point we will not get another interrupt. So we have saved on the one-shot timer interrupt that would have happened at this point of time by combining the hard and soft timer in this manner. I mentioned that fielding a one-shot timer interrupt is expensive. We avioded that. Because, fielding the interrupt happened as part of handling the system call and at this point itself, we have re-programmed the one-shot timer for the next event as well. So you can see that the firm timer design combines the good points of both the one-shot timer as well as the soft timer. It is giving us the accuracy that we want from the one-shot timer but at the same time, we are avoiding the overhead associated with one-shot timer by processing that interrupt using this over shoot parameter. Of course, with, within this over shoot parameter, there is no system call or external interrupt that brings us into the kernel, then we will have this one-shot timer going off at this point. But the hope is that by choosing this knob appropriately, between hard and soft timers, we can make sure that we reduce the number of times we get the one shot timer interrupts actually occurring.
</li> 
  <li></li> 
  <li></li> 

</ul>


<h2>5. Firm Timer Implementation</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/5.JPG?raw=true" alt="drawing" width="600"/>
</p>


<ul>
  <li>As you know, in Linux they use the term task to signify a schedulable entity. And so, we're using T1, T2, T3, to mean tasks, which are schedulable entities. And the timer-q data structure, what it contains is the tasks and the expiry time for that particular task. And the tasks are ordered in this Timer-q data structure maintained by the kernel, sorted by the expiry time. So task T1 is the earliest task to go off. Because it has the earliest expiry time. T2 is next, with an expiry time of 12. T3 with an expiry time of 15 and so on. So this is the way the kernel is maintaining the data structure to know when a particular task's expiry time is up for processing the event associated with that particular task. The basis for firm timer implementation is the availability of APIC hardware. APIC is advanced programmable interrupt controller, which is implemented on chip in modern CPU starting from Intel Pentium onwards, and the firm timer implementation in TS-Linux takes advantage of the APIC hardware. The good news is with the APIC timer hardware, re-programming a one shot timer takes only a few cycles. So there is not a significant overhead to re-programming a one shot timer on modern CPUs because of the availability of the APIC hardware. So if the task that is at the head of the queue, that had it's timer event go off, if it has to be reprogrammed, all that we need to do is execute a few instructions in modern processes to reprogram that one shot timer to go off at the next one shot interval. When the APIC timer expires, the interrupt handler will go through this timer-q data structures and look for tasks whose timers have expired. Some of these tasks may be periodic tasks, some of these tasks may have been programmed to deal with the APIC timer event. So, associated with each entry in this timer queue data structure is the callback handler for dealing with that particular event. And those callback handlers are going to be called by the interrupt handler upon the expiry of the APIC timer. The expired timers are going to be removed from those timer queue data structures. If the entry corresponds to a periodic timer, then the handler will re enqueue that particular task in the timer queue data structure after updating its expiry field for the next periodic event for that task. If it is a one shot timer that this task was using. In that case, the interrupt handler will reprogramme that task for the next one short event. The way the APIC timer hardware works is by setting a value into register which is decremented at each memory bus cycle until it reaches zero. At which point it'll generate an interrupt. Now given a 100 megahertz memory bus, for instance, on modern CPUs, a one short timer has a theoretical accuracy of ten nanoseconds. However, in practice, the time needed to feel the timer interrupt is significantly higher. And that becomes the limiting factor in the granularity that you can get with one shot timers implemented using the APIC hardware. But the important point is that APIC hardware allows you to implement very fine grained timers in modern processes. And as I already mentioned, by choosing an appropriate overshoot parameter in reprogramming the epic timer. We can eliminate the need for fielding one shot interrupts, because of soft timers going off within that overshoot period. Another optimization that we can do, in the firm time at implementation, is looking at the distance between one shot events. For instance, in this picture I'm showing you, the long distance between two one shot events. There is a one shot event happening here. There is a one shot timer event happening here. Another one shot timer event happening here. And if you have such a long distance, it is possible that there may be several periodic timer events that may be going off within this long distance. So this suggests that if periodic events are going to go off, and if it is close enough to a one shot timer that would have gone off, why not take advantage of that? So what we want to do is dispatch a one shot event at a preceding periodic event. The key thing for time sensitivity is not missing the timer event. If you're going to process it a little bit sooner, that's okay. So that's exactly what is happening here. Just as in the case of the overshoot parameter being used in combing one shot to the soft timer. What we're doing here is, because periodic timers are going to interrupt anyhow and if the kernel notices that there is a one shot event that is coming up fairly soon, then it can simply dispatch that one shot event at the preceding periodic timer event that is any how accepting the processor. And once you do that, then you can reprogram this one shot event to go off at the next expiry point for that one shot event. So basically, what we're doing when we have a long distance between one shot events, is to use a preceding periodic timer event, so that we can both avoid the overhead of dealing with this one shot event. And also the cost of reprogramming it. Or in other words, we completely eliminate using one shot events for situations where the distance between the one shot events is so big that we can simply use the periodic event instead of the one shot event. And the reasons for doing that are twofold. One is, periodic event data structures are much more efficient than the kernel, they're order of one data structures. Whereas, the one shot event programming data structures in the kernel. Tends to be order of log n, where n is the number of active timers. So as an optimization, if the one shot events happen at fairly long distances. That there are several periodic events that are going to happen anyhow within that. We will simply use the periodic timers instead of the one shot timer. So to summarize, the firm timer implementation. The first point is that the APIC hardware allows reprogramming one shot timers in few cycles. And secondly, by choosing the appropriate overshoot distance, we can eliminate the need for fielding the one shot timer interrupts if soft timers go off within that overshoot period. And third, if the distance between one shot timers is really long, then instead of using one shot timers, we will simply use periodic timers and dispatch the one shot event at the preceding periodic timer event. Those are the ideas that are enshrined in the firm timer implementation in TS-Linux, that essentially combines the advantages of one shot timer with soft timers with periodic timers. So this should give you a feel for how TS-Linux, by a clever implementation, reduces the timer latency, the first component of the latency from the point of event happening to event activation.
</li> 
  <li></li> 
  <li></li> 

</ul>


<h2>6. Reducing Kernel Preemption Latency</h2>

<!-- <p>The second source of latency, as I mentioned, is the kernel preemption latency. Now how do we reduce the kernel preemption latency? Timer goes off when the kernel is in the middle of something and so we have to wait for the kernel to be ready to take that interrupt. The first approach is the explicitly insert preemption points in the kernel. So that it can actually look for events that may have gone off and take action. So that is the first approach. The second approach is to allow preemption of the kernel anytime the kernel is not manipulating shared data structures. Now the reason why you don't want the kernel to be preempted is because it is manipulating some shared data structures and if you preempt that that can result in some race conditions in the kernel. Bad news. So what we want to do is, we want to make sure that the kernel is not preempted while it is manipulating shared data structures. But if it is not, then we want to allow preemption. So these are two different approaches that can be combined in order to reduce the latency associated with kernel preemption. A technique, due to Robert Love, called the lock-breaking preemptible kernel combines these two ideas. And by combining these two ideas it reduces the spin lock holding time in the kernel. The idea is the following. So there will be a long critical section in the kernel where in it is manipulating some shared data structures, but it also doing other things within this critical section. So what we're going to do is apply these two principles and break this long critical section as follows. So we're going to change this critical section code into two sections: you acquire the lock, you manipulate the shared data structures, and you release the lock. And the reason you're releasing the lock is because you're done with manipulating the shared data structures, and then we will reacquire the lock, and finally release it. So essentially we replaced this long critical section by two shorter critical sections, one manipulating shared data and the rest of the code here. What is the advantage of doing this? What we can do is, at this point, once we are done with manipulating shared data, we release the lock and at this point we can preempt the kernel safetly. And since we can preempt the kernel at this point it's a great opportunity to check for expired timers at this point. If there are expired timers, dispatch them, reprogram one-shot timers if need be. All of that stuff can be done at this point, and then you can come back, reacquire the lock, and continue with the critical section. So that's the idea of the lock breaking preemptible kernel that combines these two ideas of explicit insertion of preemption points and allowing preemption any time the kernel is not manipulating shared data structures. The third source of time latency I mentioned is the scheduling latency, that is, the timer event goes off, and we want to schedule the app that is going to deal with the timer event as quickly as possible, but the scheduler is in the way. How do we quickly make sure that that app gets scheduled? Firm timer implementation in TS Linux uses a combination of two principles. One is called Proportional Period Scheduling and in Proportional Period Scheduling what is being done is that, when a task, so these things represent tasks T1 and T2, when a task starts up it is going to say that it needs a certain proportion of the CPU time to be allocated to it in every time quantum. So there are two parameters associated with proportional period scheduling, Q and T. T is a time window, a time quantum, and this is the time quantum that is exposed to an application. And the application can say, within the time quantum T, I need a certain proportion of the time for my task. So T1 might say that, in any time T, I need two thirds of the time to be devoted to my task and if another task, T2, says in any time period T, I need one-third of the CPU to be devoted to my task, then these two requests can be obviously satisfied by TS Linux because the two add to the periodicity of scheduling T. And this is the idea behind proportional period scheduling, in which what the scheduler does is admission control at the time that a process starts up. If it asks for a certain proportion of time it sees whether it is possible to satisfy that request. So, for example, if already the scheduler has promised T1 two-thirds of the time, and T2 comes along and says I also need two-thirds of the time in every period TS Linux is going to say uh-huh, no, cannot do that, because it doesn't have the capacity to accommodate both these requests simultaneously. So that's the idea behind Proportional Period Scheduling. So it provides temporal protection by allocating each task a fixed proportion of the CPU during each task period T. And both this Q and T parameters are adjustable parameters using a feedback control mechanism. And so this, in essence, improves the accuracy of scheduling analysis that you can do on behalf of processes that are time-sensitive. The second technique that is used in TS Linux for reducing scheduling latency is to use priority scheduling. And let me motivate that by introducing a problem that plagues real time tasks and that is what is called priority inversion. So here is a high priority task C1 and it needs some service. And it gets that service by calling a server. And this call is a blocking call to the server. And the server itself may be a low priority server. For example, this client may contact a window manager to say, I need a portion of the window and this is what I want you to do in terms of painting that portion of the window. That might be a call that a high priority task is making to a low priority server and this is a blocking call and till the server is done with its work the high priority task cannot continue execution. So if you look at the timeline, C1 is running and it makes a blocking call at this point and the server takes over. And this is the service time for the server to execute the blocking call made by C1. So at the end of this service time, C1 is ready to run again. But not so fast. It could be that during the service time of this low priority server, on behalf of C1, some of the higher priority tasks C2, which perhaps was waiting for some I/O completion, becomes runnable again. If it becomes runnable again, then this higher priority task, compared to this server, is going to preempt the server and take over the CPU. So what happens at this point is that this medium priority task, because it is higher than the servers priority, it's going to take over, preempting the server, and that is essentially a priority inversion as far as C1 is concerned because C1 is higher in priority than C2. But unfortunately, at this point of time, the server is the one that is scheduled and that is lower priority than either of these two guys. And therefore, C2 happily preempts the server and takes over the CPU. But from the point of view of C1, that's a priority inversion. And it is a time-sensitive task that affects the sensitivity of the time-sensitive tasks. This is where the priority-based scheduling of TSL saves the day for us. Basically, the idea is that when C1 makes a request to the server, the server is going to assume the priority of C1. Even though normally, it has a static priority which is lower than the priority of C1. Because C1 is making this call, the server's priority is going to be boosted to be the priority of C1 itself. So for the duration of the service time of the server the priority of the server task is going to be the same as the priority of C1, which is higher. Now, C2, when it becomes runnable, it cannot preempt the server because the server is now running at the priority of C1. And therefore, we avoid the priority inversion that would have normally happened because of the priority-based scheduling in TS linux, so the upside is that there will be no priority inversion with this priority-based scheduling mechanism that is there in TS linux. So to recap, TS linux avoids scheduling latency to shrink the distance between the event happening and event activation by doing this admission control through the proportion period scheduler and also it avoids priority inversion by using this priority-based scheduling. So these two mechanisms allow shrinking that distance as well. The other advantage of the Proportion Period Scheduling is that TS Linux can have a control over how much of the CPU time is devoted to time-sensitive tasks so that it can reserve a portion of the time for throughput-oriented tasks. So for instance, it could say, within any period T, I'm going to reserve a third of the time for throughput-oriented tasks, so that even if there are time-sensitive tasks running throughput-oriented tasks are going to get their dibs for running on the CPU. And that way, we can make sure that while supporting the timeliness of time sensitive tasks, TS-Linux can also ensure that throughput oriented tasks are able to make forward progress. So the three ideas that are enshrined in TS Linux, for dealing with time-sensitive tasks are, first of all, coming up with the firm timer design that increases the accuracy of the timer without exorbitant overhead in dealing with one shock timer interrupts. Second, using a preemptible kernel to reduce the kernel preemption latency and third, using priority-based scheduling to avoid priority inversion and guaranteeing a portion of the CPU time to be allowed for time-sensitive tasks. Those are the ways by which the distance between event happening and event activation can be deduced and we can get good performance for time-sensitive applications even though the operating system is a commodity operating system like Linux.
</p>
 -->
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/6.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li></li>  
</ul>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/7.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li></li>  
</ul>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/8.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li></li>  
</ul>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/9.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li></li>  
</ul>

<h2>7. TS-Linux Conclusion</h2>
<ul>
  <li>The upshot of attacking and fixing the three sources of latency of concern for real time task is that TS-Linux is able to provide quality of service guarantees for real time applications running on commodity operating systems such as Linux. At the same time, by admission control, using the proportional period scheduling, TS-Linux ensures that throughput-oriented tasks are not shut out from getting CPU time. The proof of the pudding is in the eating. I encourage you to read the details of performance evaluation carried out in this paper that I've assigned to you for reading, wherein the authors show that both the above objectives are achieved by their design.
</li> 
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

# L10b: PTS
<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li>In the previous lesson, we studied an operating system scheduler adaptation for providing accurate timing for upper layers of software, especially needed for real time multi-media applications. Now we move up in the food chain. The focus in this lesson is in the middleware that sits in between commodity operating systems and novel multimedia applications that are realtime and distributed.
</li> 
</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
  <li></li> 
  <li></li> 

</ul>

<h2>1. PTS Introduction</h2>
<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l10/10.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li></li> 
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
