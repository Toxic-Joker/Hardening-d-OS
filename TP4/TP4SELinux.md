# TP4 : SELinux

## 0. Prérequis

**
🌞 Installer NGINX
```bash
    [it4@b002-10 ~]$ sudo dnf install nginx -y
    [sudo] password for it4: 
    Last metadata expiration check: 0:05:11 ago on Fri 28 Feb 2025 12:55:15 PM CET.
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
    Downloading Packages:
    (1/4): nginx-filesystem-1.20.1-20.el9.0.1.noarch.rpm                                                                                                                                                                           89 kB/s | 8.4 kB     00:00    
    (2/4): rocky-logos-httpd-90.15-2.el9.noarch.rpm                                                                                                                                                                               149 kB/s |  24 kB     00:00    
    (3/4): nginx-1.20.1-20.el9.0.1.x86_64.rpm                                                                                                                                                                                     171 kB/s |  36 kB     00:00    
    (4/4): nginx-core-1.20.1-20.el9.0.1.x86_64.rpm                                                                                                                                                                                1.8 MB/s | 566 kB     00:00    
    --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    Total                                                                                                                                                                                                                         622 kB/s | 634 kB     00:01     
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
    [it4@b002-10 ~]$ sudo systemctl enable --now nginx
    Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /usr/lib/systemd/system/nginx.service.
    [it4@b002-10 ~]$ sudo firewall-cmd --permanent --add-port={80/tcp,443/tcp}
    success
    [it4@b002-10 ~]$ sudo firewall-cmd --reload
    success
    [it4@b002-10 ~]$ sudo systemctl status nginx
    ● nginx.service - The nginx HTTP and reverse proxy server
         Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: disabled)
         Active: active (running) since Fri 2025-02-28 13:01:45 CET; 27s ago
        Process: 18710 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
        Process: 18711 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
        Process: 18712 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
       Main PID: 18713 (nginx)
          Tasks: 2 (limit: 11097)
         Memory: 2.0M
            CPU: 10ms
         CGroup: /system.slice/nginx.service
                 ├─18713 "nginx: master process /usr/sbin/nginx"
                 └─18714 "nginx: worker process"

    Feb 28 13:01:45 b002-10.etudiants.campus.villejuif systemd[1]: Starting The nginx HTTP and reverse proxy server...
    Feb 28 13:01:45 b002-10.etudiants.campus.villejuif nginx[18711]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    Feb 28 13:01:45 b002-10.etudiants.campus.villejuif nginx[18711]: nginx: configuration file /etc/nginx/nginx.conf test is successful
    Feb 28 13:01:45 b002-10.etudiants.campus.villejuif systemd[1]: Started The nginx HTTP and reverse proxy server.

    white-ghost@white-ghost-AORUS-16X-ASG:~$ curl http://192.168.56.109/ -s -v > /dev/null 
    *   Trying 192.168.56.109:80...
    * Connected to 192.168.56.109 (192.168.56.109) port 80
    > GET / HTTP/1.1
    > Host: 192.168.56.109
    > User-Agent: curl/8.5.0
    > Accept: */*
    > 
    < HTTP/1.1 200 OK
    < Server: nginx/1.20.1
    < Date: Fri, 28 Feb 2025 14:54:43 GMT
    < Content-Type: text/html
    < Content-Length: 7620
    < Last-Modified: Wed, 21 Feb 2024 13:12:33 GMT
    < Connection: keep-alive
    < ETag: "65d5f6c1-1dc4"
    < Accept-Ranges: bytes
    < 
    { [7000 bytes data]
    * Connection #0 to host 192.168.56.109 left intact

```

**🌞 Dans la conf NGINX par défaut**

```bash
    [it4@b002-10 ~]$ cat /etc/nginx/nginx.conf
        server {
            listen       80;
            listen       [::]:80;
            server_name  _;
            root         /usr/share/nginx/html;

            # Load configuration files for the default server block.
            include /etc/nginx/default.d/*.conf;

            error_page 404 /404.html;
            location = /404.html {
            }

            error_page 500 502 503 504 /50x.html;
            location = /50x.html {
            }
        }
```

## Part I : Premiers pas

### 1. Enabling SELinux

**🌞 Vérifier l'état actuel de SELinux**

