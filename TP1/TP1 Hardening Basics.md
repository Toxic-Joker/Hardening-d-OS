# TP1: Hardening Basics

## Part I: User Management

### 1. Existing Users

🌞 **Déterminer l'existant** :

- Lister tous les utilisateurs créés sur la machine:
    ```bash
    [white-ghost@localhost ~]$ cat /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    operator:x:11:0:operator:/root:/sbin/nologin
    games:x:12:100:games:/usr/games:/sbin/nologin
    ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
    systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
    dbus:x:81:81:System message bus:/:/sbin/nologin
    tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
    sssd:x:998:996:User for sssd:/:/sbin/nologin
    sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
    chrony:x:997:995:chrony system user:/var/lib/chrony:/sbin/nologin
    it4:x:1000:1000:it4:/home/it4:/bin/bash
    tcpdump:x:72:72::/:/sbin/nologin
    white-ghost:x:1001:1001::/home/white-ghost:/bin/bash
    ```

- Lister tous les groupes d'utilisateur:
    ```bash
    [white-ghost@localhost ~]$ cat /etc/group
    root:x:0:
    bin:x:1:
    daemon:x:2:
    sys:x:3:
    adm:x:4:
    tty:x:5:
    disk:x:6:
    lp:x:7:
    mem:x:8:
    kmem:x:9:
    wheel:x:10:it4
    cdrom:x:11:
    mail:x:12:
    man:x:15:
    dialout:x:18:
    floppy:x:19:
    games:x:20:
    tape:x:33:
    video:x:39:
    ftp:x:50:
    lock:x:54:
    audio:x:63:
    users:x:100:
    nobody:x:65534:
    utmp:x:22:
    utempter:x:35:
    input:x:999:
    kvm:x:36:
    render:x:998:
    systemd-journal:x:190:
    systemd-coredump:x:997:
    dbus:x:81:
    ssh_keys:x:101:
    tss:x:59:
    sssd:x:996:
    sshd:x:74:
    chrony:x:995:
    sgx:x:994:
    it4:x:1000:
    tcpdump:x:72:
    white-ghost:x:1001:
    ```

- Déterminer la liste des groupes dans lesquels se trouve votre utilisateur:
    ```bash
    [white-ghost@localhost ~]$ grep "$(whoami)" /etc/group
    white-ghost:x:1001:
    ```

🌞 **Lister tous les processus qui sont actuellement en cours d'exécution, lancés par root**:
    ```bash
    
    [white-ghost@localhost ~]$ ps aux | grep ^root
    root           1  0.0  0.8 106640 15856 ?        Ss   10:06   0:00 /usr/lib/systemd/systemd --switched-root --system --deserialize 31
    root           2  0.0  0.0      0     0 ?        S    10:06   0:00 [kthreadd]
    ...
    ```

🌞 **Lister tous les processus qui sont actuellement en cours d'exécution, lancés par votre utilisateur**:
  ```bash
  [white-ghost@localhost ~]$ ps -u $whoami
  USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
  white-g+    1225  0.0  0.2   7444  4096 tty1     Ss+  10:07   0:00 -bash
  white-g+    1272  0.0  0.3  16224  7040 pts/0    Ss   10:09   0:00 -bash
  white-g+    1511  0.0  0.1  10140  3328 pts/0    R+   10:33   0:00 ps -u
   ```

🌞 **Déterminer le hash du mot de passe de root**:
  ```bash
  [white-ghost@localhost ~]$ sudo cat /etc/shadow | grep ^root
  root:$6$.8fzl//9C0M819BS$Sw1mrG49Md8cyNUn0Ai0vlthhzuSZpJ/XVfersVmgXDSBrTVchneIWHYHnT3mC/NutmPS03TneWAHihO0NXrj1::0:99999:7:::
  ```

🌞 **Déterminer le hash du mot de passe de votre utilisateur**:
  ```bash
  [white-ghost@localhost ~]$ sudo cat /etc/shadow | grep "$(whoami)"
  white-ghost:$6$AmH1xSeYxclVgy/5$FqNfA1LikoG0.cyScFgODXpm76pyWaLc2UO63sg4BHg1o3vBBPu7oX7JNDIB63ZD6ljKoHzjYC5FUY7cV2pp/0:20122:0:99999:7:::
  ```

