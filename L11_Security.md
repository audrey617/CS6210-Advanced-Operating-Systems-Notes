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
   <li></li> 
   <li></li>   
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
  <li></li> 
  <li></li>   
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
<p align="center">
   <img src="https://www.nicepng.com/png/detail/11-117393_to-be-continued-meme-png-street-sign.png" alt="drawing" width="600"/>
</p>