```bash
    [it4@b002-10 ~]$ sudo setenforce 1
    [sudo] password for it4: 
    [it4@b002-10 ~]$ sestatus
    SELinux status:                 enabled
    SELinuxfs mount:                /sys/fs/selinux
    SELinux root directory:         /etc/selinux
    Loaded policy name:             targeted
    Current mode:                   enforcing
    Mode from config file:          permissive
    Policy MLS status:              enabled
    Policy deny_unknown status:     allowed
    Memory protection checking:     actual (secure)
    Max kernel policy version:      33
    [it4@b002-10 ~]$ cat /etc/selinux/config | grep SELINUX
    # SELINUX= can take one of these three values:
    # NOTE: Up to RHEL 8 release included, SELINUX=disabled would also
    SELINUX=permissive
    # SELINUXTYPE= can take one of these three values:
    SELINUXTYPE=targeted
    [it4@b002-10 ~]$ sudo nano /etc/selinux/config
    [it4@b002-10 ~]$ sudo nano /etc/selinux/config
    [it4@b002-10 ~]$ cat /etc/selinux/config | grep SELINUX
    # SELINUX= can take one of these three values:
    # NOTE: Up to RHEL 8 release included, SELINUX=disabled would also
    SELINUX=enforcing
    # SELINUXTYPE= can take one of these three values:
    SELINUXTYPE=targeted
```

### 2. The Z flag

**🌞 Déterminer le type SELinux de...**

```bash
    [it4@b002-10 ~]$ sudo su -s /bin/bash nginx
    bash-5.1$ id -Z
    unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
    bash-5.1$ whoami
    nginx
    [it4@b002-10 ~]$ ls -Z /etc/nginx/nginx.conf
    system_u:object_r:httpd_config_t:s0 /etc/nginx/nginx.conf
    [it4@b002-10 ~]$ ls -Z $(which nginx)
    system_u:object_r:httpd_exec_t:s0 /usr/sbin/nginx
    [it4@b002-10 ~]$ ls -dZ /usr/share/nginx/html
    system_u:object_r:httpd_sys_content_t:s0 /usr/share/nginx/html
    [it4@b002-10 ~]$ ls -Z /usr/share/nginx/html/index.html
    system_u:object_r:httpd_sys_content_t:s0 /usr/share/nginx/html/index.html
    [it4@b002-10 ~]$ ps -ef -Z | grep nginx | grep -v grep
    system_u:system_r:httpd_t:s0    root       18713       1  0 13:01 ?        00:00:00 nginx: master process /usr/sbin/nginx
    system_u:system_r:httpd_t:s0    nginx      18714   18713  0 13:01 ?        00:00:00 nginx: worker process
    [it4@b002-10 ~]$ ss -lnptu -Z | grep nginx
    [it4@b002-10 ~]$ sudo ss -lnptu -Z | grep nginx
    tcp   LISTEN 0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=18714,proc_ctx=system_u:system_r:httpd_t:s0,fd=6),("nginx",pid=18713,proc_ctx=system_u:system_r:httpd_t:s0,fd=6))
    tcp   LISTEN 0      511             [::]:80           [::]:*    users:(("nginx",pid=18714,proc_ctx=system_u:system_r:httpd_t:s0,fd=7),("nginx",pid=18713,proc_ctx=system_u:system_r:httpd_t:s0,fd=7))
```

### 3. Voir les règles définies

**🌞 Afficher les règles liées à NGINX**

```bash
    [it4@b002-10 ~]$ sudo dnf install setools-console -y
    Last metadata expiration check: 2:35:11 ago on Fri 28 Feb 2025 12:55:15 PM CET.
    Dependencies resolved.
    ==============================================================================================================================================================================================================================================================
     Package                                                              Architecture                                             Version                                                         Repository                                                Size
    ==============================================================================================================================================================================================================================================================
    Installing:
     setools-console                                                      x86_64                                                   4.4.4-1.el9                                                     baseos                                                    46 k
    ...
    Installed:
      python3-setools-4.4.4-1.el9.x86_64                                                python3-setuptools-53.0.0-13.el9.noarch                                                setools-console-4.4.4-1.el9.x86_64                                               

    Complete!

    [it4@b002-10 ~]$ sesearch --allow --source httpd_t --target httpd_sys_content_t --class file
    allow domain file_type:file map; [ domain_can_mmap_files ]:True
    allow httpd_t httpd_content_type:file { getattr ioctl lock map open read };
    allow httpd_t httpdcontent:file { append create getattr ioctl link lock open read rename setattr unlink watch watch_reads write }; [ ( httpd_builtin_scripting && httpd_unified && httpd_enable_cgi ) ]:True
    allow httpd_t httpdcontent:file { execute execute_no_trans getattr ioctl map open read }; [ ( httpd_builtin_scripting && httpd_unified && httpd_enable_cgi ) ]:True
```

