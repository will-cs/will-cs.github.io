---
layout: post
title:  "Ssh between Ubuntu Machines on LAN"
date:   2016-07-13
categories: ubuntu network
---

Ssh access across Ubuntu computers on a local network.
======================================================
This a post about how to get ssh working between to Ubuntu computers on a local network. First check if the computer is actually reachable on the network, if it is then several steps are listed to ensure ssh should work. It was tested with one computer running Ubuntu 14.04LTS, and another running 16.04LTS.

First to test if the computer is discoverable.
==============================================
From one of the ubuntu computers run the following to discover local computers on the network:
{% highlight bash %}
avahi-browse -tl _workstation._tcp
{% endhighlight %}
If it comes up empty, it means the other Ubuntu computers cannot be seen. So either you're missing an ethernet cable, or perhaps the router/switch you are using is misconfigured.
If it comes up with a computer name, ie "Terrier.home" then that means you've found a Ubuntu computer called "Terrier". Assuming that is the computer you are trying to connect to then that's good. You can try ssh'ing to it now `ssh user@Terrier.home` where Terrier is your login on the computer Terrier. If it works, then the rest of this post can be ignored :D. Otherwise on with steps of trying to fix it.

Next to check the correct packages are installed. The package "open-ssh-client" should be installed on the computer you are trying to ssh from. You also need the "openssh-server" package installed on the server you are cloning from. These packages can be installed via apt: `sudo apt-get install <PACKAGE-NAME>`
Now after installing openssh-server, ensure it's running by running the following:
{% highlight bash %}
sudo service ssh status
{% endhighlight %}
If it's not running, try starting it with `sudo service ssh start`. It should now hopefully be running.

Next on the server you want to check if the Ubuntu firewall is turned on, and if so wether it will allow ssh traffic.
First check if you have the Ubuntu firewall installed:
{% highlight bash %}
sudo ufw status
{% endhighlight %}
If it's disabled, then that should be fine. If it's enabled then you want to add a rule to allow all incoming ssh traffic. Don't worry, they still need a password or approved ssh key to login to the computer - it doesn't just let anyone in! {% highlight bash %} sudo ufw allow in log-all 22 {% endhighlight %}
This allows all traffic on port 22 (the default ssh port) and logs it to `/var/log/syslog` and also to `/var/log/ufw.log`.


Now test the ssh connection! In the following snippet <COMPUTER-NAME> is whatever you found it as from the start of the post, I used an example "Terrier". <USER> is the user you have the password for on the other computer.
{% highlight bash %}
ssh -Tv <USER>@<COMPUTER-NAME>.home
{% endhighlight %}
That should prompt you for a password, then allow you to connect if you enter the right password.

If you have an ssh key generated, you can add this to your remote server machine so that ssh uses your ssh keys instead of prompting for a password each time. You can do this with the folowing:

{% highlight bash %}ssh-copy-id <USER>@<COMPUTER-NAME>.home{% endhighlight %}
This command was a convenience command, it looks for public ssh keys you have in .ssh (or listed under .ssh/config) and copy them to "~/.ssh/authorized_keys" file on the server. This means that the client can login to the server using the ssh keys instead of being prompted for a password each time.

Now if you try to test the ssh connection again, the ssh keys you just installed on the server should mean there is no password prompt :).

That was a blog post on how to SSH between two Ubuntu computers on a LAN, detailing some issues I came across which could happen to you! If you found it helpful please do let me know!