🌞 **Déterminer la fonction de hachage qui a été utilisée**:
- Les haches commencant par `$6$` la fonction utilisée est donc **SHA-512**.

🌞 **Déterminer, pour l'utilisateur root**:
  ```bash
  [white-ghost@localhost ~]$ cat /etc/passwd | grep ^root
  root:x:0:0:root:/root:/bin/bash
```

    - Son shell par défaut: `"/bin/bash"`
    - Le chemin vers son répertoire personnel: `"/root"`

🌞 **Déterminer, pour votre utilisateur**:
  ```bash  
  [white-ghost@localhost ~]$ cat /etc/passwd | grep "$(whoami)"
  white-ghost:x:1001:1001::/home/white-ghost:/bin/bash
  ```

    - Son shell par défaut: `"/bin/bash"`
    - Le chemin vers son répertoire personnel: `"/home/white-ghost"`

🌞 **Afficher la ligne de configuration du fichier sudoers qui permet à votre utilisateur d'utiliser sudo**:
  ```bash  
  [white-ghost@localhost ~]$ sudo visudo
  ## Allows people in group wheel to run all commands
  %wheel  ALL=(ALL)       ALL
  visudo: /etc/sudoers.tmp unchanged
  ```

### 2. User Creation and Configuration

🌞 **Créer un utilisateur**:
  ```bash
  [white-ghost@localhost ~]$ sudo groupadd admins
  [white-ghost@localhost ~]$ sudo useradd -g admins -M -s /sbin/nologin meow
  [white-ghost@localhost ~]$ sudo passwd meow
  Changing password for user meow.
  New password: 
  BAD PASSWORD: The password is shorter than 8 characters
  Retype new password: 
  passwd: all authentication tokens updated successfully.
  [white-ghost@localhost ~]$ id meow
  uid=1002(meow) gid=1002(admins) groups=1002(admins)
  ```

🌞 **Configuration sudoers**:
  ```bash 
  [white-ghost@localhost ~]$ sudo visudo
  # Permettre à meow d'exécuter ls, cat, less et more en tant que white-ghost
  meow ALL=(white-ghost) /bin/ls, /bin/cat, /usr/bin/less, /bin/more
  # Permettre au groupe admins d'exécuter apt en tant que root
  %admins ALL=(ALL) /usr/bin/apt
  # Permettre à votre utilisateur d'exécuter n'importe quelle commande en tant que root sans mot de passe
  white-ghost ALL=(ALL:ALL) NOPASSWD: ALL
  ```
  Preuve que l'utilisateur meow puisse exécuter seulement et uniquement les commandes ls, cat, less et more en tant que votre utilisateur
  ```bash
    [white-ghost@localhost ~]$ sudo su -s /bin/bash - meow 
    Last login: Mon Feb  3 11:33:50 CET 2025 on pts/0
    su: warning: cannot change directory to /home/meow: No such file or directory
    [meow@localhost white-ghost]$ sudo -u white-ghost ls
    We trust you have received the usual lecture from the local System
    Administrator. It usually boils down to these three things:
        #1) Respect the privacy of others.
        #2) Think before you type.
        #3) With great power comes great responsibility.
    [sudo] password for meow: 
    [meow@localhost white-ghost]$ sudo -u white-ghost ls /home
    it4  white-ghost
  ```
    
### 3. Hackers gonna hack