## Part II : Un peu de conf

### 1. Racine web

**🌞 Créez-moi tout ça**

```bash
    [it4@b002-10 ~]$ sudo mkdir -p /var/www/meow/
    [sudo] password for it4: 
    [it4@b002-10 ~]$ ps aux | grep nginx | grep -v root | awk '{print $1}' | head -n 1
    nginx
    [it4@b002-10 ~]$ sudo chown nginx:nginx /var/www/meow/
    [it4@b002-10 ~]$ echo "<h1>Meow World!</h1>" | sudo tee /var/www/meow/index.html
    -bash: !: event not found
    [it4@b002-10 ~]$ echo '<h1>Meow World!</h1>' | sudo tee /var/www/meow/index.html
    <h1>Meow World!</h1>
    [it4@b002-10 ~]$ sudo chown nginx:nginx /var/www/meow/index.html
    [it4@b002-10 ~]$ sudo chmod 644 /var/www/meow/index.html
    [it4@b002-10 ~]$ sudo chmod 755 /var/www/meow/
```

**🌞 Modifier la conf NGINX**

```bash
    [it4@b002-10 ~]$ sudo nano /etc/nginx/nginx.conf
        root /var/www/meow;
    [it4@b002-10 ~]$ sudo nginx -t
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    [it4@b002-10 ~]$ sudo systemctl restart nginx
    
    white-ghost@white-ghost-AORUS-16X-ASG:~$ curl http://192.168.56.109/ -s -v > /dev/null 
    *   Trying 192.168.56.109:80...
    * Connected to 192.168.56.109 (192.168.56.109) port 80
    > GET / HTTP/1.1
    > Host: 192.168.56.109
    > User-Agent: curl/8.5.0
    > Accept: */*
    > 
    < HTTP/1.1 403 Forbidden
    < Server: nginx/1.20.1
    < Date: Fri, 28 Feb 2025 14:52:44 GMT
    < Content-Type: text/html
    < Content-Length: 153
    < Connection: keep-alive
    < 
    { [153 bytes data]
    * Connection #0 to host 192.168.56.109 left intact
```

**🌞 Logs !**

```bash
    [it4@b002-10 ~]$ sudo cat /var/log/audit/audit.log | grep avc
    type=AVC msg=audit(1740754182.984:592): avc:  denied  { getattr } for  pid=19889 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754312.755:593): avc:  denied  { getattr } for  pid=19889 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754323.614:594): avc:  denied  { getattr } for  pid=19889 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754354.675:595): avc:  denied  { getattr } for  pid=19889 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754364.384:596): avc:  denied  { getattr } for  pid=19889 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754586.943:639): avc:  denied  { getattr } for  pid=19934 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740754622.494:640): avc:  denied  { getattr } for  pid=19934 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
    type=AVC msg=audit(1740755239.929:697): avc:  denied  { getattr } for  pid=19945 comm="nginx" path="/var/www/meow/index.html" dev="dm-0" ino=4205645 scontext=system_u:system_r:httpd_t:s0 tcontext=unconfined_u:object_r:var_t:s0 tclass=file permissive=0
```

**🌞 Etat des lieux**

```bash
    [it4@b002-10 ~]$ ls -dZ /usr/share/nginx/html
    system_u:object_r:httpd_sys_content_t:s0 /usr/share/nginx/html
    [it4@b002-10 ~]$ ls -dZ /var/www/meow
    unconfined_u:object_r:var_t:s0 /var/www/meow
```

**🌞 Conf simpliste**

```bash
    [it4@b002-10 ~]$ chcon -R --reference /usr/share/nginx/html /var/www/meow
    chcon: failed to change context of 'index.html' to ‘system_u:object_r:httpd_sys_content_t:s0’: Operation not permitted
    chcon: failed to change context of '/var/www/meow' to ‘system_u:object_r:httpd_sys_content_t:s0’: Operation not permitted
    [it4@b002-10 ~]$ sudo chcon -R --reference /usr/share/nginx/html /var/www/meow
```

