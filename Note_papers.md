# Outline
- [Extensibility, Safety and Performance in the SPIN Operating System](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/Note_papers.md#extensibility-safety-and-performance-in-the-spin-operating-system)
- 
- 

# Note
<h2>Extensibility, Safety and Performance in the SPIN Operating System</h2>
<p>Bershad, B. N., Savage, S., Pardyak, P., Sirer, E. G., Fiuczynski, M. E., Becker, D., Chambers, C., & Eggers, S. (1995). Extensibility safety and performance in the SPIN operating system. ACM SIGOPS Operating Systems Review, 29(5), 267–283. </p>


<ul>
   <li> <strong>SPIN is an operating system</strong> that can be dynamically specialized to safely meet the performance and functionality re-quirements of applications. </li>  
   
   <li> <strong>Goals and approach </strong>
      <ul>
         <li> Goals - extensibility, safety and good performance
            <ul>
               <li>Extensibility: determined by the interfaces to services and resources that are exported to applications</li>
               <li>Safety: the exposure of applications to the actions of others, and requires that access be controlled at the same granularity at which extensions are defined</li>
               <li>Good performance: low overhead communication between an extension and the system</li>
             </ul></li>
         <li>the SPIN operating system relies on four tech-niques implemented at the level of the language or its runtime
            <ul>
               <li>Co-location: Operating system extensions are dynamically linked into the kernel virtual address space. Co-location enables communication between system and extension code to have low cost</li>
               <li>Enforced modularity: Extension is written in Modula-3,a modular programming language for which the compiler enforces interface boundaries between modules. Extensions, which execute in the kernel’s virtual address space, cannot access memory or execute privileged instructions unless they have been given explicit access through an interface. Modularity enforced by the compiler enables modules to be isolated from one another with low cost</li>
               <li> Logical protection domains: Extensions exist within logical protection domains, which are kernel namespaces that contain code and exported interfaces. Interfaces, which are language-level units, represent views on system resources that are protected by the operating system. An in-kernel dynamic linker resolves code in separate logical protection domains at runtime, en-abling cross-domain communication to occur with the overhead of a procedure call</li>
               <li> Dynamic call binding: Extensions execute in response to system events. An event can describe any potential action in the system, such as a virtual memory page fault or the scheduling of a thread. Events are declared within interfaces, and can be dispatched with the overhead of a procedure call. </li>
            </ul>
         </li>         
      </ul>
   </li>
   
   <li>SPIN is primarily written in Modula-3, which allows extensions to directly use system interfaces without requiring runtime conversion when communicating with other system code. Although SPIN relies on language features to ensure safety withinthe kernel, applications can be written in any language and execute within their own virtual address space. Only code that requires low-latency access to system services is written in the system’s safe extension language. For example, we have used SPIN to implement a UNIX operating system server. The bulk of the server is written in C, and executes within its own address space (as do applications). The server consists of a large body of code that implements the DEC OSF/1 system call interface, and a small number of SPINextensions that provide the thread, virtual memory, and device interfaces required by the server. </li>
   <li> Motiviation: An extensible system is one that can be changed dynamically to meet the needs of an application.</li>
   
   <li> <strong>The SPIN Architecture </strong>
      <ul>
      <li> SPIN makes few demands of the hardware, and instead relies on language-level services, such as static typechecking and dynamic linking</li>
      <li>Modula-3's key features of the language include support for inter-faces, type safety, automatic storage management, objects, generic interfaces, threads, and exceptions. The design of SPIN depends only on the language’s safety and encapsulation mechanisms,specifically interfaces. </li>
      <li>Capabilities: All kernel resources in SPIN are referenced by capabilities. A capability is an unforgeable reference to a resource which can be a system object, an interface, or a collection of interfaces. SPIN implements capabilities directly using pointers, which are supported by the language.A pointer is a reference to a block of memory whose type is declared within an interface. Sample code is provided in section 3.1 The protection model Capabilities</li>
      <li>In SPIN the naming and protection interface is at the level of the language, not of the virtual memory system. A domain, named by a capability, is used to control dynamic linking, and corresponds to one or more safe object files with one or more exported interfaces. An object file is safe if it is unknown to the kernel but has been signed by the Modula-3 compiler, or if the kernel can otherwise as-sert the object file to be safe. Protection domains: Create, Resolve, Combine. Sample code is provided in section Protection domains </li>
      <li>Extensions in SPIN are defined in terms of events and handlers.An event is a message that announces a change in the state of the system or a request for service. An event handler is a procedure that receives the message. An extension installs a handler on an event by explicitly registering the handler with the event through a central dispatcher that routes events to handlers. Event names are protected by the domain machinery. An event is defined as a procedure exported from an interface and its handlers are defined as procedures having the same type. A handler is invoked with the arguments specified by the event raise.The right to call a procedure is equivalent to the right to raise the event named by the procedure.</li>
      </ul>
   </li>
   <li> <strong>The core services</strong>
      <ul>
      <li>Extensible memory management:The SPIN memory management interface decomposes memory services into three basic components: physical storage, naming, and translation.These correspond to the basic memory resources exported by processors, namely physical addresses, virtual addresses, and translations. Application-specific services interact with these three services to define higher level virtual memory abstractions, such as address spaces. </li>
      <li>Extensible thread management:An operating system’s thread management system provides applications with interfaces for scheduling, concurrency, and synchronization. Applications, though, can require levels of functionality and performance that a thread management system is unable to deliver.In SPIN an application can provide its own thread package and scheduler that executes within the kernel.The thread package defines the application’s execution model and synchronization constructs. Although SPIN does not define a thread model for applications, it does define the structure on which an implementation of a thread model rests. This structure is defined by a set of events that are raised or handled by schedulers and thread packages. A scheduler multiplexes the underlying processing resources among competing contexts, called strands. A strand is similar to a thread in traditional operating systems in that it reflects some processor context. Unlike a thread though, a strand has no minimal or requisite kernel state other than a name. An application-specific thread package defines an implementation of the strand interface for its own threads.Together, the thread package and the scheduler implement the control flow mechanisms for user-space contexts.Application-specific thread packages only manipulate the flow of control for application threads executing outside of the kernel. For safety reasons, the responsibility for scheduling and synchronization within the kernel belongs to the kernel. As a thread transfers from user mode to kernel mode, it is checkpointed and a Modula-3 thread executes in the kernel on its behalf. As the Modula-3 thread leaves the kernel, the blocked application-specific thread is resumed. A global scheduler implements the primary processor allocation policy between strands.Additional application-specific schedulers can be placed on top of the global scheduler.That is, an application-specific scheduler presents itself to the global scheduler as a thread package. </li>
      <li>Implications for trusted services: The core services are trusted, which means that they must perform according to their interface specification. In designing the interfaces for SPIN’s trusted services, we have worked to ensure that an extension’s failure to use an interface correctly is isolated to the extension itself</li>
      <li>Code check paper Figure 3 & Figure 4 for memory VPN,PPN & translation and threads strand</li>
      </ul>
   </li>
   
  <li> <strong>System performance </strong>
      <ul>
      <li>Evaluate the performance of SPIN from System size (codebase), Microbenchmarks (Measurements of low-level system services, such as protected communication, thread management and virtual memory to reveal the overhead of basic system functions), Networking, End-to-end performance (application) </li>
      <li> Compared monolithic (DEC OSF/1 V2.1), MicroKernel (Mach) and SPIN </li>
      <li> Protected communication: 1) protected in-kernel call inSPINis implemented as a procedure call between two domains that have been dynamically linked. SPIN>rest(NA) 2) System call overhead reflects the time to cross the user-kernel boundary, execute a procedure and return. MACH > DEC > SPIN  3)Cross-address space call shows the time to perform a protected, cross-address space procedure call. DEC OSF/1 supports cross-address space procedure call using sockets and SUN RPC. Mach provides an optimized path for cross-address space communication using messages. SPIN’s cross-address space procedure call is implemented as an extension that uses system calls to transfer control in and out of the kernel and cross-domain procedure calls within the kernel to transfer control between address spaces. DEC > MACH > SPIN </li>
      <li>Thread management: 1) Measurement on "the time to create, schedule, and terminate a new thread, synchronizing the termination with another thread." 2) Measurement on "synchronization overhead, and measures the time for a pair of threads to synchronize with one another" 3) Conclusion - SPIN’s extensible thread implementation does not incur a perfor-mance penalty when compared to non-extensible ones, even when integrated with kernel services.</li>
      <li>Virtual memory:1)measure the time for an application to query the status of a particular virtual page,the latency between a page fault and the time when a handler executes,the time to increase the pro-tection of a single page,the time to increase and decrease the protection over a range of 100 pages. 2)SPIN outperforms with application-specific system calls and fast in-kernel protected procedure call. DEC OSF/1 and Mach, though, communicate these events by means of more expensive traps or messages</li>
      <li>Networking: 1)Latency and Bandwidth:For DEC OSF/1, the application code executes at user level, and each packet sent involves a trap and several copy operations as the data moves across the user/kernel boundary. For SPIN, the application code executes as an extension in the kernel, where it has low-latency access to both the device and data. Each incoming packet causes a series of events to be generated for each layer in the UDP/IP protocol stack 2)Protocol forwarding:In SPIN an application installs a node into the protocol stack which redirects all data and control packets destined for a particular port number to a secondary host. The DEC OSF/1 forwarder is not able to forward protocol control packets because it executes above the transport layer. As a result it cannot maintain a protocol’s end-to-end semantics  </li>
      <li>End-to-end performance: SPIN allows a server to both control its cache and avoid the problem of double buffering. A SPIN web server implements its own hybrid caching policy based on file type. DEC OSF/1 relies on the operating system’s caching file system (no double buffering)</li>
     </ul>
   </li> 
    
   <li>Issues: 1)Scalability and the dispatcher 2)Impact of automatic storage management 3)Size of extensions</li>
   
</ul>

