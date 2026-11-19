Attached below is the link to yt video just in case the mp4 format doesn't work: 
https://youtu.be/xNSNJqUY68s


further documentation would be done here. 

In the creation and development of this assignment 3 I was tasked with implementing and demonstrating qlocks.

firstly I had to open the xv6 folder and made use of the videos given in the assignment, to get an idea as to how to implemented. 

after which following the idea of the previous assignment I created the files kernel/qlock.h and kernel/qlock.c
within the qlock.h I added the structure of the qlock, using spinlock to protect the locks information and a variable named locked to check if the lock was being used. after which i added next_ticket and serving_ticket to keep track of the processes and in what order they came in.

the idea behind using ticketing system was using some of the code and ideas presented during the qlock lesson.

Within qlock.c, I created the initqlock() function to initialize qlock.

This made the lock be set 0 or available state and set the ticket system to work by setting the serving and next ticket at 0

after this I implemented acquireq() which is responsible for the processes being able to acquire the lock if available and deny access if currently being used. under the idea that it is being used a ticket will be assigned to the process, upon the current process holding the lock finishing the next ticket will be called. Thus allowing the awaiting process to sleep using the sleeping functions of xv6.

next was to implement releaseq, in which the process finished using the shared resource so qlock is set back to 0. serving_ticket is increased by 1 and the next process with that ticket number takes hold of the lock.

after which I had to connect the qlock functions to xv6, this is because otherwise the qlock would not be able to be tested/demonstrated!

first i modified main.c to initialize the shared qlock when xv6 starts. or for simpler terms so when make qemu starts and xv6 is running qlock will be ready to use!

next i modified sysproc.c to add "sys_qlocktest()" which allows the qlock test to be called through a system call such as qlocktest.

now to be able to use said system call, I had to modify the kernel/syscall.h and kernel/syscall.c

in .h I added the define sys_qlocktest 24 because i added it all the way at the bottom after qlock 23 which was from the previous assignment

in .c i added a function declaration and registered the system call so that xv6 can know what function is supposed to run when the system call is used.

after registering my system call, i had to modify sysproc.c to add the function sys_qlocktest()

this functions calls qlock_concurrent_test(), which just runs the test that was created for the demonstration later on. all that it did was allow the test to be started through the system call rather than having to run it directly from the kernel.

next I modified user/user.h to add "int qlocktest(void)" simply allows user programs to call it

and user/usys.pl added the entry qlocktest to allow the user program to request the system call from the xv6 kernel

after finishing the system call changes/modifications i created user/qlocktest.c to actually test the qlock.

with the use of fork() i created 3 child processes each one calls qlocktest() to run the test in kernel. 

the idea was to do what the assignment asked me to and check if my qlock could handle multiple processes could share the qlock without interfering with one another.
as shown in the video it did in fact work to my relief.

lastly I wanna touch on makefile because I added qlock.o and _qlocktest which compiled the new qlock implementation.

funnily enough a majority of my time was spend debugging my own code, because everytime i tried to compile qlock i had either mistyped something or there was a missing declaration, or even the complete opposite and i double decleared something which meant although I could see where the issue was, after spending a good time writing things it had gotten tiring and my process became slower. after spending hours I was finally able to implement things accurately and run it 
