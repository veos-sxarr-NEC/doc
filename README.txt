VEOS:README
Date:Oct-2020

This document describes the information regarding the VEOS version 2.7.2 or 
later.

- CAUTIONS

  (1)Unauthorized reproduction of the contents of this document, in part or in
     its entirety, is prohibited.
  (2)The contents of this document may change without prior notice.
  (3)Every effort has been made to ensure the completeness of this document.
     However, if you have any concerns, or discover errors or omissions, please
     contact your retailer.
  (4)NEC assumes no liability for any loss, including loss of earnings, arising
     from the use of this SW, irrespective of (3) above.
  (5)This SW is not intended for use in medical, nuclear, aerospace, mass
     transit or other applications where human life may be at stake or high
     reliability is required, nor is it intended for use in controlling such
     applications. We disclaim liability for any personal injury and property
     damages caused by such use of this SW.

- About VEOS

  VEOS is the software running on Linux/Vector Host and provides OS 
  functionality for VE programs running on Vector Engine.

- Supported Platforms and Operating Systems

  Operating System         Platform
  RHEL7.6                  x86_64
  RHEL7.7                  x86_64
  RHEL7.8                  x86_64
  RHEL8.1                  x86_64

- VEOS doesn't support Linux suspend and resume. Please do not suspend and
  resume your Linux/x86 machine where VEs are installed. When you suspend and
  resume the Linux/x86 machine, Linux will be down in the worst case, such
  as kernel panic etc. This causes your important data loss.

- VEOS doesn't support Linux which works on any virtual machine, such as QEMU
  etc. VEOS can work correctly only on Linux which works directly on the
  physical machine.

- The ve_exec command which is a part of VEOS uses over 1TB of VH virtual 
  address space. VEOS requires that Linux kernel parameter vm.overcommit_memory
  is not '2' to enable "overcommit".

- Please do not apply excessive loads on your Linux/x86 machine to make VEOS to
  be able to provide OS functionality steadily. Excessive loads on Linux/x86
  machine may cause unexpected performance degrades on VEOS such as long
  response time.

- The number of threads of VE processes which run on VE should be less
  than or equal to the number of available VE cores, in order to
  achieve best performance.

- The getaddrinfo() function in VE glibc fails if a program invokes it
  in a docker container which disabled IPv6 support. Please enable
  IPv6 support of docker environment.

- MPIPROGINF/PROGINF and some commands such as "ps" and "veswap" show
  the VE memory usage and non-swappable VE memory usage which are
  obtained from VEOS. The getrusage() function also obtains VE memory
  usage from VEOS. These value includes pinned VE memory to be
  transferred by MPI API, even if it is already unmapped from a
  virtual address space of a VE process. But, there is a restriction
  that these values do not include pinned memory if the type of memory
  is one of the following types, and if it is already unmapped from a
  virtual address space of a VE process.

    Private filebacked memory (mmap() with MAP_PRIVATE)
    Shared filebacked memory  (mmap() with MAP_SHARED)
    Shared anonymous memory   (mmap() with MAP_SHARED and MAP_ANONYMOUS)
    System V shared memory    (shmat())
    POSIX shared memory       (shm_open())

- The VEO implementation has changed to Alternative VE Offload(AVEO).
  It is a faster and much lower latency replacement to the previous
  VEO implementation which brings multi-VE support, simultaneous
  debugging of VE and VH side, API extensions. 
  You can migrate to AVEO from the previous VEO implementation by
  installing the AVEO's packages and re-linking your program with AVEO
  without modification of makefiles.
  NEC supports the previous VEO implementation until the end of Mar. 2021.
  Please see VEOS document "The Tutorial and API Reference of
  Alternative VE Offloading" for VEO migration and AVEO installation.
