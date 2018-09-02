# What-eats-my-server
Curated list of things to check for troubleshooting 

## Table of Contents

- [Hardware]()
    * CPU
    * Memory
    * Load
    * Disk
- [Journalctl]()
- Network

## Hardware

### CPU

*First:* Using `lscpu` command to determine the number of CPUs we have

<details>
<summary><b>How to use `lscpu`?</b></summary><br>


Expample #1: 

    $ lscpu
    Architecture:          x86_64
    CPU op-mode(s):        32-bit, 64-bit
    CPU(s):                4
    Thread(s) per core:    2
    Core(s) per socket:    2
    CPU socket(s):         1
    NUMA node(s):          1
    Vendor ID:             GenuineIntel
    CPU family:            6
    Model:                 37
    Stepping:              5
    CPU MHz:               2667.000
    Virtualization:        VT-x
    L1d cache:             32K
    L1i cache:             32K
    L2 cache:              256K
    L3 cache:              3072K
    NUMA node0 CPU(s):     0-3

In the above my Intel i5 laptop has 4 "CPUs" in total

> CPU(s):                4

of which there are 2 physical cores

> Core(s) per socket:    2

of which each can run up to 2 threads

> Thread(s) per core:    2

at the same time. These threads are the core's logical capabilities.

**Note: Intel refers to a physical processor as a socket.**

From `man lscpu`:

   > CPU<br>
   The logical CPU number of a CPU as used by the Linux kernel.<br>
   CORE<br>
    A core can contain several CPUs.<br>
   SOCKET<br>
   A socket can contain several cores.<br>

Expample #2: Taking it a bit further

    $ lscpu | grep -E '^Thread|^Core|^Socket|^CPU\('
    CPU(s):                32
    Thread(s) per core:    2
    Core(s) per socket:    8
    Socket(s):             2

> CPUs = Threads per core X cores per socket X sockets


</details>



*Second:*

Getting the load averages for the past 1, 5, and 15 minutes.

    $ w 
        09:47:28 up 5 days, 12:37,  2 users,  load average: 0.23, 0.41, 0.47
        USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
        zach     tty1      Mon20   41:38m 11:17   0.00s xinit /home/zach/.xinitrc -- /et
        zach     pts/0     09:33    0.00s  0.01s  0.00s w

Now load averages are 0.23, 0.41, 0.47

<details>
<summary><b>What is load average?</b></summary><br>

Linux load averages are "system load averages" that show the running thread (task) demand on the system as an average number of running plus waiting threads. This measures demand, which can be greater than what the system is currently processing. Most tools show three averages, for 1, 5, and 15 minutes.

Some interpretations:

- If the averages are 0.0, then your system is idle.
- If the 1 minute average is higher than the 5 or 15 minute averages, then load is increasing.
- If the 1 minute average is lower than the 5 or 15 minute averages, then load is decreasing.
- If they are higher than your CPU count, then you might have a performance problem (it depends).

</details>



### Disk
https://unix.stackexchange.com/questions/125429/tracking-down-where-disk-space-has-gone-on-linux
https://unix.stackexchange.com/questions/113840/whats-eating-my-disk-space

## Journalctl
https://superuser.com/a/1188380/760542

## Network
Knowing what serivce lestining on a port
`ss -plnt 'sport = :80'`