**🌞 Constater le changement**

```bash
    [it4@b002-10 ~]$ ls -dZ /var/www/meow
    system_u:object_r:httpd_sys_content_t:s0 /var/www/meow
```

**🌞 Redémarrez NGINX**

```bash
    [it4@b002-10 ~]$ sudo systemctl restart nginx

    white-ghost@white-ghost-AORUS-16X-ASG:~$ curl http://192.168.56.109/ -s -v > /dev/null 
    *   Trying 192.168.56.109:80...
    * Connected to 192.168.56.109 (192.168.56.109) port 80
    > GET / HTTP/1.1
    > Host: 192.168.56.109
    > User-Agent: curl/8.5.0
    > Accept: */*
    > 
    < HTTP/1.1 200 OK
    < Server: nginx/1.20.1
    < Date: Fri, 28 Feb 2025 15:13:31 GMT
    < Content-Type: text/html
    < Content-Length: 21
    < Last-Modified: Fri, 28 Feb 2025 14:43:35 GMT
    < Connection: keep-alive
    < ETag: "67c1cb97-15"
    < Accept-Ranges: bytes
    < 
    { [21 bytes data]
    * Connection #0 to host 192.168.56.109 left intact
```

### 2. Port

**🌞 Modifier la conf de NGINX**

```bash
    [it4@b002-10 ~]$ sudo nano /etc/nginx/nginx.conf
            listen 8888;
    [it4@b002-10 ~]$ sudo firewall-cmd --add-port=8888/tcp --permanent
    success
    [it4@b002-10 ~]$ sudo firewall-cmd --reload
    success
    [it4@b002-10 ~]$ sudo systemctl restart nginx
    Job for nginx.service failed because the control process exited with error code.
    See "systemctl status nginx.service" and "journalctl -xeu nginx.service" for details.
```

**🌞 Logs logs logs !**

```bash
    [it4@b002-10 ~]$ sudo cat /var/log/audit/audit.log | grep avc
    type=AVC msg=audit(1740755895.466:756): avc:  denied  { name_bind } for  pid=20081 comm="nginx" src=8888 scontext=system_u:system_r:httpd_t:s0 tcontext=system_u:object_r:unreserved_port_t:s0 tclass=tcp_socket permissive=0
```

**🌞 Marquons le port 8888/tcp avec le type http_port_t**

```bash
    [it4@b002-10 ~]$ semanage port -a -t http_port_t -p tcp 8888
    -bash: semanage: command not found
    [it4@b002-10 ~]$ sudo dnf install policycoreutils-python-utils
    Last metadata expiration check: 0:43:39 ago on Fri 28 Feb 2025 03:39:06 PM CET.
    Dependencies resolved.
    ...
    Complete!

    [it4@b002-10 ~]$ sudo semanage port -l |grep 8888
    http_port_t                    tcp      8888, 80, 81, 443, 488, 8008, 8009, 8443, 9000
```

**🌞 Redémarrez NGINX**

```bash
    [it4@b002-10 ~]$ sudo systemctl restart nginx
```

### 3. Your own policy

**🌞 Passer temporairement SELinux en mode permissive**

```bash
    [it4@localhost ~]$ sudo setenforce 0
    [it4@localhost ~]$ sestatus
    SELinux status:                 enabled
    SELinuxfs mount:                /sys/fs/selinux
    SELinux root directory:         /etc/selinux
    Loaded policy name:             targeted
    Current mode:                   permissive
    Mode from config file:          enforcing
    Policy MLS status:              enabled
    Policy deny_unknown status:     allowed
    Memory protection checking:     actual (secure)
    Max kernel policy version:      33
```

**🌞 Ptite conf SELinux**

```bash
    [it4@localhost ~]$ sudo cp /usr/bin/python3.9 /opt/python
    [sudo] password for it4: 
    [it4@localhost ~]$ ls -al /opt | grep calc
    -r-x------.  1 calculatrice calculatrice   780 Mar  2 16:02 calc.py
    [it4@localhost ~]$ semanage fcontext -a -t var_t "/opt(/.*)?"
    ValueError: SELinux policy is not managed or store cannot be accessed.
    [it4@localhost ~]$ sudo !!
    sudo semanage fcontext -a -t var_t "/opt(/.*)?"
    File context for /opt(/.*)? already defined, modifying instead
    [it4@localhost ~]$ sudo restorecon -vRF /opt 
    Relabeled /opt/calc.py from system_u:object_r:bin_t:s0 to system_u:object_r:var_t:s0
    Relabeled /opt/python from unconfined_u:object_r:usr_t:s0 to system_u:object_r:var_t:s0
```

