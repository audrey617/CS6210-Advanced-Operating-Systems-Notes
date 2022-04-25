# Lesson outline
- [L11a: Principles of Information Security](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L11_Security.md#l11a-principles-of-information-security)
- [L11b: Security in Andrew](https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/L11_Security.md#l11b-security-in-andrew)

# L11a: Principles of Information Security
<p>Saltzer, J. H., & Schroeder, M. D. (1975). The protection of information in computer systems. Proceedings of the IEEE, 63(9), 1278-1308.</p>

<h2>1. Principles of Information Security Introduction</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/1.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Computer system security has assumed enormous importance in the connected world that we live in today. Indeed, there are courses offered just on the topic of computer system security. And universities, including Georgia Tech, offer an entire Master's degree program on information security. In this course module our goal is to get an introduction to the terminologies in computer system security that we should be familiar with as operating system designers. Using a case study we'll also learn Cryptographic techniques that can be used as a tool by an operating system designer to provide authentication in a distributed manner. We'll start this module with a discussion of the terminologies first articulated by Jerome Saltzer. You'll be amazed at how computer visionaries thought of issues with information security, even before computers were connected to one another.
</li> 
</ul>


<h2>2. Firsts from Computing Pioneers</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/2.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>It's always very illuminating to go back in time and look at the thought pieces from computer visionaries, and particular I want to draw your attention to this memorandum that was done back in 1963 that talks about intergalactic computer networks. I encourage you to do a web search and get this document and read it. Here is another one this document talks about the first time computers talk to one another. The first computer to computer communication back in October 29th, 1969. Now this is considered the first ever email that was written by Ray Tomlinson from BBN Technologies back in 1971, and with all the junk mail that is floating around, you might be wondering why email was invented in the first place, but it is a useful tool when it is not junk. 
</li> 
</ul>


<p align="center">
    <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/3.JPG?raw=true" alt="drawing" width="600"/>
 </p>
 <ul>
   <li>This is a log book that was maintained by professor Leonard Kleinrock of UCLA who is considered one of the networking pioneers. And this log book records the first time there was a computer to computer communication. A call was made from UCLA to Stanford Research Institute to host communication recorded on October 29th, 1969. And this is the first ever computer to computer communication. </li>  
 </ul>


 <p align="center">
    <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/4.JPG?raw=true" alt="drawing" width="600"/>
 </p>
 <ul>
   <li>Here is another interesting chart. This chart shows all the computers in the world in one chart. March of 1977. This is your entire internet on one chart back in 1977. </li> 
 </ul>

 <p align="center">
    <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/5.JPG?raw=true" alt="drawing" width="600"/>
 </p>
 <ul>
   <li>And the model of a computer system back in 1975 was you had a mainframe computer, which consisted of a CPU, memory, and I/O devices like a disk, and you have cathode ray terminals through which users can connect to the CPU and use the CPU in a time-shared manner. So this was the model of the computer. Back in 1975. Note that I'm not showing any connection to any network here. The reason I'm giving you this buildup with all the first in computer technology and the model of the computer system, is because the seminal paper by Jerome Swalser identifies terminologies, you and I know, as current day issues. Examples include denial of service, firewalls, sandboxing, objects, and so on.</li> 
 </ul>


<h2>3. Terminologies</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/6.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Let's talk about the terminologies identified by Saltzer in his seminal paper from 1975, that talks about the issues in computer security.</li> 
  <li>The first issue that is identified is when to release information. In this day and age, we all know the importance of privacy of information. And there is a distinction between privacy of information and security. Privacy has to to with when, as individuals, we expect to release information or prevent information for being released. That's the individual's right and responsibility in terms of information that they own. Security, on the other hand, is dealing with how to make sure that the system is respecting the guarantees that the user needs both in terms of privacy of information as well as when they want to release information. Said differently, privacy is more concerning the individual and the information that they own, they possess, how their information is protected and released when and if an individual wants to release information. Security on the other hand, is a system function. The system is guaranteeing certain properties about information that it is preserving on behalf of the user community. And in particular, the system has to protect information that belongs to individuals. And ensure that those information is released only when the individual that owns that information wants it to be released. So in this sense, there's an obligation for the system to ensure that users of the system are authenticated. Meaning that if I go to the system and want to access some information. The system has to ensure that I am who I am. That's authentication, and then it also has to make sure that whatever information I have access to is something that is not violating any privacy of other individuals that may have put information into the system. So, protection and authentication go hand in hand and building a secure system.</li> 
  <li>This also identifies a comprehensive set of security concerns.
    <ul>
        <li>The first security concern is unauthorized information release. If I have put some information into the system, then it is a responsibility of the system to make sure that the information that I've put in is not released without my authorization as the owner of the information. So that is the first concern. </li>
        <li>The second concern is unauthorized modification of information. I may have released it for a certain set of users to access information that I've put into the system. But I may not have given authority for them to modify the information, so, unauthorized modification of information is the second set of concerns. </li>
        <li>The third security concern is unauthorized denial of use. What that means is. If I've given authority to a set of users that they can access my information, there should be nothing that prevents that set of authorized users from being able to access that information. So that is what unauthorized denial of use is concerned about.And you may be much more familiar with the term Denial of Service. That's exactly what this particular concern is all about. In fact, this is the first mention in the literature of Denial of Service attacks on a server, that prevents authorized users from being able to get service from the system. </li>
    </ul>  
</li> 
  <li>So what should be the goal of a secure system? Well the goal of a secure system should be to make sure that all such violations do not occur. That is preventing all violations is the concern of a secure system. But unfortunately stated this way, it's a negative statement, preventing all violations, that's insuring that the system is somehow secure proof, meaning that nobody can attack the system. And because it's a negative statement it's hard to achieve. It's sort of like saying there, are no bugs in my program. There is no way to prove that a program has no bugs. And in a similar manner there, is no way to assure that the system prevents all violations. So what Saltzer argues is, the goal of the secure system should not be stated negatively but positively. If the goal of the secured system is stated in this negative manner that it has to prevent all violations. At best, it can give a false sense of security because there is no way to assure that bad guys are not going to break into the system. So, making a statement like my system prevents all violation, is giving a false sense of security because it is not achievable in practice.</li> 
</ul>


<h2>4. Levels of Protection</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/7.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Also goes on to identify four levels of protection.This is unprotected systems. So an unprotected system is where there is no level of protection. An example would be the early operating system for the personal computer called MS-DOS. And it had hooks in the system for preventing mistakes by the user. But there was no real protection. So mistake prevention is not the same as securing a system. That's important to see the distinction between mistake prevention and a secure system.</li> 
  <li>Now, let's talk about the four levels of protection that Saltzer identified in this classic paper.</li> 
  <li>The first level is what he calls all or nothing. An example of a system that implemented that would be IBM's VM-370. And most of the time-sharing systems of the 60s and the 70s, they had this all-or-nothing property. For example, in the VM-370, Each user was given the illusion as though they have their own personal system. That's why it's called a virtual machine and the only way they can interact with one another is by explicitly doing I/O from one virtual machine to another virtual machine. That's the All-or-Nothing Property that VM-370 had.</li> 
  <li>The second level of protection is what results are called controlled sharing, that is for example having access lists associated with files, so that an individual, if I create a file, I could say my file can be shared by My students, and here are the names of the students that have access to their file. So that's an access list that you can associate with information that you create and give to the system for safekeeping. That's controlled sharing.</li> 
  <li>Another level of protection is what's also called user programmed sharing controls. Now this would be facilities similar to what you find in Unix file system today, for instance, such as being able to associate different access right for files for different groups of users in unix, for instance, there are levels of protection that you can associate with a particular file that you created. What is the access right for the creator, the owner of the file? What is the access right for a group that is defined and what is the access right for the rest of the world? So there are three levels of, protection that you can associate of the file, and that's an example of user program sharing controls.</li> 
  <li>Another level of protection is having user defined strings on information. For instance, in the military it is often common to have physical files labeled with top secret. That has to be opened by only some privileged set of users. Similarly, you can associate such strings on information that you create and store with the system. There's four levels of protection that I've identified here are not cast in concrete, because as the system evolves, as the user community evolves, You need to be able to deal with the dynamics of use of this information, use of the system. So that is another issue which you can call as a cross-cutting issue with respect to these levels of protection. For example, how do I change the permissions I've given to a particular file to a set of users that I've defined as a group that can access that file. How do I change the set of users I've included in the access list? What do I do to remove somebody from the access list? What do I do to add someone to the access list for information that I've already created? So all of these are issues that deal with the dynamics of use of information.
</li> 
</ul>

<h2>5. Design Principles</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/8.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Saltzer identifies eight design principles that goes hand in hand with the levels of protection that we just talked about.</li> 
  <li>Principle 1: In designing a secure system, the first principle to adhere to is economy of mechanisms, and by that, what is meant is that the mechanism should be easy enough, so that it can be verified. The mechanism should be easy enough so that it can be verified whether it works or not, that's important, Economy of Mechanisms. </li> 
  <li>Principle 2: The second principle is what he calls fail safe defaults, the idea is you want to explicitly allow access to the system or information. The default should not be no access. The reason for saying this is because if the default is no access then there is no way to guarantee that information is protected. This is a negative statement again. So instead of a negative statement like that, you want to explicitly allow access to information. The second design principle is in designing the system you make sure that the default is fail safe. </li> 
  <li>Principle 3: The third design principle is what he calls complete mediation. And by that what is meant is that the security mechanism should not take any shortcuts. For example in authentication, you want to make sure that when somebody presents credentials for authenticating themselves with the system, that has to be checked completely. And what that means is that there should be no shortcuts to authentication. An example of a shortcut will be a password file that lives on storage if it is cached for performance reasons, that can be a source of secuirty violation. Because the actual password file on the storage system may have changed, but the cached copy in memory may not be reflecting it, and that would result in violation of security. If the authentication uses the cached copy instead of the persistent copy. So that's an example of why you need complete mediation in designing a secure system, so that's another principle that a secure system should adhere to.</li> 
  <li>Principle 4: The next design principle is what is called open design, meaning that you want to make the design completely open, so in other words you publish the design exactly spec it out and publish it, but protect the keys that are used by the design. What that means is that cracking the design, even though it is published, in order to access the design or get any useful service out of the system, you have to present keys. And those authentication keys should be so hard to break. Computationally breaking the keys should become infeasible. So that's what is meant by open design. And what does open design principle also foster is the underlying tenet that detection is easier than prevention. We don't know what all attacks are possible from the bad guys. And so it is not possible to prevent all of them. And therefore, you want to publish the design but make breaking the protection computationally infeasible and detect that a violation has happened than try to prevent it, detect that a violation has happened, so that's the key idea behind this open design principle. </li> 
  <li>Principle 5: The next principle talks about separation of privilege. You may have seen quite often in banks, two keys may be necessary to open the vault of the bank. And the two keys will be held by two different individuals, so that both of them have to come together in order to open the vault. So this is the idea behind separation of privilege. </li> 
  <li>Principle 6: The next principle talks about least privilege. That is, we want to use the absolute minimum capability that we need, in order to carry out a certain task. So the controls in the system should be based on need to know. An example would be to do certain things on your computer. For instance, if you want to install certain new pieces of software, you may be able to do that as a normal user on your laptop, but for certain things you may need administrative privileges. So clearly identifying what functions you need administrative privilege or superuser privilege versus what functions you need normal privilege and ensuring that the right level of privilege is provided or afforded by the system to the user, is an important design principle. And in fact, this is the origin for the idea of Firewalls, that are common these days in organizational setups. What firewalls do is to ensure that individuals within an organization are able access external information from inside the corporate network only on a need basis, or information that is inside the corporate network is allowed to get out only under authorized conditions. So that's the need to know based controls, and that has become very common these days with the prevalence of firewalls in any administrative setup. </li> 
  <li>Principle 7: Another principle is least common mechanism. Or in other words, if there is a mechanism that the system wants to implement to assure information security. At what level should that mechansim be implemented? Should it be implemented in the kernel? Or should it be available as a library that is outside the kernel? Because there are implications when something is inside the kernel, it has access to information that is also inside the kernel. Wheres if it is a library sitting on top of the kernel, then you can limit the amount of damage a malfunctioning mechanism can do to the system as a whole. So that's the idea behind these common mechanisms.</li> 
  <li>Principle 8:  The last design principle that Salza identifies is psychological acceptability. And by that, what is meant is the mechanisms being easy to use for the end user, so that they completely understand what they are doing. When they are using a particular mechanism that is provided by the system, so good user interface is something that is extremely important as one of the design principles in building a secure system. </li> 
  <li>Two things jump out when you look at this set of design principles. That is lead out by Saltzer in his seminal paper back in the early 70s. 
    <ul>
        <li>The first thing that jumps out is all of these are positive statements. They are not negative statements. I mentioned earlier saying that my system prevents all violations is a negative statement because it says that my system is bullet proof. No system is bullet proof, and therefore all the things that are being laid out as design principles are positive statements and they give a way by which you can say that these are the things that it can do. And you build enough into your design, such that you can detect violations when they occur, rather than trying to prevent them. Prevention is much harder than detection. 
        </li>
        <li>The other thing that jumps out is the fact that all of these principles, laid out in the early 70s, are applicable to today's systems and these principles were crafted when computers were not even connected to the extent that they are today. </li>
     </ul>
    So the two key takeaways from these design principles are, first of all you want to build the system in such a way that cracking the protection boundary is computationally infeasible. That's the first takeaway. The second takeaway is you build the system to detect violations rather than prevent violations. Because prevention is much harder. Detection on the other hand is something that is doable.
</li>  
</ul>

<h2>6. Principles of Information Security Conclusion</h2>
<ul>
  <li>This paper by Saltzer is a classic. The issues identified in section one of the paper are classic information security issues relevant to this day. To think that these issues were thought of at a time when computer to computer communication was not routine and all the nodes connected to one another in the entire globe fits on one slide gives you how visionary our computer pioneers were.
</li> 
</ul>

# L11b: Security in Andrew

# L11b: Security in Andrew

<p>M. Satyanarayanan, " Integrating Security in Large Scale Distributed Systems ", ACM TOCS, Aug. 1989.</p>
<h2>1. Security in Andrew Introduction</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/9.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>The Andrew File System was a bold new experiment in the CS department at CMU. The intent was to enable students across campus to be able to walk up to any workstation on campus and start using it. All the files stored in a central server on the local area network will be accessible in a safe and secure manner from that workstation. The assumption is that the network itself is untrusted. In this lesson we will use, the Andrews system as a case study to see how private key cryptographic infrastructure can be used for the security and authentication of such a system.</li> 
  <li>The focus in this lesson is a distributed file system being made available to a user community. And the year is circa 1988, and the state of the computing at that time looked as follows. Local area network and client workstations connecting the servers and the local disk on the workstation serve as efficient caches of files that maybe downloaded from the server.
  </li> 
</ul>




<h2>2. State of Computing Circa 1988</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/10.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>The stage set for the Andrew File System, AFS, is the CMU campus. And the user community can access file servers using workstations connected to a local area network. And as I mentioned, local disks on the workstation served as efficient caches. And the vision of the designers of the Andrew File System and the Coda file system, both of which were built at CMU for enabling the student community to access a central file system is as follows.</li> 
  <li>A user walks up to any workstation and logs into the work station. And your content magically appears on this workstation from the central server as soon as you log in. In other words, in the CMU experiment, what they wanted to afford the users to be able to do is to have this ability to walk up to any workstation spread out throughout the campus and be able to access their personal information that is stored in servers that are central to the entire campus. Isn't that what today's cloud computing and mobile devices are trying to do to your content? In some sense you'll see that many of the technologies that we take for granted today had their modest beginnings in experiments such as the Andrew File System and the Coda file system at CMU.
  </li>   
</ul>

<h2>3. Andrew Architecture</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/11.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>The architecture of the Andrew System looks like this. There are client workstations which are called virtues. And these client workstations are connected by insecure network links to a local area network. And through the local area network, they can access servers S1, S2, S3 and so on. And these servers are in a secure environment or in other words, accessing the servers from a client work station uses insecure links but once inside the boundary of the servers, the servers can talk to one another, exchange information, grab data from the disks and so on.</li> 
  <li>This communication that happens inside this server environment is a secure communication. That's part of the reason they've named the server environment as vice, and the client environment where you're accessing information from the secure servers as the virtue workstation.</li> 
  <li>And the important thing to note is the fact that the clients have to access information from the server over insecure links. What that means is that in order to make sure that information that is sent on the wire to the servers or received from the servers to the clients. They have to be encrypted since anyone can sniff the packets that are flowing on these wires. But inside the vice, the communication that happens among the servers, because it is secure, there is no need for information encryption.</li> 
  <li>The client workstations run some flavor of Unix operating system. So inside each client workstation, there is a special process called Venus. And this process which runs on the virtue workstation is for the purpose of authentication of the user and for client caching of files that the user may fetch from device servers into the workstation that they're logging in to. The user will use RPC in order to get the files that they want to work on on the workstation. And this is where Venus comes in. Venus is the one that authenticates a user walking up to a virtue workstation to the server and then acting as the surrogate for the user in fetching the files that he or she wants to work on from the file server using RPC. And since I mentioned that the link that is used to communicate between virtue and vice is insecure. The RPC has to be a secure RPC. In other words, the RPC messages, both passing the parameters and receiving the results, is going to use encryption on the data that is being sent and received between the virtue and the vice file server.
</li> 
</ul>

<h2>4. Encryption Primer</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/12.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Let me give you a very quick refresher on encryption. There are two families of encryption systems.</li> 
  <li>One is the private key cryptosystem. And in the private key cryptosystem both the sender and the receiver use symmetric keys for encryption of the data and decryption of the data. A common private key system that we all are using probably on an every day basis is passwords. When we log into either a corporate network or a university network. We use a username and password and that is using a private key encryption systems. The idea is simple. The sender takes the data that they want to send a receiver and encrypts the data using a key and this key is a private key that is known only to the sender and the receiver space and nobody else. So by encrypting the data with this key they produce what is called a cyphertext and the cyphertext can go on insecure links. Anybody can see the bit pattern of the cyphertext, but in order to make sense out of the cyphertext they have to decrypt it. And to decrypt it they need the key and the key is only available between the sender and the receiver. So this is where Saltzer's principle comes into play. That is, publish the design, but protect the key. The keys are protected between the sender and the receiver. You're publishing the method by which you are transfering information between sender and receiver. Namely, using a private key encryption system. But, for any bad guy to access information that is flowing on the wire they have to have the key in order to make sense out of the data that is flowing. And you make breaking the key hard enough computationally that the system is secure. Now one of the problems with the private key system is the fact that the keys have to be distributed to the sender and the receiver and key distribution problem is one of the difficulties with private key cryptosystems. Especially as the size of the organization becomes larger and larger. </li> 
  <li>The public key cryptosystem overcomes this key distribution problem. And in this case there is a public key which is published. The name itself suggests the idea behind the public key cryptosystem. And that is, anyone can create an encrypted data by taking this public key which is available, let's say published in the Yellow Pages. They can take the public key, take the data that they want to send and encrypt it, and send it on the wire. But in order to decrypt the data, the public key is no good. You need a private key that is the only way to decrypt this data. In other words, the mathematical underpinning of the public key cryptosystem is that there are asymmetric keys, a pair of keys, for information exchange. In order to encrypt you use a key that is a public key. Anybody can have access to that. But in order to decrypt that, you need a private key. And conversion of data into a cyphertext using the public key is a one-way function, which is not reversible. Similarly, converting the encrypted message into the original data using the private key is another one-way function. So one-way functions are the mathematical basis for the public key cryptosystem. So now, the way you would send data, if you're the sender, take the data, encrypt it using the public key. You create a cyphertext. Once you have the cyphertext, you cannot convert it back to the data using the public key. The only entity that can convert the cyphertext back to its original data is the entity that has the private key. The other part of this asymmetric key infrastructure, namely, the private key and using the private key, you can decrypt the cyphertext and create the original data. So this is the workflow for a sender to encrypt the data, clear the cypher text, send it to the receiver, receiver decrypts it using the private key, and produces the original data.
</li> 

</ul>

<h2>5. Private Key Encryption System in Action</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/13.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>So here is a private key crypto system in action. Two entities, A and B, have exchanged keys. A will use the key KA to send a message to B, and B can decrypt the message using the same key KA. And similarly, when B wants to send a message to A it encrypts it lets say using another private key KB and when the message comes over here A will decrypt the message using the same key that was used for encryption namely KB. So one of the things that should be obvious is that, in order for this private key encryption system to work, both the entities need to know when they get an encrypted message who is the author of the message, because that is the only way they know what key to use in order to decrypt this message. So if A sends a message to B, it's sending this encrypted message. When this cyphertext arrives here, for B to know that it has to use this key KA, it needs to know the identity of the sender. So the identity of the sender has to be sent in cleartext. So this is the format of a message that is going from A to B. The identity of the sender in cleartext Meaning that, looking at this, we will immediately know "this message is from sender A, and therefore, I should use key kA to decrypt the cyphertext" and vice versa when A gets a message from B. Of course, KA and KB can be the same, or in other words, it is the same key that is used for, A to send a message to B as well as for B to send a message to A. But the important thing to take away is that the key used for encryption and decryption of a given message is exactly the same, that's the idea behind the private key encryption system.
  </li> 
</ul>

<h2>6. Challenges for Andrew System</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/14.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>There a few key challenges in implementing this Andrew system, which is intended for campus environment. User community consisting of students, who can login to work stations anywhere on campus and those workstation are connected by insecure network links to central server. And a simple server is a depository for all the files of the entire user community. So the challenges include:
    <ul>
      <li>A first challenge is authenticating a user. That is, when a user says "I'm Kishore", the system has to verify unambiguously that the user indeed is Kishore.</li> 
      <li>A second challenge is authenticating the server. That is, if I, as a user, log into a work station and I get a message from the server, I have to be assured that this message is actually coming from the server. And not some Trojan horse that is pretending to be the server.</li> 
      <li>The third challenge is preventing replay attacks. And what that means, is that even though we using encryption to secure the data that is going on the wire, someone that is sniffing the wire could grab a packet and then resend that same packet, and that should not fool either the sender or the receiver. That's what is meant by replay attack.</li> 
      <li>The fourth challenge is of course ensuring that the user community is shielded from one another. Either due to unintended interference of user or another, or malicious interference. Both of those situations have to be avoided. So that is what I meant by isolating users.</li> 
    </ul>
  </li> 

  <li>So these are the design challenges for the Andrew system. So in the Andrew file system, they decided in order to make sure that all of these challenges are met, they have to use a secure RPC as the basis for client server communication. And, in implementing the secure RPC, they also decided to use private key cryptosystem, and I mentioned that the public key crypto system does not have the key distribution problem, but on the other hand, for a closed community like a campus environment, the key distribution problem is not as big a challenge, so therefore, the design of the Andrew file system decided to use private key crypto system. And as I mentioned earlier in the private key crypto key system in order to identify what key to use to decrypt a message that is coming in, the identity of the sender has to be known. So this has to be sent in clear text.</li> 
  <li>If you take a traditional operating system like Unix you have username and password which is the way you authenticate yourself for the system. But, in a campus environment if you're going to use secure RPC as a mechanism for communication between the client and the server. There can be lots of communication that is happening between individual clients and the servers. And if you're going to use, use a name and password all the time for such communication. Such overuse will result in a security hold. One of the things that I mentioned as a principle proposed by Jerome Saltzer, is you publish the design but protect the keys, but you publish the design but protect the keys. And what is meant by protecting the keys is making it competitionally hard to break the key but over exposing the key allows someone to take a long time to crack the key, and therefore overusing username and password as the basis for all communications, in a circular RPC, will pose a security hole. So the dilemma that the Andrew Fine system designers had to face, was what to use as the identity that needs to be sent in cleartext, and what to use as the private key so that we don't always use any pair of identity and private keys for a long time.
</li> 
</ul>

<h2>7. Andrew Solution</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/15.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>The solution that they took in Andrew is as follows. Only for logging into a workstation the user name and password are used. And of course, they have to be securely communicated to the server over insecure links, we'll see how that is done. But the key point is that username and password is only useful logging in. Once a user is logged into a workstation, at that point onwards for the rest of the log-in session, the intent is to use ephemeral IDs and ephemeral keys but all the subsequent Venus-vize communication. Recall that Venus is the process that resides on the virtual workstation acting as a surrogate for the user for file caching. And vize is the server that is living inside a secure environment. And this venus-vize communication happens over insecure links, that's where we going to use ephemeral ids and keys.</li> 
  <li>So this gives rise to three classes of client-server interactions.
    <ul>
      <li>The first interaction is logging in. Imagine you're a student, and you're doing a course project. What is that you're going to do. You're going to walk up to a workstation and log in presenting your username and password to the workstation. And this username and password is going to be used as the basis for the client server communication that lets you authenticate yourself to the server.</li>
      <li>Now once you authenticate yourself to the server, what are you going to do? well, you want to do a course project, and for the course project, you probably need to download some files to the server. And that requires that Venus running on your workstation establish an RPC session with the server in order to fetch the files that you need to work on for your course project. That's the second class of client-server communication, establishing an RPC session.</li>
      <li>Now once you establish an RPC session, then you can request the files that you want as a user. As a student you want certain files to be accessed during this RPC session. You bring those files and once you brought in the file that you want, you can close the RPC session, work on your project locally on the workstation. And once you're done with completing whatever work you needed to do, you may establish a new RPC session to upload the results of your work back into the file server.</li>
    </ul>
</li> 
  <li> So the three classes of client-server interactions are logging in, which happens exactly once for the entire login session, and RPC session establishment, which may happen several times during the time that you're logged into work station. Every time you decide "oh, I want to fetch a new file. Oh, I want to store this file back into the server." The third set of interaction is the actual file system access during the session. Once this RPC session has been established by venus-vize, then you as the user, you may want to work on a particular file You may open a file. At that point, Venus will go, using this RPC session, and fetch the required file and cache it locally for you to use it, and later on, if you close the file, at that point, Venus will commit the changes that you made to the file to the central file server again. So that's the kind of file system operation that's the third class of client server interaction. And for both RPC session establishment, and for the actual access to the file system during an RPC session, we want to use ephemeral IDs and keys.</li> 
  <li></li>   
</ul>

<h2>8. Login Process</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/16.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Let's first understand how the login process works? You walk up to a workstation and you login. And how do you login? Well you login using your username and password and this login process is special that runs on the virtue workstation. It communicates with the login server that is inside the vice by presenting the username and password in a secure fashion. We'll see how that is done in a bit. So it presents the username and password securely over the insecure links to the login server, and the login server, once it authenticates who you are using the username and password, it is going to send you a pair of tokens, and remember all of this is happening on behalf of the user by this in the virtual workstation. The user has to do nothing special. All that the user is doing is logging in using a user name and password. Under the covers these things are happening.</li> 
  <li>First, the login process communicates securely with the login server presenting the user name and password. And the login server then returns two tokens. One is called a secret token. The other is called a clear token. The clear token is a data structure. Once again I have to mention that both the secret and clear tokens are communicated back to the login process by the login server in a secure manner over the insecure link. We'll see how that is done in a minute. So once, the login process gets these two tokens, decrypts them and gets these two tokens, the login process decrypts the message that contains these two tokens, and extracts the clear token and the secret token.</li> 
  <li>The clear token is really a data structure which is known to the login process. And the data structure in particular contains a handshake key. We'll call it HKC. So from this clear token data structure, the login process can extract this handshake key, and the secret token is just a bit string so far as the login process is concerned. The weight is generated by the login server is to encrypt this clear token with a key known only to vice. It's not the same as HKC. It is a key that is known only to vice to encrypt the clear token. And we will see how this key is going to be used, later on.</li> 
  <li>So to recap, virtue sends securely the username and password to the login server. Login server securely sends secret token and clear token back to this login process. From the clear token The login process extracts the handshake key. And the secret token is basically a bitstream which is an encryption of the contents of this cleared token, encrypted with a key that is known only to Vice. In other words, the secret token is unique for this login session, and It is a bit string, which means nothing to anybody that sees it on the wire. And therefore we can use this bit string that secret token represents as ephemeral client-id for this login session. Recall I said, we don't want to expose the user name and password too often on the wire. This is Andrew's answer to dealing with a problem by providing an ephemeral client-id for this login session. Once I use it as logged in, they get an ephemeral client-id, which is the secret token. And this can be used in the future communication between virtual And vice as the client-id.</li> 
  <li>Now, how will vice know who is communicating with it when it sees this bit pattern? Remember that this bit pattern secret token is an an encryption of the clear token. And the key for decrypting it is know only to vice. So when the secret token comes as the client-id, vice can decrypt it and find out from that, what is the clear token associated with that particular bit stream which is representing the secret token. And once it knows that, it can also extract the handshake key that it gave to particular client, and that's how the identity of the client that is communicating with the vice in the future, presenting this secret token as the ephemeral client-id can be recognized by vice.</li> 
  <li>So now, after the login is done, for all the future communication between Venus and Vice, HKC can be used as a private key for establishing a new RPC session. This pair of tokens, the secret token and clear token, is stored on the Virtue workstation by Venus. On behalf of this user for the entire login session. At the end of this login session, these two tokens will be thrown away by Venus. But during the login session, these two tokens are representing this particular that has logged in. And, so for the duration of this login session. Venus will use the secret token, which is a bit string that represents this particular user for this login session, as the client-id to send information over to the vice. And any information that Venus sends to vice to establish an RPC session is going to be encoded with the private key that it had been handed now through the clear token data structure, that is the handshake key that had been given as part of this login exchange through the clear token data structure. At the core of the entire secure RPC system of. The Andrew file system is the bind mechanismfor setting up a client server connection securely, and that's what we're going to look at next.
</li>  
</ul>

<h2>9. RPC Session Establishment</h2>
<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/17.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now that the user had logged in to a client work station, Venus will establish an RPC session on behalf of the client. In order to establish the RPC session, the following message exchange is going to happen. This is what is called the bind operation between the client and the server.</li> 
 <li>Venus sends the client identity and an encrypted cipher. The client identity, as I mentioned earlier, in private keycryptal systems, you have to send it in clear text so that the server knows the identity of the client. In order to establish a new RPC session, Venus will use the secret token as the client identity. And it will use the HKC, the handshake key for the client that was contained in the clear token data structure that was given back to the workstation by the server as part of executing the login process. That is the key that will be used for encrypting the starting message for the RPC session establishment.</li>  
</ul>


<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/18.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now what exactly is the massage that is being sent? Well, all there is sent for initiating this RPC session establishment, is a random number which is new for each RPC session that Venus is establishing on behalf of this client. Every time it wants to establish a new RPC session within a login session, it creates a new random number and encrypts it using the handshake key. That's the cipher text. And the secret token is the client ID, and that's the message that goes from virtue to vice, the server. So when this message arrives at the server, how will the server decrypt this message? Well, the client ID is the secret token. Recall that the secret token is nothing but an encryption of the clear token with a key that is known only to the server, and therefore, what the server can do is take the secret token, decrypt it using the key that it has and once it decrypts it, it gets this clear token data structure and from the clear token data structure, it can say "well, what is the key that is contained in this clear token data structure, take the key and decrypt this message." Cause that key is HKC, and that is how the server can get the message that has been sent as the initiation of the RPC session. So in particular, what the server has gotten now is the random number that was sent to it by Venus Xr.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/19.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now what the server does, is it takes this random number, increments it by 1, and also adds a new random number Yr, and that is a message that it is going to send back to the client. It has to encrypt this message, because its going on the wire which is insecure. So it's going to encrypt it using a key, a handshake key which we'll call HKS and by design, HKC and HKS are exactly the same. So in other words, whatever is the handshake key that has been given to the client in the clear token, is the same handshake key that the server is going to use to communicate information back to the client as well. So, it encrypts this message with this handshake key and sends it over to the client. So what is the purpose of this message? When this message comes over to the client, the client can decrypt this message. How? It's going to use HKC which is the handshake key that it knows will be used by a genuine server to encode the message and send it to the client, and so it can use HKC to decrypt this message and once it decrypts this message, it gets these two numbers. There's Xr plus 1, that is the original random number that this guy sent over to the server incremented by 1.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/20.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now what is the purpose served by this number Xr plus 1? It actually establishes to the client that the server is genuine. Anybody could replay a message that they see on the wire, but for this message to have the right content, what the client is expecting is that the response that the client is going to get for the original message that was initiating the RPC session is that the response from the server is going to contain a number that is 1 greater than the random number that it sent the server in the first place. So, if this number, Xr plus 1, is what the client was expecting, then it establishes that the server is genuine. So this goes back to what I said about authentication of the server. That is one of the challenges that the Andrew system has. This is the way the virtue workstation, the Venus process on the virtue workstation on behalf of the client can establish the genuineness of the server that this workstation is talking to. Or in other words, it's not a Trojan Horse that is sending this message. But it is genuine server that is sending this message because this field is exactly what Venus expected it to be.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/21.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now what is this number Yr? This is another random number that the server is sending as part of this message. Why is it sending this random number? It'll become clear in the next set of communication that's going on between Venus and Vice.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/22.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now once Venus has extracted this Yr from this message, what it is going to do is it is going to increment it by 1 and take that as a message, Yr plus 1, as a message, encrypt it using the handshake key, and send it over to this server. And when the server decrypts this message and extracts this field, it'll see whether this field is what it is expecting it to be. What the server is expecting is that this will be Yr plus 1. And if the client is genuine, then the client would've been able to extract Yr from this response that came from the server and generate Yr plus 1 and send it over to the server. </li>  
</ul>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/23.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li> So in other words, this message coming to the server is establishing the genuineness of the client, because this original message that we saw could have been just a replay, meaning somebody sniffed the network got a hold of this packet and replayed it. But if they replayed it, then they would not have the key, and they would not have been able to decrypt this message. So the fact that the client was able to decrypt this message, extract Yr increment it and send a new message that contains Yr plus 1 to the server is authenticating to the server that the client is genuine. Just as this message established to the client that the server is genuine, similarly, this message coming from the client to the server is establishing to the server that the client is genuine. </li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/24.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>And this pair of communication that you are seeing is also avoiding replay attack, either from the client to the server or from the server to the client. In both directions, we're avoiding it by this trick of generating a new random number for establishing an RPC session and communicating it back and forth between the client and server. </li>  
</ul>
    


<h2>10. RPC Session Establishment (cont)</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/25.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>Okay, now we know that the kind is genuine, and the server is genuine. Now what? Once again we go back to the principal of not overexposing any ID or password for too long on insecure links. Remember that the login session used the username and password exactly once. Now the RPC session, you may be establishing multiple RPC sessions. Over the lifetime of the login session. Once a login session has been established, then you are establishing the RPC session. Now within this RPC session, you may be making a number of File system calls. And for all of those file system calls, you don't want to over expose the use of this handshake key. And therefore what the server does in the Andrew file system is to use this handshake key only for establishing an RPC session. Let's say within a login session, you have three or four different RPC sessions with the server. Three or four unique RPC sessions. For each one of those unique RPC sessions, you have to use the handshake key to establish the RPC session. But, within an RPS session, what you're doing, is you're making a whole bunch of secure RPS calls to the server, for opening files, achieving new files, writing files and so on, and for all of those we don't want to over use this handshake key, and that's the reason, what the server does once it validates the indentity of the client, it generates a session key which is for this particular RPC session. 
</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/26.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Now it's going to send this new session key a generated this particular RPC session as an encrypted cipher to the client using the handshake key as the private key for encrypting this message. </li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/27.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>And now Venus can extract the session key using HKC to decrypt this message, and this sk is the session key for use for this particular RPC session. For the rest of this RPC session, any time the client wants to open a file or close a file or write a file or read a file, all of those file system operations is going to use session key as the handshake key for the rest of the RPC session with the server. </li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/28.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>The second field, num, is the starting sequence number that Venus will use on behalf of the client for all the file system related RPC calls that are going to be made as part of this particular session. For this session, sk is going to be the handshake key, and the sequence numbers going to start here. There's again a safeguard against replay attacks on the server, by someone masquerading as a client, and generating packets with certain sequence numbers. </li>  
</ul>

<br>
<p>
  So let's recap what went over for session establishment. The first message coming from the client to the server tells the server that a client wants to establish a new RPC session. The server has to authenticate whether that particular session establishment request is genuine or not. And the client has to know that it is talking to a genuine server, and that's this pair of communication you see here. One is to establish to the client that the server is genuine, the second to establish to the server that the client is genuine. And once the genuineness of the client and the server has been established, the server says, for this particular RPC session, we will not use this handshake key anymore for the rest of the communication that we want to do for file system operations. I will generate a new session key sk, and give it to you along with the sequence number to use as a starting sequence number for RPC calls during this session. That's the whole exchange that we see here. </p>



<h2>11. Sequence Establishment</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/29.JPG?raw=true" alt="drawing" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/30.JPG?raw=true" alt="drawing" width="600"/>
</p>

<h2>12. Login is a Special Case of Bind</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/31.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
  <li>It turns out that login is a special case of the general bind operation that I described to you, and it is a special case in the following sense. Password that the user has for logging into the system at any point of time is used as the handshake key to start with. And username is used as a client ID. So, to initiate the login session, the login process, the username given to the user as the client ID, and the password as the HKC. But other than that, it follows the same sequence of validating itself to the server, and, similarly, validating the server to the client. And at the end of that validation of both the client and the server, what the server gives back to the login process are a pair of tokens, the secret token and the clear token. And these two tokens have to be sent securely on the wire, which is insecure. So what the server will do is encrypt these two using password as the handshake key. And therefore, the login process can use the password to decrypt the message that comes back from the login server, and get these two tokens, secret token and clear token. And these two tokens are kept by Venus for the rest of this login session. And as we know, once we've gotten these two tokens because clear token is a data structure that contains the handshake key needed by Venus for establishing RPC session, the rest of the life is made for us.
  </li>  
</ul>

<h2>13. Putting it all together</h2>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/32.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>Let's put it all together and see how Android file system enables authorized users who are members of a campus community to log in remotely from work station over insecure links and use private key crypto system to validate the users and allow them to get useful work done on the work station. They login using the username and password. They get back a pair of tokens a secret token and clear token, and this is the first communication that happens between venus and vize.
 </li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/33.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li> Venus can then establish an RPC session on behalf of the client by using the secret token and HKC. That's the second class of communication that happens between Venus and Vice, and as a result of this session establishment, what Venus gets back is a session key for use in this particular RPC session.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/34.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li> Subsequently, any time the user opens a file, or closes a file, or writes to a file, all those file system calls that requires Venus to communicate with vice, is going to be sent as secure RPC using the secret token as a client ID and SK, the session key, as the private key for encrypting the message that it wants to send to vice, and similarly vice is going to send back responses uses the session key. So this is a third class of client server interaction that I mentioned.</li>  
</ul>

<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/35.JPG?raw=true" alt="drawing" width="600"/>
</p>
<ul>
 <li>There are three classes of client server interaction. The first one is login, second one is RPC session establishment, and the third one is the actual. Secure RPC calls that are being made for manipulating the files sytem that resides in the central server.</li> 
 <li>So the upshot of this structure that Andrew Files System provides for the user community is that the user name and password is exposed only once per login session. So in other words if a student logs in to a work station once or twice a day, it's only that many number of times that the username and passwords are going to be used on this insecure network.</li> 
 <li>The handshake key that Venus gets back as a result of setting up a login session for a user is used only for new RPC sessions. That's a second class of communication. And the validity of this handshake key is the duration of the login session. You know that passwords have a long ability deed as valid as long as you don't change your password, but the duration of this HKC that is used for RPC session establishment is only for the duration of the login session. Once the user has logged off and left this work station. A secret token and a clear token are thrown away by Venus, so the duration of the HKC is only for this login session. </li> 
 <li>And a session key is used for all the RPC calls that Venus is making on behalf of this client from manipulating the file system. And the duration of SK is the duration of a given RPC session. For instance, within one login session, let's say, I have three different RPC sessions, for each of those distinct RPC sessions, I'm going to get a unique session key. And I'm going to use that session key only for that particular RPC session. Once that is done, I cannot re-use that key anymore. I'll have to re-establish a new RPC session, and get a new session key, and restart the RPC session.</li> 
 <li>So in this lesson module as a whole. First, we looked at taxonomies proposed by Saltzer, and then we saw how we can build a practical system, which can be used by a campus community for securely accessing information. In this case, files stored in a central repository through the Andrew file system. Now, it is time for a question that is really asking you to think about the Andrew file system and its strengths and weaknesses with respect to providing security guarantees for the user community. </li>  
</ul>

<h2>14. AFS Security Report Card</h2>

<p align="center">
   <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/36.JPG?raw=true" alt="drawing" width="600"/>
</p>

<ul>
  <li>mutual suspicion: can I trust the server. Can the server trust the client? Can I trust my fellow users in the community? Is that challenge addressed by AFS?</li> 
  <li>protection from the system for users: can the system do something bad to the users?</li> 
  <li>confinement of resource usage: does the system ensure that no particular user can overuse resources available in the system? the resources could be network bandwidth and so on.</li>   
  <li>authentication: whether it provides authentication in both ways?</li>   
  <li>integrity of the server itself: can the server be compromised in any fashion?</li>     
</ul>


<p align="center">
  <img src="https://github.com/audrey617/CS6210-Advanced-Operating-Systems-Notes/blob/main/img/l11/37.JPG?raw=true" alt="drawing" width="600"/>
</p>


<h2>15. Security in Andrew Conclusion</h2>

<ul>
  <li>Andrew file system has features that extend the privilege levels provided by the unique file system symantics. This includes groups and sub groups and accesses for files with both positive and negative writes. Negative writes is especially useful for quick revocation. Further, it also introduces the notion of audit chain for system administrators modifying the system. I urge you to read this paper in its entirety. To get a feel for the extensions proposed in the Android file system.</li> 
  <li>The main takeaway in this lesson is how, as operating system designers, we can take the best solution that is out there for information security and implement a secure and usable distributed system. Further, once designed and implemented, we should be able to benchmark our solution against the design principles laid out by Saltzer on information security, and know the vulnerabilites that exist in our solution. This last point is particularly important to safeguard our system against attackers. As we mentioned at the outset of this course module, the topic of information security is fascinating, and it is of high relevance in this information age. Hopefully this course module has spurred your interest in information security to seek more in the future courses.
  </li> 
</ul>