🌞 **Déjà une configuration faible ?**:
l'utilisateur meow est en réalité complètement root sur la machine hein là. Prouvez-le.
  ```bash 
  [meow@localhost white-ghost]$ sudo -u white-ghost less /etc/passwd
  [sudo] password for meow: 
  sh-5.1$ whoami
  white-ghost
  sh-5.1$ sudo ls
  sh-5.1$ sudo -l
  Matching Defaults entries for white-ghost on localhost:
      !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR
      LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT
      LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET
      XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin
  User white-ghost may run the following commands on localhost:
      (ALL) ALL
      (ALL : ALL) NOPASSWD: ALL
  sh-5.1$ sudo su - root
  Last login: Mon Feb  3 10:37:20 CET 2025 on pts/0
  ```
  proposez une configuration similaire, sans présenter cette faiblesse de configuration
  ```bash
  [white-ghost@localhost ~]$ sudo visudo
  # Permettre à meow d'exécuter ls, cat, less et more en tant que white-ghost
  meow ALL=(white-ghost) /bin/ls, /bin/cat
  ```

## Part II : Files and permissions

### 1. Listing POSIX permissions

🌞 **Déterminer les permissions des fichiers/dossiers**:
```bash
[white-ghost@localhost ~]$ ls -l /etc/passwd
-rw-r--r--. 1 root root 1061 Feb  3 11:15 /etc/passwd
[white-ghost@localhost ~]$ ls -l /etc/shadow
----------. 1 root root 996 Feb  3 11:37 /etc/shadow
[white-ghost@localhost ~]$ ls -l /etc/ssh/sshd_config
-rw-------. 1 root root 3667 Apr 18  2024 /etc/ssh/sshd_config
[white-ghost@localhost ~]$ ls -l /root
ls: cannot open directory '/root': Permission denied
[white-ghost@localhost ~]$ sudo ls -l /root
total 4
-rw-------. 1 root root 983 Oct 14 10:03 anaconda-ks.cfg
[white-ghost@localhost ~]$ ls -l /home/white-ghost
total 0
[white-ghost@localhost ~]$ ls -l /usr/bin/ls
-rwxr-xr-x. 1 root root 140872 Apr 20  2024 /usr/bin/ls
[white-ghost@localhost ~]$ ls -l /usr/bin/systemctl 
-rwxr-xr-x. 1 root root 305680 Apr  8  2024 /usr/bin/systemctl 
```

### 2. Extended attributes

🌞 **Lister tous les programmes qui ont le bit SUID activé**
```bash
	[white-ghost@localhost ~]$ find / -type f -perm -4000 -exec ls -l {} \; 2>/dev/null
	-rwsr-xr-x. 1 root root 73704 Oct 30  2023 /usr/bin/chage
	-rwsr-xr-x. 1 root root 78024 Oct 30  2023 /usr/bin/gpasswd
	-rwsr-xr-x. 1 root root 41744 Oct 30  2023 /usr/bin/newgrp
	-rwsr-xr-x. 1 root root 48600 Apr 20  2024 /usr/bin/mount
	-rwsr-xr-x. 1 root root 36224 Apr 20  2024 /usr/bin/umount
	-rwsr-xr-x. 1 root root 57056 Apr 20  2024 /usr/bin/su
	-rwsr-xr-x. 1 root root 57240 Apr 16  2024 /usr/bin/crontab
	-rwsr-xr-x. 1 root root 32656 May 15  2022 /usr/bin/passwd
	---s--x--x. 1 root root 185304 Feb 14  2024 /usr/bin/sudo
	-rwsr-xr-x. 1 root root 23952 Apr 16  2024 /usr/sbin/unix_chkpwd
	-rwsr-xr-x. 1 root root 15608 Apr 16  2024 /usr/sbin/pam_timestamp_check
	-rwsr-xr-x. 1 root root 15440 May  1  2024 /usr/sbin/grub2-set-bootflag
```
### 3. Protect a file using permissions