**🌞 Modifiez votre calculatrice.service pour qu'il utilise /opt/python comme ExecStart=**

```bash
    [it4@localhost ~]$ sudo nano /etc/systemd/system/calculatrice.service
    [it4@localhost ~]$ cat /etc/systemd/system/calculatrice.service
    [Unit]
    Description=Super serveur calculatrice

    [Service]
    ExecStart=/opt/python /opt/calc.py
    Restart=always
    User=calculatrice

    # Filtrage des appels système
    SystemCallFilter=~clone3 rt_sigprocmask wait4
    SystemCallArchitectures=native
    NoNewPrivileges=yes

    [Install]
    WantedBy=multi-user.target
```

**🌞 Lancer l'application**

```bash
    [it4@localhost ~]$ sudo systemctl daemon-reload
    [it4@localhost ~]$ sudo systemctl restart calculatrice
    [it4@localhost ~]$ sudo systemctl status calculatrice
    ● calculatrice.service - Super serveur calculatrice
         Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
         Active: active (running) since Sun 2025-03-02 22:38:28 CET; 11s ago
       Main PID: 1878 (python)
          Tasks: 1 (limit: 11097)
         Memory: 3.4M
            CPU: 22ms
         CGroup: /system.slice/calculatrice.service
                 └─1878 /opt/python /opt/calc.py

    Mar 02 22:38:28 localhost.localdomain systemd[1]: Started Super serveur calculatrice.
```

**🌞 Observer les logs**

```bash
    [it4@localhost ~]$ sudo ausearch -m AVC -ts recent | tail -n200
    <no matches>
```

**🌞 Observer la conf autogénérée**

```bash
    [it4@localhost ~]$ sudo ausearch -m AVC -ts recent | tail -n200
    ----
    time->Sun Mar  2 22:38:28 2025
    type=PROCTITLE msg=audit(1740951508.618:391): proctitle="(python)"
    type=SYSCALL msg=audit(1740951508.618:391): arch=c000003e syscall=21 success=yes exit=0 a0=7ffd194bb5a0 a1=1 a2=0 a3=3 items=0 ppid=1 pid=1878 auid=4294967295 uid=0 gid=1001 euid=0 suid=0 fsuid=0 egid=1001 sgid=1001 fsgid=1001 tty=(none) ses=4294967295 comm="(python)" exe="/usr/lib/systemd/systemd" subj=system_u:system_r:init_t:s0 key=(null)
    type=AVC msg=audit(1740951508.618:391): avc:  denied  { execute } for  pid=1878 comm="(python)" name="python" dev="dm-0" ino=12700352 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:var_t:s0 tclass=file permissive=1
    ----
    time->Sun Mar  2 22:38:28 2025
    type=PROCTITLE msg=audit(1740951508.618:392): proctitle="(python)"
    type=PATH msg=audit(1740951508.618:392): item=0 name="/lib64/ld-linux-x86-64.so.2" inode=12583093 dev=fd:00 mode=0100755 ouid=0 ogid=0 rdev=00:00 obj=system_u:object_r:ld_so_t:s0 nametype=NORMAL cap_fp=0 cap_fi=0 cap_fe=0 cap_fver=0 cap_frootid=0
    type=CWD msg=audit(1740951508.618:392): cwd="/"
    type=EXECVE msg=audit(1740951508.618:392): argc=2 a0="/opt/python" a1="/opt/calc.py"
    type=SYSCALL msg=audit(1740951508.618:392): arch=c000003e syscall=59 success=yes exit=0 a0=5609976b0240 a1=56099768ddb0 a2=56099766fd70 a3=1 items=1 ppid=1 pid=1878 auid=4294967295 uid=1001 gid=1001 euid=1001 suid=1001 fsuid=1001 egid=1001 sgid=1001 fsgid=1001 tty=(none) ses=4294967295 comm="python" exe="/opt/python" subj=system_u:system_r:init_t:s0 key=(null)
    type=AVC msg=audit(1740951508.618:392): avc:  denied  { map } for  pid=1878 comm="python" path="/opt/python" dev="dm-0" ino=12700352 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:var_t:s0 tclass=file permissive=1
    type=AVC msg=audit(1740951508.618:392): avc:  denied  { execute_no_trans } for  pid=1878 comm="(python)" path="/opt/python" dev="dm-0" ino=12700352 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:var_t:s0 tclass=file permissive=1
    type=AVC msg=audit(1740951508.618:392): avc:  denied  { read open } for  pid=1878 comm="(python)" path="/opt/python" dev="dm-0" ino=12700352 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:var_t:s0 tclass=file permissive=1
    ----
    time->Sun Mar  2 22:38:28 2025
    type=PROCTITLE msg=audit(1740951508.632:395): proctitle="(python)"
    type=SYSCALL msg=audit(1740951508.632:395): arch=c000003e syscall=16 success=no exit=-25 a0=3 a1=5401 a2=7ffedd1528e0 a3=558996963390 items=0 ppid=1 pid=1878 auid=4294967295 uid=1001 gid=1001 euid=1001 suid=1001 fsuid=1001 egid=1001 sgid=1001 fsgid=1001 tty=(none) ses=4294967295 comm="python" exe="/opt/python" subj=system_u:system_r:init_t:s0 key=(null)
    type=AVC msg=audit(1740951508.632:395): avc:  denied  { ioctl } for  pid=1878 comm="python" path="/opt/calc.py" dev="dm-0" ino=12700360 ioctlcmd=0x5401 scontext=system_u:system_r:init_t:s0 tcontext=system_u:object_r:var_t:s0 tclass=file permissive=1
```

