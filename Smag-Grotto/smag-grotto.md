# Smag Grotto

![](img/1.png)

Room : https://tryhackme.com/room/smaggrotto

`wireshark` `privesc`

Author : [@jakeyee](https://tryhackme.com/p/jakeyee)

## Reconnaissance:

Let's start with `nmap` scan.

![](img/2.png)

Discoverd 2 Ports, Let's check port 80.

![](img/3.png)

## Enumeration:

From `gobuster` result we will get directory called `/main`

![](img/4.png)

![](img/5.png)

It has a `pcap` file to download, let's analize it with `wireshark`.

![](img/6.png)

Taking good look at it, We can find HTTP POST request to `development.smag.thm/login.php` and also username and password that used to login on clear text.

Now add `development.smag.thm` to our `/etc/hosts` file.

![](img/7.png)

![](img/8.png)

There is a login portal, So now we can login with credentials we found on `.pcap` file. and it will redirect to `/admin.php`.

![](img/9.png)

its a webshell, So let's input [reverse shell](https://www.revshells.com/) command to get reverse connection.

![](img/10.png)

Spawn a python shell using ; `python3 -c 'import pty;pty.spawn("/bin/bash")'` 

Enumerating further more we can see interesting job running on `/etc/crontab`

![](img/11.png)

Here we can manipulate that get user privilege.

![](img/12.png)

We have write permission, So we can add our public key to `/opt/.backups/jake_id_rsa.pub.backup` get User jake.

![](img/13.png)

Now we can login via `SSH` to user `jake` without password.

![](img/14.png)

## Privilege Escalation:

Let's check sudo privilege for `jake` with command `sudo -l`

![](img/15.png)

Now get exploit on [GTFObins](https://gtfobins.github.io/gtfobins/apt-get/) for `/usr/bin/apt-get` to get root shell.
```
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```
![](img/16.png)
