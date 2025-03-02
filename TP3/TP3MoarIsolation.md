# TP3: Moar Isolation

## Part I : A bit of exploration

### 1. /proc

**🌞 Afficher... :**

-   l'état complet de la mémoire (RAM):

```bash
[it4@localhost ~]$ cat /proc/meminfo
    MemTotal:        1817284 kB
    MemFree:         1473196 kB
    MemAvailable:    1522044 kB
    Buffers:            2708 kB
    Cached:           166308 kB
    SwapCached:            0 kB
    Active:           144816 kB
    Inactive:          88756 kB
    Active(anon):      69604 kB
    Inactive(anon):        0 kB
    Active(file):      75212 kB
    Inactive(file):    88756 kB
    Unevictable:           0 kB
    Mlocked:               0 kB
    SwapTotal:        839676 kB
    SwapFree:         839676 kB
    Zswap:                 0 kB
    Zswapped:              0 kB
    Dirty:                 0 kB
    Writeback:             0 kB
    AnonPages:         63956 kB
    Mapped:            40188 kB
    Shmem:              5048 kB
    KReclaimable:      31532 kB
    Slab:              62684 kB
    SReclaimable:      31532 kB
    SUnreclaim:        31152 kB
    KernelStack:        1708 kB
    PageTables:         1452 kB
    SecPageTables:         0 kB
    NFS_Unstable:          0 kB
    Bounce:                0 kB
    WritebackTmp:          0 kB
    CommitLimit:     1748316 kB
    Committed_AS:     182808 kB
    VmallocTotal:   34359738367 kB
    VmallocUsed:       21152 kB
    VmallocChunk:          0 kB
    Percpu:              376 kB
    HardwareCorrupted:     0 kB
    AnonHugePages:     14336 kB
    ShmemHugePages:        0 kB
    ShmemPmdMapped:        0 kB
    FileHugePages:         0 kB
    FilePmdMapped:         0 kB
    CmaTotal:              0 kB
    CmaFree:               0 kB
    Unaccepted:            0 kB
    HugePages_Total:       0
    HugePages_Free:        0
    HugePages_Rsvd:        0
    HugePages_Surp:        0
    Hugepagesize:       2048 kB
    Hugetlb:               0 kB
    DirectMap4k:       92096 kB
    DirectMap2M:     2004992 kB
```

-    le nombre de cœurs du CPU:

```bash
    [it4@localhost ~]$ grep -c ^processor /proc/cpuinfo
    1
```

-     le nombre de processus lancés:

```bash
    [it4@localhost ~]$ ls /proc | grep -c ^[0-9]
    103
```

-     la ligne de commande utilisée pour lancer le kernel actuel:

```bash
    [it4@localhost ~]$ cat /proc/cmdline
    BOOT_IMAGE=(hd0,msdos1)/vmlinuz-5.14.0-503.14.1.el9_5.x86_64 root=/dev/mapper/rl-root ro crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M resume=/dev/mapper/rl-swap rd.lvm.lv=rl/root rd.lvm.lv=rl/swap
```

-   la liste des connexions TCP actuelles:

```bash
    [it4@localhost ~]$ cat /proc/net/tcp
      sl  local_address rem_address   st tx_queue rx_queue tr tm->when retrnsmt   uid  timeout inode                                                     
       0: 00000000:0016 00000000:0000 0A 00000000:00000000 00:00000000 00000000     0        0 20394 1 0000000000000000 100 0 0 10 0                     
       1: 7138A8C0:0016 0138A8C0:ACE4 01 00000000:00000000 02:0009CFCD 00000000     0        0 26550 4 0000000000000000 20 6 21 10 -1                    
```

-   la valeur actuelle de la swappiness:

```bash
    [it4@localhost ~]$ cat /proc/sys/vm/swappiness
    60
```

### 2. /sys

**🌞 Afficher... :**

-   la liste des périphériques de types bloc reconnus par l'OS

```bash
    [it4@localhost ~]$ ls /sys/block
    dm-0  dm-1  sda
```

-   la liste des modules kernel qui sont actuellements en cours d'utilisation

```bash
    [it4@localhost ~]$ ls /sys/module
    8250         configfs          debug_core      edac_core            hid_magicmouse                 intel_vsec  libcrc32c       nf_nat          nft_reject       pmt_telemetry  rtc_cmos      suspend       udmabuf                vt
    acpi         cpufreq           device_hmem     efi_pstore           hid_ntrig                      ip_set      md_mod          nfnetlink       nft_reject_inet  printk         scsi_dh_alua  sysrq         uhci_hcd               watchdog
    acpiphp      cpuidle           dm_log          efivars              i2c_piix4                      ipv6        memory_hotplug  nf_reject_ipv4  nmi_backtrace    processor      scsi_dh_rdac  t10_pi        usbcore                wmi
    ahci         cpuidle_haltpoll  dm_mirror       ehci_hcd             i8042                          kdb         microcode       nf_reject_ipv6  page_alloc       psmouse        scsi_mod      tcp_cubic     usbhid                 workqueue
    ata_generic  crc32c_intel      dm_mod          fb                   ima                            kernel      module          nf_tables       page_reporting   pstore         sd_mod        thermal       uv_nmi                 xen
    ata_piix     crc32_pclmul      dm_region_hash  firmware_class       intel_idle                     keyboard    mousedev        nft_chain_nat   pcie_aspm        random         secretmem     thunderbolt   video                  xfs
    battery      crc64_rocksoft    drm             fuse                 intel_pmc_core                 kfence      msr             nft_ct          pciehp           rapl           serio_raw     tpm           virtio_pci             xhci_hcd
    blk_cgroup   crc_t10dif        drm_kms_helper  ghash_clmulni_intel  intel_powerclamp               kgdboc      netpoll         nft_fib         pci_hotplug      rcupdate       sg            tpm_crb       virtio_pci_legacy_dev  xz_dec
    block        crct10dif_pclmul  drm_ttm_helper  gpiolib_acpi         intel_rapl_common              kgdbts      nf_conntrack    nft_fib_inet    pcmcia_core      rcutree        shpchp        tpm_tis       virtio_pci_modern_dev  zswap
    button       cryptomgr         dynamic_debug   haltpoll             intel_rapl_msr                 libahci     nf_defrag_ipv4  nft_fib_ipv4    pcspkr           rfkill         spurious      tpm_tis_core  vmd
    clocksource  damon_reclaim     e1000           hid                  intel_uncore_frequency_common  libata      nf_defrag_ipv6  nft_fib_ipv6    pmt_class        rng_core       srcutree      ttm           vmwgfx
```