🌞 **Restreindre l'accès à un fichier personnel**
```bash
	[white-ghost@localhost ~]$ mkdir /tmp/shared_folder
	[white-ghost@localhost ~]$ chmod 777 /tmp/shared_folder
	[white-ghost@localhost ~]$ echo "Contenu Secret" > /tmp/shared_folder/dont_readme.txt
	[white-ghost@localhost ~]$ chmod 600 /tmp/shared_folder/dont_readme.txt
	[white-ghost@localhost ~]$ ls -l /tmp/shared_folder/dont_readme.txt
	-rw-------. 1 white-ghost white-ghost 15 Feb  3 15:16 /tmp/shared_folder/dont_readme.txt
	[white-ghost@localhost ~]$ cat /tmp/shared_folder/dont_readme.txt
	Contenu Secret
	[white-ghost@localhost ~]$ echo "Nouveau contenu" >> /tmp/shared_folder/dont_readme.txt
	[white-ghost@localhost ~]$ cat /tmp/shared_folder/dont_readme.txt
	Contenu Secret
	Nouveau contenu
	[white-ghost@localhost ~]$ sudo -u meow cat /tmp/shared_folder/dont_readme.txt
	cat: /tmp/shared_folder/dont_readme.txt: Permission denied
	[white-ghost@localhost ~]$ sudo -u root cat /tmp/shared_folder/dont_readme.txt
	Contenu Secret
	Nouveau contenu
```

## Part III : Networking

### 1. Listening ports

🌞 **Déterminer la liste des programmes qui écoutent sur port TCP**
```bash
	[white-ghost@localhost ~]$ ss -tulnp | grep LISTEN
	tcp   LISTEN 0      128          0.0.0.0:22        0.0.0.0:*          
	tcp   LISTEN 0      128             [::]:22           [::]:*   
```

🌞 **Déterminer la liste des programmes qui écoutent sur port UDP**
```bash
	[white-ghost@localhost ~]$ ss -ulnp
	State            Recv-Q           Send-Q                     Local Address:Port                     Peer Address:Port          Process          
	UNCONN           0                0                              127.0.0.1:323                           0.0.0.0:*                              
	UNCONN           0                0                                  [::1]:323                              [::]:*                              
```

### 2. Firewalling

🌞 **Pour chacun des ports précédemment repérés...**
```bash
	[white-ghost@localhost ~]$ sudo firewall-cmd --list-all
	public (active)
	  target: default
	  icmp-block-inversion: no
	  interfaces: enp0s8
	  sources: 
	  services: cockpit dhcpv6-client ssh
	  ports: 
	  protocols: 
	  forward: yes
	  masquerade: no
	  forward-ports: 
	  source-ports: 
	  icmp-blocks: 
	  rich rules: 
```

🌞 **Fermez tous les ports inutilement ouverts dans le firewall**
```bash
	[white-ghost@localhost ~]$ sudo firewall-cmd --remove-service cockpit --permanent
	success
	[white-ghost@localhost ~]$ sudo firewall-cmd --remove-service dhcpv6-client --permanent
	success
```

🌞 **Pour toutes les applications qui sont en écoute sur TOUTES les adresses IP**
```bash
    [white-ghost@localhost ~]$ sudo nano /etc/ssh/sshd_config

	ListenAddress 192.168.56.116

	[white-ghost@localhost ~]$ sudo systemctl restart sshd
	[white-ghost@localhost ~]$ sudo ss -tulnp | grep sshd
	tcp   LISTEN 0      128    192.168.56.116:22        0.0.0.0:*    users:(("sshd",pid=4683,fd=3))  

```

## Part IV : Storage and partitions

### 1. Existing partitions