**🌞 Observer la conf autogénérée**

```bash
    [it4@localhost ~]$ sudo ausearch -m AVC -ts recent | tail -n200 | sudo audit2allow -a -m meow

    module meow 1.0;

    require {
	    type init_t;
	    type var_t;
	    class file { execute execute_no_trans ioctl map open read };
    }

    #============= init_t ==============
    allow init_t var_t:file { execute execute_no_trans ioctl open read };

    #!!!! This avc can be allowed using the boolean 'domain_can_mmap_files'
    allow init_t var_t:file map;
```

**🌞 Stocker la conf générée**

```bash
    [it4@localhost ~]$ sudo ausearch -m AVC -ts recent | tail -n200 | sudo audit2allow -a -m meow > meow.te
```

**🌞 Appliquer la conf**

```bash
    [it4@localhost ~]$ sudo checkmodule -M -m -o meow.mod meow.te
    [it4@localhost ~]$ sudo semodule_package -o meow.pp -m meow.mod
    [it4@localhost ~]$ sudo semodule -i meow.pp
    [it4@localhost ~]$ 
```

**🌞 Repasser SELinux en mode enforcing**

```bash
    [it4@localhost ~]$ sudo setenforce 1
    [it4@localhost ~]$ sestatus
    SELinux status:                 enabled
    SELinuxfs mount:                /sys/fs/selinux
    SELinux root directory:         /etc/selinux
    Loaded policy name:             targeted
    Current mode:                   enforcing
    Mode from config file:          enforcing
    Policy MLS status:              enabled
    Policy deny_unknown status:     allowed
    Memory protection checking:     actual (secure)
    Max kernel policy version:      33
```

**🌞 Redémarrer le service**

```bash
    [it4@localhost ~]$ sudo systemctl restart calculatrice
    [it4@localhost ~]$ sudo systemctl status calculatrice
    ● calculatrice.service - Super serveur calculatrice
         Loaded: loaded (/etc/systemd/system/calculatrice.service; enabled; preset: disabled)
         Active: active (running) since Sun 2025-03-02 23:29:15 CET; 4s ago
       Main PID: 2361 (python)
          Tasks: 1 (limit: 11097)
         Memory: 3.3M
            CPU: 8ms
         CGroup: /system.slice/calculatrice.service
                 └─2361 /opt/python /opt/calc.py

    Mar 02 23:29:15 localhost.localdomain systemd[1]: Started Super serveur calculatrice.
```

**🌞 Observer le nouveau module chargé**

```bash
    [it4@localhost ~]$ sudo !!
    sudo semodule -l | grep meow
    meow
```