-   la liste des cartes réseau

```bash
    [it4@localhost ~]$ ls /sys/class/net
    enp0s3  enp0s8  enp0s9  lo
```

## Part II : Gotta get chrooty

### 1. Play manually

**🌞 Créez le dossier /srv/get_chrooted/**

```bash
    [it4@localhost ~]$ sudo mkdir -p /srv/get_chrooted/

    We trust you have received the usual lecture from the local System
    Administrator. It usually boils down to these three things:

        #1) Respect the privacy of others.
        #2) Think before you type.
        #3) With great power comes great responsibility.

    [sudo] password for it4: 
```

**🌞 Essayez de chroot à l'intérieur en lançant un shell**

-   déplacez le nécessaire dans /srv/get_chrooted/ pour pouvoir lancé un shell chrooté à l'intérieur

```bash
    [it4@localhost ~]$ sudo cp /bin/bash /srv/get_chrooted/
    [it4@localhost ~]$ ldd /bin/bash
	    linux-vdso.so.1 (0x00007ffdc2df8000)
	    libtinfo.so.6 => /lib64/libtinfo.so.6 (0x00007f67cf787000)
	    libc.so.6 => /lib64/libc.so.6 (0x00007f67cf400000)
	    /lib64/ld-linux-x86-64.so.2 (0x00007f67cf917000)
    [it4@localhost ~]$ sudo mkdir -p /srv/get_chrooted/lib64/
    [it4@localhost ~]$ sudo cp /lib64/ld-linux-x86-64.so.2 /srv/get_chrooted/lib64/
    [it4@localhost ~]$ sudo mkdir -p /srv/get_chrooted/lib/x86_64-linux-gnu/
    [it4@localhost ~]$ sudo cp /usr/lib64/libc.so.6 /srv/get_chrooted/lib64/
    [it4@localhost ~]$ sudo cp /usr/lib64/libtinfo.so.6 /srv/get_chrooted/lib64/
    [it4@localhost ~]$ sudo chroot /srv/get_chrooted /bash
    bash-5.1# 

```

### 2. SSH old friend

**🌞 Créez un user imsad**

```bash
    [it4@localhost ~]$ sudo useradd imsad
    [sudo] password for it4: 
    [it4@localhost ~]$ sudo passwd imsad
    Changing password for user imsad.
    New password: 
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: 
    passwd: all authentication tokens updated successfully.
    [it4@localhost ~]$ grep imsad /etc/passwd
    imsad:x:1001:1001::/home/imsad:/bin/bash
```

**🌞 Modifier la configuration du serveur SSH**

```bash

    [it4@localhost ~]$ sudo mkdir -p /srv/get_chrooted/{bin,home/imsad,lib64,usr/lib64,usr/libexec}
    [it4@localhost ~]$ sudo ln -s usr/lib64 /srv/get_chrooted/lib64
    [it4@localhost ~]$ sudo cp /bin/bash /srv/get_chrooted/bin/
    [it4@localhost ~]$ sudo cp -a /lib64/libreadline.so.* /lib64/libdl.so.* /lib64/libc.so.* /lib64/ld-linux-x86-64.so.* /srv/get_chrooted/lib64/
    [it4@localhost ~]$ sudo mkdir -p /srv/get_chrooted/home/imsad/.ssh
    [it4@localhost ~]$ sudo chmod 700 /srv/get_chrooted/home/imsad/.ssh
    [it4@localhost ~]$ sudo cp /usr/lib64/libtinfo.so.6 /srv/get_chrooted/usr/lib64/
    [it4@localhost ~]$ sudo chmod 755 /srv/get_chrooted/usr/lib64/libtinfo.so.6
    [it4@localhost ~]$ sudo nano /etc/ssh/sshd_config

    Match User imsad
        ChrootDirectory /srv/get_chrooted
        ForceCommand /bin/bash
        AllowTcpForwarding no
        X11Forwarding no

    [it4@localhost ~]$ ssh-keygen -t ed25519 -f ~/.ssh/imsad_key
    Generating public/private ed25519 key pair.
    Created directory '/home/it4/.ssh'.
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/it4/.ssh/imsad_key
    Your public key has been saved in /home/it4/.ssh/imsad_key.pub
    The key fingerprint is:
    SHA256:y9Y7gQ+q9VOm+Z9+4ThMRXeVlEZPUy4h5x6Iu2lmXJs it4@localhost.localdomain
    The key's randomart image is:
    +--[ED25519 256]--+
    |            . =+O|
    |           . =.B=|
    |          . ..=.+|
    |           . ..o |
    |        S.. ...  |
    |       .o+++.o.  |
    |      ..+BO+Eo . |
    |     ..o++o.+.o  |
    |    ..  .oo++o   |
    +----[SHA256]-----+
    [it4@localhost ~]$ sudo cp ~/.ssh/imsad_key.pub /srv/get_chrooted/home/imsad/.ssh/authorized_keys
    [it4@localhost ~]$ sudo chown imsad:imsad /srv/get_chrooted/home/imsad/.ssh/authorized_keys
    [it4@localhost ~]$ sudo chmod 600 /srv/get_chrooted/home/imsad/.ssh/authorized_keys

    [it4@localhost ~]$ sudo systemctl restart sshd
    [it4@localhost ~]$ ssh -i ~/.ssh/imsad_key imsad@localhost
    imsad@localhost's password: 
    bash-5.1$ echo $0
    /bin/bash

```

## Part III : CGroup

### 1. Explore

**🌞 Afficher... :**

```bash
    cpuset cpu io memory hugetlb pids rdma misc
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/user.slice/user-$(id -u).slice/memory.max
    max
    [it4@efrei-xmg4agau1 ~]$ find /sys/fs/cgroup -mindepth 1 -maxdepth 1 -type d
    /sys/fs/cgroup/sys-fs-fuse-connections.mount
    /sys/fs/cgroup/sys-kernel-config.mount
    /sys/fs/cgroup/sys-kernel-debug.mount
    /sys/fs/cgroup/dev-mqueue.mount
    /sys/fs/cgroup/user.slice
    /sys/fs/cgroup/sys-kernel-tracing.mount
    /sys/fs/cgroup/init.scope
    /sys/fs/cgroup/system.slice
    /sys/fs/cgroup/dev-hugepages.mount
```

### 2. Do it

**🌞 Créer un nouveau CGroup :**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo mkdir /sys/fs/cgroup/meow
    [sudo] password for it4: 
    [it4@efrei-xmg4agau1 ~]$ echo "+cpu +cpuset +memory" | sudo tee /sys/fs/cgroup/cgroup.subtree_control
    +cpu +cpuset +memory
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/cgroup.controllers
    cpuset cpu memory pids
```

**🌞 Créer un nouveau sous-CGroup :**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo mkdir /sys/fs/cgroup/meow/task1
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/task1/cgroup.controllers
    [it4@efrei-xmg4agau1 ~]$ echo "+cpuset +cpu +memory +pids" | sudo tee /sys/fs/cgroup/meow/cgroup.subtree_control
    +cpuset +cpu +memory +pids
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/task1/cgroup.controllers
    cpuset cpu memory pids
```

**🌞 Mettez en place une limitation RAM**

```bash
    [it4@efrei-xmg4agau1 ~]$ echo 150M | sudo tee /sys/fs/cgroup/meow/task1/memory.max
    150M
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/task1/memory.max
    157286400
```

**🌞 Prouvez que la limite est effective**

```bash
    [it4@efrei-xmg4agau1 ~]$ stress-ng --vm 1 --vm-bytes 80%
    stress-ng: info:  [11505] defaulting to a 1 day, 0 secs run per stressor
    stress-ng: info:  [11505] dispatching hogs: 1 vm

    [it4@efrei-xmg4agau1 ~]$ free -m
                   total        used        free      shared  buff/cache   available
    Mem:            1774        1392         240          35         320         382
    Swap:            819           0         819

    [it4@efrei-xmg4agau1 ~]$ echo $$ | sudo tee /sys/fs/cgroup/meow/task1/cgroup.procs
    1256
    [it4@efrei-xmg4agau1 ~]$ stress-ng --vm 1 --vm-bytes 200M
    stress-ng: info:  [11555] defaulting to a 1 day, 0 secs run per stressor
    stress-ng: info:  [11555] dispatching hogs: 1 vm

    [it4@efrei-xmg4agau1 ~]$ free -m
                   total        used        free      shared  buff/cache   available
    Mem:            1774         494        1138           4         290        1280
    Swap:            819         139         680
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/task1/memory.current
    156409856
```

**🌞 Créer un nouveau sous-CGroup :**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo mkdir /sys/fs/cgroup/meow/task2
    [sudo] password for it4: 
    [it4@efrei-xmg4agau1 ~]$ cat /sys/fs/cgroup/meow/task2/cgroup.controllers
    cpuset cpu memory pids
```

**🌞 Appliquer des restrictions CPU :**

```bash
    [it4@efrei-xmg4agau1 ~]$ echo "100" | sudo tee /sys/fs/cgroup/meow/task1/cpu.weight
    [sudo] password for it4: 
    100
    [it4@efrei-xmg4agau1 ~]$ echo "200" | sudo tee /sys/fs/cgroup/meow/task2/cpu.weight
    200
```

- Terminal 1
```bash
    [it4@efrei-xmg4agau1 ~]$ echo $$ | sudo tee /sys/fs/cgroup/meow/task1/cgroup.procs
    1256
    [it4@efrei-xmg4agau1 ~]$ stress-ng --cpu 1 --timeout 0
    stress-ng: info:  [12244] setting to a 0 secs run per stressor
    stress-ng: info:  [12244] dispatching hogs: 1 cpu
```

- Terminal 2
```bash
    [it4@efrei-xmg4agau1 ~]$ echo $$ | sudo tee /sys/fs/cgroup/meow/task2/cgroup.procs
    [sudo] password for it4: 
    11517
    [it4@efrei-xmg4agau1 ~]$ stress-ng --cpu 1 --timeout 0
    stress-ng: info:  [12246] setting to a 0 secs run per stressor
    stress-ng: info:  [12246] dispatching hogs: 1 cpu
```

- Terminal 3
```bash
    [it4@efrei-xmg4agau1 ~]$ top
    top - 10:59:16 up  2:17,  3 users,  load average: 0.68, 0.16, 0.05
    Tasks: 109 total,   3 running, 106 sleeping,   0 stopped,   0 zombie
    %Cpu(s): 92.7 us,  2.4 sy,  0.0 ni,  0.0 id,  0.0 wa,  4.9 hi,  0.0 si,  0.0 st
    MiB Mem :   1774.7 total,   1185.8 free,    371.7 used,    426.7 buff/cache
    MiB Swap:    820.0 total,    819.7 free,      0.3 used.   1403.0 avail Mem 

      12466 it4       20   0   51812   7532   3712 R  66.1   0.4   0:34.08 stress-ng-cpu                                                                                                                                                                          
      12464 it4       20   0   51812   7528   3712 R  33.0   0.4   0:22.45 stress-ng-cpu                                                                                                                                                                          
```

### 3. systemd

#### A. One-shot

**🌞 Lancer un serveur Web Python sous forme de service temporaire**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo firewall-cmd --add-port=8888/tcp --permanent
    [sudo] password for it4: 
    success
    [it4@efrei-xmg4agau1 ~]$ sudo firewall-cmd --reload
    success
    [it4@efrei-xmg4agau1 ~]$ sudo systemd-run -u python_web_server python -m http.server 8888
    Running as unit: python_web_server.service
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl status python_web_server
    ● python_web_server.service - /bin/python -m http.server 8888
         Loaded: loaded (/run/systemd/transient/python_web_server.service; transient)
      Transient: yes
         Active: active (running) since Wed 2025-02-26 11:05:33 CET; 6s ago
       Main PID: 12518 (python)
          Tasks: 1 (limit: 11097)
         Memory: 9.2M
            CPU: 35ms
         CGroup: /system.slice/python_web_server.service
                 └─12518 /bin/python -m http.server 8888

    Feb 26 11:05:33 efrei-xmg4agau1.campus.villejuif systemd[1]: Started /bin/python -m http.server 8888.
```

**🌞 Appliquer à la volée des restrictions**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo systemd-run -u meow_test -p MemoryMax=234M sleep 9999
    Running as unit: meow_test.service
    [it4@efrei-xmg4agau1 ~]$ systemctl status meow_test
    ● meow_test.service - /bin/sleep 9999
         Loaded: loaded (/run/systemd/transient/meow_test.service; transient)
      Transient: yes
         Active: active (running) since Wed 2025-02-26 11:10:52 CET; 24s ago
       Main PID: 12581 (sleep)
          Tasks: 1 (limit: 11097)
         Memory: 196.0K (max: 234.0M available: 233.8M)
            CPU: 652us
         CGroup: /system.slice/meow_test.service
                 └─12581 /bin/sleep 9999

    Feb 26 11:10:52 efrei-xmg4agau1.campus.villejuif systemd[1]: Started /bin/sleep 9999.
    [it4@efrei-xmg4agau1 ~]$ systemctl show meow_test -p MemoryMax
    MemoryMax=245366784

```

**🌞 Restrictions CGroup ?**

```bash
    [it4@efrei-xmg4agau1 ~]$ grep -nri $(( 234 * 1024 * 1024 )) /sys/fs/cgroup
    ...
    /sys/fs/cgroup/system.slice/meow_test.service/memory.max:1:245366784
    ...
```

#### B. Real service

**🌞 Créez un service web.service**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo nano /etc/systemd/system/web.service
    [sudo] password for it4: 
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl daemon-reload
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl enable web.service
    Created symlink /etc/systemd/system/multi-user.target.wants/web.service → /etc/systemd/system/web.service.
    [it4@efrei-xmg4agau1 ~]$ sudo firewall-cmd --add-port=9999/tcp --permanent
    success
    [it4@efrei-xmg4agau1 ~]$ sudo firewall-cmd --reload
    success
    [it4@localhost ~]$ sudo mkdir -p /var/www/html
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl start web.service
    [it4@localhost ~]$ sudo systemctl status web.service
    ● web.service - Python Web Server
         Loaded: loaded (/etc/systemd/system/web.service; enabled; preset: disabled)
         Active: active (running) since Thu 2025-02-27 00:06:47 CET; 4s ago
       Main PID: 1622 (python3)
          Tasks: 1 (limit: 11097)
         Memory: 9.3M
            CPU: 58ms
         CGroup: /system.slice/web.service
                 └─1622 /usr/bin/python3 -m http.server 9999

    Feb 27 00:06:47 localhost.localdomain systemd[1]: Started Python Web Server.
```

- web.service
```bash
    [Unit]
    Description=Python Web Server
    After=network.target

    [Service]
    ExecStart=/usr/bin/python3 -m http.server 9999
    WorkingDirectory=/var/www/html
    Restart=always
    User=it4

    [Install]
    WantedBy=multi-user.target
```

**🌞 Restriction CGroup**

```bash
    [it4@localhost ~]$ sudo nano /etc/systemd/system/web.service
        
        [Service]
        MemoryMax=321M
        IOWriteBandwidthMax=/dev/sda 1M
        IOReadBandwidthMax=/dev/sda 1M
        CPUQuota=50%
    
    [it4@localhost ~]$ sudo systemctl daemon-reload
    [it4@localhost ~]$ sudo systemctl restart web.service
    [it4@localhost ~]$ sudo systemctl status web.service
    ● web.service - Python Web Server
         Loaded: loaded (/etc/systemd/system/web.service; enabled; preset: disabled)
         Active: active (running) since Thu 2025-02-27 00:17:25 CET; 3s ago
       Main PID: 1743 (python3)
          Tasks: 1 (limit: 11097)
         Memory: 8.9M (max: 321.0M available: 312.0M)
            CPU: 40ms
         CGroup: /system.slice/web.service
                 └─1743 /usr/bin/python3 -m http.server 9999

    Feb 27 00:17:25 localhost.localdomain systemd[1]: Started Python Web Server.

```

**🌞 Prouvez que ces restrictions ont été mises en place avec les CGroups**

```bash
    [it4@localhost ~]$ cat /sys/fs/cgroup/system.slice/web.service/memory.max
    336592896
    [it4@localhost ~]$ cat /sys/fs/cgroup/system.slice/web.service/io.max
    8:0 rbps=1000000 wbps=1000000 riops=max wiops=max
    [it4@localhost ~]$ cat /sys/fs/cgroup/system.slice/web.service/cpu.max
    50000 100000
```

## Part IV : Namespaces

### 1. Explore

**🌞 Utiliser /proc**

```bash
    [it4@efrei-xmg4agau1 ~]$ ls -l /proc/self/ns
    total 0
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 cgroup -> 'cgroup:[4026531835]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 ipc -> 'ipc:[4026531839]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 mnt -> 'mnt:[4026531841]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 net -> 'net:[4026531840]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 pid -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 pid_for_children -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 time -> 'time:[4026531834]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 time_for_children -> 'time:[4026531834]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 user -> 'user:[4026531837]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 09:17 uts -> 'uts:[4026531838]'
    [it4@efrei-xmg4agau1 ~]$ ls -l /proc/1/ns
    ls: cannot open directory '/proc/1/ns': Permission denied
    [it4@efrei-xmg4agau1 ~]$ sudo ls -l /proc/1/ns
    [sudo] password for it4: 
    total 0
    lrwxrwxrwx. 1 root root 0 Feb 27 08:54 cgroup -> 'cgroup:[4026531835]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 ipc -> 'ipc:[4026531839]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 mnt -> 'mnt:[4026531841]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 net -> 'net:[4026531840]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 pid -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 pid_for_children -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 time -> 'time:[4026531834]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 time_for_children -> 'time:[4026531834]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 user -> 'user:[4026531837]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 uts -> 'uts:[4026531838]'
```

**🌞 Lister tous les namespaces en cours d'utilisation**

```bash
    [it4@efrei-xmg4agau1 ~]$ lsns
            NS TYPE   NPROCS   PID USER COMMAND
    4026531834 time        4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531835 cgroup      4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531836 pid         4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531837 user        4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531838 uts         4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531839 ipc         4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531840 net         4   717 it4  /usr/bin/python3 -m http.server 9999
    4026531841 mnt         4   717 it4  /usr/bin/python3 -m http.server 9999
```

### 2. Create

#### A. net

**🌞 Créer un nouveau namespace network**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo unshare -n bash
    [sudo] password for it4: 
    [root@efrei-xmg4agau1 it4]# ip link set lo up
    [root@efrei-xmg4agau1 it4]# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host 
           valid_lft forever preferred_lft forever
```

**🌞 Prouvez que votre nouveau namespace est bien là**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo unshare -n bash
    [sudo] password for it4: 
    [root@efrei-xmg4agau1 it4]# ip a
    1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    [root@efrei-xmg4agau1 it4]# lsns --type net
            NS TYPE NPROCS   PID USER    NETNSID NSFS COMMAND
    4026531840 net     103     1 root unassigned      /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026532165 net       2  2452 root unassigned      bash
    [root@efrei-xmg4agau1 it4]# ls -l /proc/self/ns/net
    lrwxrwxrwx. 1 root root 0 Feb 27 10:50 /proc/self/ns/net -> 'net:[4026532165]'
    [root@efrei-xmg4agau1 it4]# ls -l /proc/1/ns/net
    lrwxrwxrwx. 1 root root 0 Feb 27 09:18 /proc/1/ns/net -> 'net:[4026531840]'
```

#### B. pid

**🌞 Créer un nouveau namespace pid**

```bash
[root@efrei-xmg4agau1 it4]# sudo unshare --pid --fork --mount-proc bash
```

**🌞 Prouvez que votre nouveau namespace est bien là**

```bash
    [root@efrei-xmg4agau1 it4]# ps -ef
    UID          PID    PPID  C STIME TTY          TIME CMD
    root           1       0  0 11:11 pts/0    00:00:00 bash
    root          24       1  0 11:14 pts/0    00:00:00 ps -ef
    [root@efrei-xmg4agau1 it4]# lsns --type pid
            NS TYPE NPROCS PID USER COMMAND
    4026532252 pid       2   1 root bash
    [root@efrei-xmg4agau1 it4]# ls -l /proc/self/ns/pid
    lrwxrwxrwx. 1 root root 0 Feb 27 11:14 /proc/self/ns/pid -> 'pid:[4026532252]'
    [root@efrei-xmg4agau1 it4]# ls -l /proc/1/ns/pid
    lrwxrwxrwx. 1 root root 0 Feb 27 11:12 /proc/1/ns/pid -> 'pid:[4026532252]'
    [root@efrei-xmg4agau1 it4]# ls -al /proc/1/ns/pid
```

### 3. AND MY CONTAINERS

#### A. Quick install

**🌞 Installer Docker sur la machine**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    [sudo] password for it4: 
    Adding repo from: https://download.docker.com/linux/centos/docker-ce.repo
    [it4@efrei-xmg4agau1 ~]$ sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    Docker CE Stable - x86_64                                                                                                                                                                                                      49 kB/s |  65 kB     00:01    
    Last metadata expiration check: 0:00:01 ago on Thu 27 Feb 2025 11:17:44 AM CET.
    Dependencies resolved.
    ==============================================================================================================================================================================================================================================================
     Package                                                                Architecture                                        Version                                                       Repository                                                     Size
    ==============================================================================================================================================================================================================================================================
    Installing:
     containerd.io                                                          x86_64                                              1.7.25-3.1.el9                                                docker-ce-stable                                               43 M
     docker-buildx-plugin                                                   x86_64                                              0.21.1-1.el9                                                  docker-ce-stable                                               16 M
    ....

    Installed:
      container-selinux-3:2.232.1-1.el9.noarch     containerd.io-1.7.25-3.1.el9.x86_64    docker-buildx-plugin-0.21.1-1.el9.x86_64    docker-ce-3:28.0.1-1.el9.x86_64    docker-ce-cli-1:28.0.1-1.el9.x86_64    docker-ce-rootless-extras-28.0.1-1.el9.x86_64   
      docker-compose-plugin-2.33.1-1.el9.x86_64    fuse-common-3.10.2-9.el9.x86_64        fuse-overlayfs-1.14-1.el9.x86_64            fuse3-3.10.2-9.el9.x86_64          fuse3-libs-3.10.2-9.el9.x86_64         libslirp-4.4.0-8.el9.x86_64                     
      slirp4netns-1.3.1-1.el9.x86_64               tar-2:1.34-7.el9.x86_64               

    Complete!


    [it4@efrei-xmg4agau1 ~]$ sudo usermod -aG docker $(whoami)
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl start docker
    [it4@efrei-xmg4agau1 ~]$ sudo systemctl enable --now docker
    Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /usr/lib/systemd/system/docker.service.
    [it4@efrei-xmg4agau1 ~]$ sudo docker run hello-world
    Unable to find image 'hello-world:latest' locally
    latest: Pulling from library/hello-world
    e6590344b1a5: Pull complete 
    Digest: sha256:e0b569a5163a5e6be84e210a2587e7d447e08f87a0e90798363fa44a0464a1e8
    Status: Downloaded newer image for hello-world:latest

    Hello from Docker!
    ...
```

#### B. A simple container

**🌞 Lancer un simple conteneur qui sleep**

```bash
    [it4@efrei-xmg4agau1 ~]$ sudo docker run -d debian sleep 9999
    53a05533a41113ba7e8418994fcbaa6b13d6798ec9618368507e64388cfd4ece
```

**🌞 Avec les commandes de votre choix, avec le plus de détails possible, prouvez-que :**

- ce processus sleep est bien lancé sur votre machine, par votre utilisateur courant
```bash
    [it4@efrei-xmg4agau1 ~]$ sudo docker run -d debian sleep 9999
    dee1191c367937583c3229ab1033d00f47bdd3573ae6c35d135914e9d96ad0f8
    [it4@efrei-xmg4agau1 ~]$ ps aux | grep sleep
    root        6003  0.1  0.0   2484  1280 ?        Ss   12:41   0:00 sleep 9999
    it4         6018  0.0  0.1   6408  2176 pts/0    S+   12:41   0:00 grep --color=auto sleep
    [it4@efrei-xmg4agau1 ~]$ sudo docker ps
    CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
    dee1191c3679   debian    "sleep 9999"   21 seconds ago   Up 20 seconds             lucid_maxwell
```

- ce processus sleep est isolé à l'aide de namespaces
```bash
    [it4@efrei-xmg4agau1 ~]$ ls -al /proc/$$/ns
    total 0
    dr-x--x--x. 2 it4 it4 0 Feb 27 13:02 .
    dr-xr-xr-x. 9 it4 it4 0 Feb 27 13:02 ..
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 cgroup -> 'cgroup:[4026531835]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 ipc -> 'ipc:[4026531839]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 mnt -> 'mnt:[4026531841]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 net -> 'net:[4026531840]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 pid -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 pid_for_children -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 time -> 'time:[4026531834]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 time_for_children -> 'time:[4026531834]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 user -> 'user:[4026531837]'
    lrwxrwxrwx. 1 it4 it4 0 Feb 27 13:02 uts -> 'uts:[4026531838]'
    [it4@efrei-xmg4agau1 ~]$ sudo !!
    sudo ls -al /proc/385/ns
    total 0
    dr-x--x--x. 2 root root 0 Feb 27 09:40 .
    dr-xr-xr-x. 9 root root 0 Feb 27 08:54 ..
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 cgroup -> 'cgroup:[4026531835]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 ipc -> 'ipc:[4026531839]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 mnt -> 'mnt:[4026531841]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 net -> 'net:[4026531840]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 pid -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 root root 0 Feb 27 13:03 pid_for_children -> 'pid:[4026531836]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 time -> 'time:[4026531834]'
    lrwxrwxrwx. 1 root root 0 Feb 27 13:03 time_for_children -> 'time:[4026531834]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 user -> 'user:[4026531837]'
    lrwxrwxrwx. 1 root root 0 Feb 27 09:40 uts -> 'uts:[4026531838]'
    [it4@efrei-xmg4agau1 ~]$ sudo lsns
            NS TYPE   NPROCS   PID USER   COMMAND
    4026531834 time      108     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531835 cgroup    107     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531836 pid       107     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531837 user      108     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531838 uts       104     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531839 ipc       107     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531840 net       107     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531841 mnt        98     1 root   /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    4026531862 mnt         1    23 root   kdevtmpfs
    4026532120 mnt         1   572 root   /usr/lib/systemd/systemd-udevd
    4026532121 uts         1   572 root   /usr/lib/systemd/systemd-udevd
    4026532157 mnt         1   635 root   /sbin/auditd
    4026532158 mnt         2   666 dbus   /usr/bin/dbus-broker-launch --scope system --audit
    4026532159 mnt         1   673 chrony /usr/sbin/chronyd -F 2
    4026532160 mnt         1   671 root   /usr/lib/systemd/systemd-logind
    4026532161 uts         1   671 root   /usr/lib/systemd/systemd-logind
    4026532162 uts         1   673 chrony /usr/sbin/chronyd -F 2
    4026532163 mnt         1   676 root   /usr/sbin/NetworkManager --no-daemon
    4026532166 mnt         1  6003 root   sleep 9999
    4026532167 uts         1  6003 root   sleep 9999
    4026532168 ipc         1  6003 root   sleep 9999
    4026532169 pid         1  6003 root   sleep 9999
    4026532170 cgroup      1  6003 root   sleep 9999
    4026532171 net         1  6003 root   sleep 9999
    4026532250 mnt         1   795 root   /usr/sbin/rsyslogd -n
```

- un CGroup a été attribué à ce conteneur
```bash
    [it4@efrei-xmg4agau1 ~]$ docker inspect dee1191c3679 | grep -i cgroup
                "CgroupnsMode": "private",
                "Cgroup": "",
                "CgroupParent": "",
                "DeviceCgroupRules": null,
```

```bash
    [it4@efrei-xmg4agau1 ~]$ docker exec -it dee bash
    root@dee1191c3679:/# apt update -y
    Hit:1 http://deb.debian.org/debian bookworm InRelease
    Hit:2 http://deb.debian.org/debian bookworm-updates InRelease
    Hit:3 http://deb.debian.org/debian-security bookworm-security InRelease
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    All packages are up to date.
    root@dee1191c3679:/# apt install -y procps iproute2 iputils-ping
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    procps is already the newest version (2:4.0.2-3).
    The following additional packages will be installed:
      krb5-locales libatm1 libbpf1 libbsd0 libcap2-bin libelf1 libgssapi-krb5-2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libmnl0 libpam-cap libssl3 libtirpc-common libtirpc3 libxtables12
    Suggested packages:
      iproute2-doc python3:any krb5-doc krb5-user
    The following NEW packages will be installed:
      iproute2 iputils-ping krb5-locales libatm1 libbpf1 libbsd0 libcap2-bin libelf1 libgssapi-krb5-2 libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 libmnl0 libpam-cap libssl3 libtirpc-common libtirpc3 libxtables12
    0 upgraded, 19 newly installed, 0 to remove and 0 not upgraded.
    Need to get 4464 kB of archives.
    After this operation, 14.5 MB of additional disk space will be used.
    Get:1 http://deb.debian.org/debian bookworm/main amd64 libelf1 amd64 0.188-2.1 [174 kB]
    Get:2 http://deb.debian.org/debian bookworm/main amd64 libbpf1 amd64 1:1.1.0-1 [145 kB]
    ...
    debconf: falling back to frontend: Readline
    debconf: unable to initialize frontend: Readline
    debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.36.0 /usr/local/share/perl/5.36.0 /usr/lib/x86_64-linux-gnu/perl5/5.36 /usr/share/perl5 /
    usr/lib/x86_64-linux-gnu/perl-base /usr/lib/x86_64-linux-gnu/perl/5.36 /usr/share/perl/5.36 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
    debconf: falling back to frontend: Teletype
    Processing triggers for libc-bin (2.36-9+deb12u9) ...
    root@dee1191c3679:/# ps aux
    USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    root           1  0.0  0.0   2484  1280 ?        Ss   11:41   0:00 sleep 9999
    root         204  0.0  0.1   4188  3328 pts/0    Ss   12:13   0:00 bash
    root         559  0.0  0.2   8088  4224 pts/0    R+   12:16   0:00 ps aux
```

#### C. CGroup

**🌞 Lancer un conteneur restreint**

```bash
    [it4@efrei-xmg4agau1 ~]$ docker run -d --memory=456m debian sleep 9999
    98cc7294e8d0....
    [it4@efrei-xmg4agau1 ~]$ docker ps

    [it4@efrei-xmg4agau1 ~]$ docker stats
    CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS     NAMES
    98cc7294e8d0   debian    "sleep 9999"   2 minutes ago    Up 2 minutes              clever_greider
    dee1191c3679   debian    "sleep 9999"   38 minutes ago   Up 38 minutes             lucid_maxwell
    CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O         PIDS 
    98cc7294e8d0   clever_greider   0.00%     340KiB / 456MiB       0.07%     656B / 126B     0B / 0B           1 
    dee1191c3679   lucid_maxwell    0.00%     3.941MiB / 1.733GiB   0.22%     15MB / 47.4kB   1.55MB / 49.4MB   1 
```

**🌞 CGroup ?**

```bash
    root@98cc7294e8d0:/# cd /sys/fs/cgroup/       
    root@98cc7294e8d0:/sys/fs/cgroup# echo $(( 456 * 1024 * 1024 ))
    478150656
    root@98cc7294e8d0:/sys/fs/cgroup# grep -nri 478150656
    memory.swap.max:1:478150656
    grep: cgroup.kill: Invalid argument
    memory.max:1:478150656

    [it4@efrei-xmg4agau1 ~]$ cd /sys/fs/cgroup/
    [it4@efrei-xmg4agau1 cgroup]$ grep -nri 478150656 2> /dev/null
    system.slice/docker-98cc7294e8d029156b2478b4a517914adfddc26637f751f0d0fc3189384cc825.scope/memory.swap.max:1:478150656
    system.slice/docker-98cc7294e8d029156b2478b4a517914adfddc26637f751f0d0fc3189384cc825.scope/memory.max:1:478150656
```

## Part V : Harden me baby

**🌞 Proposez un nouveau fichier web.service**

```bash
    [it4@localhost ~]$ sudo nano /etc/systemd/system/web.service
    [sudo] password for it4: 
    [it4@localhost ~]$ sudo cat /etc/systemd/system/web.service
    [Unit]
    Description=Python Web Server
    After=network.target

    [Service]
    ExecStart=/usr/bin/python3 -m http.server 9999
    WorkingDirectory=/var/www/html
    Restart=always
    User=it4

    # Restrictions de ressources 
    MemoryMax=321M
    IOWriteBandwidthMax=/dev/sda 1M
    IOReadBandwidthMax=/dev/sda 1M
    CPUQuota=50%

    # Mesures de sécurité supplémentaires
    PrivateTmp=yes
    PrivateDevices=yes
    ProtectSystem=strict
    ProtectHome=yes
    NoNewPrivileges=yes
    ProtectKernelTunables=yes
    ProtectKernelModules=yes
    ProtectControlGroups=yes
    RestrictNamespaces=yes
    LockPersonality=yes
    MemoryDenyWriteExecute=yes
    RestrictRealtime=yes
    CapabilityBoundingSet=CAP_NET_BIND_SERVICE
    AmbientCapabilities=CAP_NET_BIND_SERVICE
    SecureBits=no-setuid-fixup-locked
    SystemCallFilter=@system-service
    SystemCallErrorNumber=EPERM

    [Install]
    WantedBy=multi-user.target
    
    [it4@localhost ~]$ sudo systemctl daemon-reload
    [it4@localhost ~]$ sudo systemctl restart web.service
    [it4@localhost ~]$ sudo systemctl status web.service
    ● web.service - Python Web Server
         Loaded: loaded (/etc/systemd/system/web.service; enabled; preset: disabled)
         Active: active (running) since Sun 2025-03-02 15:33:54 CET; 4s ago
       Main PID: 1620 (python3)
          Tasks: 1 (limit: 11097)
         Memory: 9.1M (max: 321.0M available: 311.8M)
            CPU: 99ms
         CGroup: /system.slice/web.service
                 └─1620 /usr/bin/python3 -m http.server 9999

```

- On passe donc de  "→ Overall exposure level for web.service: 9.2 UNSAFE 😨" à:
```bash
    [it4@localhost ~]$ systemd-analyze security web.service
      NAME                                                        DESCRIPTION                                                                                         EXPOSURE
    ✓ SystemCallFilter=~@swap                                     System call allow list defined for service, and @swap is not included                                       
    ✗ SystemCallFilter=~@resources                                System call allow list defined for service, and @resources is included (e.g. ioprio_set is allowed)      0.2
    ✓ SystemCallFilter=~@reboot                                   System call allow list defined for service, and @reboot is not included                                     
    ✓ SystemCallFilter=~@raw-io                                   System call allow list defined for service, and @raw-io is not included                                     
    ✗ SystemCallFilter=~@privileged                               System call allow list defined for service, and @privileged is included (e.g. chown is allowed)          0.2
    ✓ SystemCallFilter=~@obsolete                                 System call allow list defined for service, and @obsolete is not included                                   
    ✓ SystemCallFilter=~@mount                                    System call allow list defined for service, and @mount is not included                                      
    ✓ SystemCallFilter=~@module                                   System call allow list defined for service, and @module is not included                                     
    ✓ SystemCallFilter=~@debug                                    System call allow list defined for service, and @debug is not included                                      
    ✓ SystemCallFilter=~@cpu-emulation                            System call allow list defined for service, and @cpu-emulation is not included                              
    ✓ SystemCallFilter=~@clock                                    System call allow list defined for service, and @clock is not included                                      
    ✗ RemoveIPC=                                                  Service user may leave SysV IPC objects around                                                           0.1
    ✗ RootDirectory=/RootImage=                                   Service runs within the host's root directory                                                            0.1
    ✓ User=/DynamicUser=                                          Service runs under a static non-root user identity                                                          
    ✓ RestrictRealtime=                                           Service realtime scheduling access is restricted                                                            
    ✓ CapabilityBoundingSet=~CAP_SYS_TIME                         Service processes cannot change the system clock                                                            
    ✓ NoNewPrivileges=                                            Service processes cannot acquire new privileges                                                             
    ✗ AmbientCapabilities=                                        Service process receives ambient capabilities                                                            0.1
    ✗ ProtectClock=                                               Service may write to the hardware clock or system clock                                                  0.2
    ✗ ProtectKernelLogs=                                          Service may read from or write to the kernel log ring buffer                                             0.2
    ✗ SystemCallArchitectures=                                    Service may execute system calls with all ABIs                                                           0.2
    ✗ RestrictSUIDSGID=                                           Service may create SUID/SGID files                                                                       0.2
    ✗ ProtectHostname=                                            Service may change system host/domainname                                                                0.1
    ✗ RestrictAddressFamilies=~AF_PACKET                          Service may allocate packet sockets                                                                      0.2
    ✗ RestrictAddressFamilies=~AF_NETLINK                         Service may allocate netlink sockets                                                                     0.1
    ✗ RestrictAddressFamilies=~AF_UNIX                            Service may allocate local sockets                                                                       0.1
    ✗ RestrictAddressFamilies=~…                                  Service may allocate exotic sockets                                                                      0.3
    ✗ RestrictAddressFamilies=~AF_(INET|INET6)                    Service may allocate Internet sockets                                                                    0.3
    ✓ ProtectSystem=                                              Service has strict read-only access to the OS file hierarchy                                                
    ✓ SupplementaryGroups=                                        Service has no supplementary groups                                                                         
    ✓ CapabilityBoundingSet=~CAP_SYS_RAWIO                        Service has no raw I/O access                                                                               
    ✓ CapabilityBoundingSet=~CAP_SYS_PTRACE                       Service has no ptrace() debugging abilities                                                                 
    ✓ CapabilityBoundingSet=~CAP_SYS_(NICE|RESOURCE)              Service has no privileges to change resource use parameters                                                 
    ✓ CapabilityBoundingSet=~CAP_NET_ADMIN                        Service has no network configuration privileges                                                             
    ✓ CapabilityBoundingSet=~CAP_AUDIT_*                          Service has no audit subsystem access                                                                       
    ✓ CapabilityBoundingSet=~CAP_SYS_ADMIN                        Service has no administrator privileges                                                                     
    ✓ PrivateTmp=                                                 Service has no access to other software's temporary files                                                   
    ✓ CapabilityBoundingSet=~CAP_SYSLOG                           Service has no access to kernel logging                                                                     
    ✓ ProtectHome=                                                Service has no access to home directories                                                                   
    ✓ PrivateDevices=                                             Service has no access to hardware devices                                                                   
    ✗ ProtectProc=                                                Service has full access to process tree (/proc hidepid=)                                                 0.2
    ✗ ProcSubset=                                                 Service has full access to non-process /proc files (/proc subset=)                                       0.1
    ✗ CapabilityBoundingSet=~CAP_NET_(BIND_SERVICE|BROADCAST|RAW) Service has elevated networking privileges                                                               0.1
    ✗ PrivateNetwork=                                             Service has access to the host's network                                                                 0.5
    ✗ PrivateUsers=                                               Service has access to other users                                                                        0.2
    ✓ DeviceAllow=                                                Service has a minimal device ACL                                                                            
    ✓ KeyringMode=                                                Service doesn't share key material with other services                                                      
    ✓ Delegate=                                                   Service does not maintain its own delegated control group subtree                                           
    ✗ IPAddressDeny=                                              Service does not define an IP address allow list                                                         0.2
    ✓ NotifyAccess=                                               Service child processes cannot alter service state                                                          
    ✓ CapabilityBoundingSet=~CAP_SYS_PACCT                        Service cannot use acct()                                                                                   
    ✓ CapabilityBoundingSet=~CAP_KILL                             Service cannot send UNIX signals to arbitrary processes                                                     
    ✓ CapabilityBoundingSet=~CAP_WAKE_ALARM                       Service cannot program timers that wake up the system                                                       
    ✓ CapabilityBoundingSet=~CAP_(DAC_*|FOWNER|IPC_OWNER)         Service cannot override UNIX file/IPC permission checks                                                     
    ✓ ProtectControlGroups=                                       Service cannot modify the control group file system                                                         
    ✓ CapabilityBoundingSet=~CAP_LINUX_IMMUTABLE                  Service cannot mark files immutable                                                                         
    ✓ CapabilityBoundingSet=~CAP_IPC_LOCK                         Service cannot lock memory into RAM                                                                         
    ✓ ProtectKernelModules=                                       Service cannot load or read kernel modules                                                                  
    ✓ CapabilityBoundingSet=~CAP_SYS_MODULE                       Service cannot load kernel modules                                                                          
    ✓ CapabilityBoundingSet=~CAP_SYS_TTY_CONFIG                   Service cannot issue vhangup()                                                                              
    ✓ CapabilityBoundingSet=~CAP_SYS_BOOT                         Service cannot issue reboot()                                                                               
    ✓ CapabilityBoundingSet=~CAP_SYS_CHROOT                       Service cannot issue chroot()                                                                               
    ✓ PrivateMounts=                                              Service cannot install system mounts                                                                        
    ✓ CapabilityBoundingSet=~CAP_BLOCK_SUSPEND                    Service cannot establish wake locks                                                                         
    ✓ MemoryDenyWriteExecute=                                     Service cannot create writable executable memory mappings                                                   
    ✓ RestrictNamespaces=~user                                    Service cannot create user namespaces                                                                       
    ✓ RestrictNamespaces=~pid                                     Service cannot create process namespaces                                                                    
    ✓ RestrictNamespaces=~net                                     Service cannot create network namespaces                                                                    
    ✓ RestrictNamespaces=~uts                                     Service cannot create hostname namespaces                                                                   
    ✓ RestrictNamespaces=~mnt                                     Service cannot create file system namespaces                                                                
    ✓ CapabilityBoundingSet=~CAP_LEASE                            Service cannot create file leases                                                                           
    ✓ CapabilityBoundingSet=~CAP_MKNOD                            Service cannot create device nodes                                                                          
    ✓ RestrictNamespaces=~cgroup                                  Service cannot create cgroup namespaces                                                                     
    ✓ RestrictNamespaces=~ipc                                     Service cannot create IPC namespaces                                                                        
    ✓ CapabilityBoundingSet=~CAP_(CHOWN|FSETID|SETFCAP)           Service cannot change file ownership/access mode/capabilities                                               
    ✓ CapabilityBoundingSet=~CAP_SET(UID|GID|PCAP)                Service cannot change UID/GID identities/capabilities                                                       
    ✓ LockPersonality=                                            Service cannot change ABI personality                                                                       
    ✓ ProtectKernelTunables=                                      Service cannot alter kernel tunables (/proc/sys, …)                                                         
    ✓ CapabilityBoundingSet=~CAP_MAC_*                            Service cannot adjust SMACK MAC                                                                             
    ✗ UMask=                                                      Files created by service are world-readable by default                                                   0.1

    → Overall exposure level for web.service: 2.9 OK 🙂
```