🌞 **Déterminer la liste des partitions du système**
```bash
	[white-ghost@localhost ~]$ mount
	proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
	sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime,seclabel)
	devtmpfs on /dev type devtmpfs (rw,nosuid,seclabel,size=4096k,nr_inodes=222011,mode=755,inode64)
	securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
	tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,seclabel,inode64)
	devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,seclabel,gid=5,mode=620,ptmxmode=000)
	tmpfs on /run type tmpfs (rw,nosuid,nodev,seclabel,size=363672k,nr_inodes=819200,mode=755,inode64)
	cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,seclabel,nsdelegate,memory_recursiveprot)
	pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime,seclabel)
	bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
	/dev/mapper/rl-root on / type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
	selinuxfs on /sys/fs/selinux type selinuxfs (rw,nosuid,noexec,relatime)
	systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=29,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=18384)
	debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime,seclabel)
	tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime,seclabel)
	mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime,seclabel)
	hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,seclabel,pagesize=2M)
	none on /run/credentials/systemd-sysctl.service type ramfs (ro,nosuid,nodev,noexec,relatime,seclabel,mode=700)
	fusectl on /sys/fs/fuse/connections type fusectl (rw,nosuid,nodev,noexec,relatime)
	configfs on /sys/kernel/config type configfs (rw,nosuid,nodev,noexec,relatime)
	none on /run/credentials/systemd-tmpfiles-setup-dev.service type ramfs (ro,nosuid,nodev,noexec,relatime,seclabel,mode=700)
	/dev/sda1 on /boot type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
	none on /run/credentials/systemd-tmpfiles-setup.service type ramfs (ro,nosuid,nodev,noexec,relatime,seclabel,mode=700)
	tmpfs on /run/user/1001 type tmpfs (rw,nosuid,nodev,relatime,seclabel,size=181832k,nr_inodes=45458,mode=700,uid=1001,gid=1001,inode64)
	binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,nosuid,nodev,noexec,relatime)
```

