# ColddBox: Easy

![](img/1.png)

Room : https://tryhackme.com/room/colddboxeasy

`security` `box` `privesc` `wordpress`

Author : [@martinfriasc](https://twitter.com/martinfriasc)

## Reconnaissance:

Let's start with `nmap` scan.

![](img/2.png)

Only port 80 is open lets's check it.

![](img/3.png)

We can see server is running `wordpress` with an old version `4.1.31`.

## Enumeration

From `Gobuster` result we get directory called `/hidden`

![](img/6.png)
![](img/7.png)

From the directory we get some usernames maybe.
Let's run the `wpscan`

![](img/4.png)
![](img/5.png)

So users confirmed, Give a try login with bruteforce method.

![](img/8.png)

We get Username:Password, let's login via `/wp-login.php`

And add php-reverse shell on `404 Template` and call it.

![](img/9.png)
![](img/10.png)

It's a wordpress server so let's check for `/wp-config.php` file to get credentials.

![](img/11.png)

Now we can Switch user to `c0ldd` and get user flag.

![](img/12.png)

## Privilege Escalation:

Running `sudo -l` we see that we can run some certain binaries as the root user.

![](img/13.png)

Here iam using `chmod` which will changes permission of a binary file or executable,

Add a SUID bit on bash and get a root shell that way `sudo chmod +s /bin/bash`

![](img/14.png)

Now run `/bin/bash -p` to get root shell

![](img/15.png)

## Thank you for reading my writeup!