🌞 **Identifier la partition qui est montée sur /**
```bash
	[white-ghost@localhost ~]$ lsblk -o NAME,MOUNTPOINT | grep ' /$'
	  ├─rl-root /
```


### 2. Mount options

🌞 Déterminer les options de montage de la partition /
```bash
	[white-ghost@localhost ~]$ mount | grep ' / '
	/dev/mapper/rl-root on / type xfs (rw,relatime,seclabel,attr2,inode64,logbufs=8,logbsize=32k,noquota)
```
Nous aurons donc
```bash
	    rw : Lecture et écriture autorisées.
	    relatime : Met à jour l'horodatage d'accès aux fichiers uniquement si nécessaire, améliorant les performances.
	    seclabel : Active le support des étiquettes de sécurité SELinux.
	    attr2 : Utilise la version 2 des attributs étendus, offrant de meilleures performances.
	    inode64 : Permet l'utilisation d'inodes 64 bits, supportant de très grands systèmes de fichiers.
	    logbufs=8 : Définit le nombre de tampons de journalisation à 8.
	    logbsize=32k : Définit la taille des tampons de journalisation à 32 Ko.
	    noquota : Désactive le système de quotas sur cette partition.
```

🌞 Monter une partition de type tmpfs sur le dossier /tmp
```bash
	[white-ghost@localhost ~]$ sudo mount -t tmpfs -o rw,nosuid,noexec tmpfs /tmp
	[white-ghost@localhost ~]$ mount | grep '/tmp'
	tmpfs on /tmp type tmpfs (rw,nosuid,noexec,relatime,seclabel,inode64)
	tmpfs on /tmp type tmpfs (rw,nosuid,noexec,relatime,seclabel,inode64)
	[white-ghost@localhost ~]$ cp /bin/ls /tmp/
	[white-ghost@localhost ~]$ chmod 777 /tmp/ls
	[white-ghost@localhost ~]$ /tmp/ls
	-bash: /tmp/ls: Permission denied
	[white-ghost@localhost ~]$ echo "Test fichier temporaire" > /tmp/test.txt
	[white-ghost@localhost ~]$ cat /tmp/test.txt
	Test fichier temporaire
```

# Part V : OpenSSH Server


## 1. Basics

**🌞 Afficher l'identifiant du processus serveur OpenSSH en cours d'exécution**

```bash
	[white-ghost@localhost ~]$ ps aux | grep sshd | grep -v grep
	root        1286  0.0  0.4  15860  9088 ?        Ss   22:52   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```

**🌞 Changer le port d'écoute du serveur OpenSSH**

```bash
	[white-ghost@localhost ~]$ sudo nano /etc/ssh/sshd_config

	Port 19

	[white-ghost@localhost ~]$ sudo systemctl restart sshd
	[white-ghost@localhost ~]$ sudo ss -tulnp | grep sshd
	tcp   LISTEN 0      128    192.168.56.116:19        0.0.0.0:*    users:(("sshd",pid=1579,fd=3))  

	[white-ghost@localhost ~]$ sudo firewall-cmd --permanent --add-port=19/tcp
	success
	[white-ghost@localhost ~]$ sudo firewall-cmd --reload
	success

	white-ghost@white-ghost-AORUS-16X-ASG:~$ ssh -p 19 white-ghost@192.168.56.116
	white-ghost@192.168.56.116's password: 
	Last login: Wed Feb  5 22:53:39 2025 from 192.168.56.1
	[white-ghost@localhost ~]$ 
```

## 2. Authentication modes

### A. Key-based authentication

**🌞 Configurer une authentification par clé**

```bash
	white-ghost@white-ghost-AORUS-16X-ASG:~$ ssh-keygen -t ed25519
	Generating public/private ed25519 key pair.
	Enter file in which to save the key (/home/white-ghost/.ssh/id_ed25519): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/white-ghost/.ssh/id_ed25519
	Your public key has been saved in /home/white-ghost/.ssh/id_ed25519.pub
	The key fingerprint is:
	SHA256:Gkkx/p5kcad4EYe3kqAa+wze02tuHVzNogP8Jq/kvKM white-ghost@white-ghost-AORUS-16X-ASG
	The key's randomart image is:
	+--[ED25519 256]--+
	|      o   ...    |
	|     . o. .o.    |
	|      oo..oo.+   |
	|    ...oo+o++ o  |
	|     +o S+o+ .   |
	|    +  *.oB      |
	|   . =..+= o     |
	|    . =+= o      |
	|      E*B=       |
	+----[SHA256]-----+
	white-ghost@white-ghost-AORUS-16X-ASG:~$ ssh-copy-id -p 19 white-ghost@192.168.56.116
	/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
	/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
	white-ghost@192.168.56.116's password: 

	Number of key(s) added: 1

	Now try logging into the machine, with:   "ssh -p 19 'white-ghost@192.168.56.116'"
	and check to make sure that only the key(s) you wanted were added.

	[white-ghost@localhost ~]$ sudo systemctl restart sshd

	white-ghost@white-ghost-AORUS-16X-ASG:~$ ssh -p 19 white-ghost@192.168.56.116
	Last login: Wed Feb  5 23:25:37 2025 from 192.168.56.1
	[white-ghost@localhost ~]$ 
```

**🌞 Désactiver la connexion par password**

```bash
	[white-ghost@localhost ~]$ sudo nano /etc/ssh/sshd_config

	PasswordAuthentication no

	[white-ghost@localhost ~]$ sudo systemctl restart sshd
```

**🌞 Désactiver la connexion en tant que root**

```bash
	[white-ghost@localhost ~]$ sudo nano /etc/ssh/sshd_config
	
	PermitRootLogin no

	[white-ghost@localhost ~]$ sudo systemctl restart sshd
```

## 3. Cert-based authentication

**🌞 Configurer une authentification par certificat**

```bash
	[white-ghost@localhost .ssh]$ ssh-keygen -t rsa
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/white-ghost/.ssh/id_rsa): 
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/white-ghost/.ssh/id_rsa
	Your public key has been saved in /home/white-ghost/.ssh/id_rsa.pub
	The key fingerprint is:
	SHA256:kuMjsP5a2FjWywf5F0/auRfAfnkSUKVdC2svf+7fQHc white-ghost@localhost.localdomain
	The keys randomart image is:
	+---[RSA 3072]----+
	|             o..o|
	|            . ooo|
	|           . +...|
	|     . o    + o  |
	|  . o B S ...o.+E|
	|   O o *   *.oB.o|
	|  + + = o o +..=.|
	| . . . o .   ..oo|
	|  oo.       .. .*|
	+----[SHA256]-----+
	[white-ghost@localhost .ssh]$ ssh-keygen -s ca_key -I user_identity -n white-ghost -V +52w id_rsa.pub
	Signed user key id_rsa-cert.pub: id "user_identity" serial 0 for white-ghost valid from 2025-02-05T23:52:00 to 2026-02-04T23:53:52

	[white-ghost@localhost .ssh]$ sudo nano /etc/ssh/sshd_config

	TrustedUserCAKeys /home/white-ghost/.ssh

	[white-ghost@localhost .ssh]$ sudo systemctl restart sshd

	<pre><font color="#26A269"><b>white-ghost@white-ghost-AORUS-16X-ASG</b></font>:<font color="#12488B"><b>~/.ssh</b></font>$ scp -P 19 white-ghost@192.168.56.116:/home/white-ghost/.ssh/id_rsa-cert.pub ~/.ssh/
	id_rsa-cert.pub                                                                                    100% 2052   997.3KB/s   00:00    
	<font color="#26A269"><b>white-ghost@white-ghost-AORUS-16X-ASG</b></font>:<font color="#12488B"><b>~/.ssh</b></font>$ cd ..
	<font color="#26A269"><b>white-ghost@white-ghost-AORUS-16X-ASG</b></font>:<font color="#12488B"><b>~</b></font>$ ls ~/.ssh/id_rsa-cert.pub
	/home/white-ghost/.ssh/id_rsa-cert.pub
	<font color="#26A269"><b>white-ghost@white-ghost-AORUS-16X-ASG</b></font>:<font color="#12488B"><b>~</b></font>$ chmod 600 ~/.ssh/id_rsa-cert.pub
	<font color="#26A269"><b>white-ghost@white-ghost-AORUS-16X-ASG</b></font>:<font color="#12488B"><b>~</b></font>$ ssh -i ~/.ssh/id_rsa -p 19 white-ghost@192.168.56.116
	Warning: Identity file /home/white-ghost/.ssh/id_rsa not accessible: No such file or directory.
	Last login: Wed Feb  5 23:34:25 2025 from 192.168.56.1
```

## 4. Further hardening

**🌞 Proposer au moins 5 configurations supplémentaires qui permettent de renforcer la sécurité du serveur OpenSSH**

```bash
	[white-ghost@localhost .ssh]$ sudo nano /etc/ssh/sshd_config

	# 1. Utiliser un LogLevel plus détaillé
	LogLevel VERBOSE

	# 2. Limiter le nombre de tentatives d'authentification
	MaxAuthTries 3

	# 3. Désactiver le transfert X11
	X11Forwarding no

	# 4. Restreindre l'accès SSH à des utilisateurs spécifiques
	AllowUsers white-ghost

	# 5. Activer StrictModes
	StrictModes yes

	[white-ghost@localhost .ssh]$ sudo sshd -t
	[white-ghost@localhost .ssh]$ sudo systemctl restart sshd
```

## 5. fail2ban

**🌞 Installer fail2ban sur la machine**
```bash
	[white-ghost@localhost ~]$ sudo dnf install epel-release
	Last metadata expiration check: 0:05:52 ago on Thu 06 Feb 2025 12:41:19 AM CET.
	Dependencies resolved.

	Installed:
	  epel-release-9-7.el9.noarch                                                                                                        

	Complete!

	[white-ghost@localhost ~]$ sudo dnf install fail2ban
	Extra Packages for Enterprise Linux 9 - x86_64                                                       3.8 MB/s |  23 MB     00:06    
	Extra Packages for Enterprise Linux 9 openh264 (From Cisco) - x86_64                                 3.3 kB/s | 2.5 kB     00:00    
	Dependencies resolved.
	=====================================================================================================================================
	 Package                                      Architecture           Version                         Repository                 Size
	=====================================================================================================================================
	Installing:
	 fail2ban       
	 
	Installed:
	  checkpolicy-3.6-1.el9.x86_64                     esmtp-1.2-19.el9.x86_64                     fail2ban-1.0.2-12.el9.noarch          
	  fail2ban-firewalld-1.0.2-12.el9.noarch           fail2ban-selinux-1.0.2-12.el9.noarch        fail2ban-sendmail-1.0.2-12.el9.noarch 
	  fail2ban-server-1.0.2-12.el9.noarch              libesmtp-1.0.6-24.el9.x86_64                liblockfile-1.14-10.el9.0.1.x86_64    
	  policycoreutils-python-utils-3.6-2.1.el9.noarch  python3-audit-3.1.5-1.el9.x86_64            python3-distro-1.5.0-7.el9.noarch     
	  python3-libsemanage-3.6-2.1.el9_5.x86_64         python3-policycoreutils-3.6-2.1.el9.noarch  python3-setools-4.4.4-1.el9.x86_64    
	  python3-setuptools-53.0.0-13.el9.noarch         

	Complete!

	[white-ghost@localhost ~]$ sudo systemctl start fail2ban
	[white-ghost@localhost ~]$ sudo systemctl enable fail2ban
	Created symlink /etc/systemd/system/multi-user.target.wants/fail2ban.service → /usr/lib/systemd/system/fail2ban.service.
	[white-ghost@localhost ~]$ sudo systemctl status fail2ban
	● fail2ban.service - Fail2Ban Service
	     Loaded: loaded (/usr/lib/systemd/system/fail2ban.service; enabled; preset: disabled)
	     Active: active (running) since Thu 2025-02-06 00:51:30 CET; 13s ago
	       Docs: man:fail2ban(1)
	   Main PID: 48191 (fail2ban-server)
	      Tasks: 3 (limit: 11100)
	     Memory: 10.2M
		CPU: 54ms
	     CGroup: /system.slice/fail2ban.service
		     └─48191 /usr/bin/python3 -s /usr/bin/fail2ban-server -xf start

	Feb 06 00:51:30 localhost.localdomain systemd[1]: Starting Fail2Ban Service...
	Feb 06 00:51:30 localhost.localdomain systemd[1]: Started Fail2Ban Service.
	Feb 06 00:51:30 localhost.localdomain fail2ban-server[48191]: Server ready
```

**🌞 Configurer fail2ban**

```bash
	[white-ghost@localhost ~]$ sudo nano /etc/fail2ban/jail.d/sshd.conf

	[sshd]
	enabled = true
	port = ssh
	filter = sshd
	logpath = /var/log/secure
	maxretry = 7
	findtime = 300
	bantime = 600


	[white-ghost@localhost ~]$ sudo systemctl restart fail2ban
```

🌞 Prouvez que fail2ban est effectif

faites-vous ban

```bash
	[toto@localhost ~]$ ssh -p 19 white-ghost@192.168.56.116
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Received disconnect from 192.168.56.116 port 19:2: Too many authentication failures
	Disconnected from 192.168.56.116 port 19
	[toto@localhost ~]$ ssh -p 19 white-ghost@192.168.56.116
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Received disconnect from 192.168.56.116 port 19:2: Too many authentication failures
	Disconnected from 192.168.56.116 port 19
	[toto@localhost ~]$ ssh -p 19 white-ghost@192.168.56.116
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Permission denied, please try again.
	white-ghost@192.168.56.116's password: 
	Received disconnect from 192.168.56.116 port 19:2: Too many authentication failures
	Disconnected from 192.168.56.116 port 19
```

montrez l'état de la jail fail2ban pour voir quelles IP sont ban

```bash
	[white-ghost@localhost ~]$ sudo fail2ban-client status sshd
	Status for the jail: sshd
	|- Filter
	|  |- Currently failed:	1
	|  |- Total failed:	24
	|  `- Journal matches:	_SYSTEMD_UNIT=sshd.service + _COMM=sshd
	`- Actions
	   |- Currently banned:	1
	   |- Total banned:	1
	   `- Banned IP list:	192.168.56.114
```

levez le ban avec une commande adaptée

```bash
	[white-ghost@localhost ~]$ sudo fail2ban-client set sshd unbanip 192.168.56.114
	1
	[white-ghost@localhost ~]$ sudo fail2ban-client status sshd
	Status for the jail: sshd
	|- Filter
	|  |- Currently failed:	1
	|  |- Total failed:	24
	|  `- Journal matches:	_SYSTEMD_UNIT=sshd.service + _COMM=sshd
	`- Actions
	   |- Currently banned:	0
	   |- Total banned:	1
	   `- Banned IP list:	
```

---

**Conclusion**: Ceci cnclu donc le TP1
