
---
title: "How I bypassed my school's firewall using Shadowsocks"
date: 2023-03-06
draft: false
author: "Eren Mengeş"
authorLink: "/about/"
summary: The school firewall annoyed the hell out of me, so I got around it.
---
>This paper is not to be reproduced in any way. The content is my work and belongs to me as the author. I must be cited as the author if anyone wishes to quote or paraphrase.

## Purpose

This project aims to bypass the school firewall's restrictions without using a commercial VPN or proxy. The method should be resilient to further attempts to block firewall circumvention methods.


## Backstory

This started as a personal project but turned into an essay because I decided to document it. Sometimes I need to access websites that the school firewall blocks. For instance, I am the president of the Ethical Hacking club. I lecture students every week. I needed them to install hacking software that we would use in national and international competitions, but the school firewall blocked the website because it was related to hacking. In conclusion, sometimes, there is a legitimate reason to bypass the firewall.


## Description

Shadowsocks is a proxy software that is extremely lightweight and easy to set up. It is also fast. It uses the SOCKS5 protocol, which has less overhead than most VPN protocols, thus making Shadowsocks a faster alternative. 

The reason I am not using a VPN to bypass the school firewall is that most VPN protocols are easily detectable by commercial firewalls. VPN traffic can be obfuscated to prevent detection, but obfuscation is a time-consuming process. Shadowsocks traffic is very similar to regular HTTPS traffic. Therefore it is tough to detect and block Shadowsocks since blocking HTTPS traffic as a whole isn’t a viable option. 

Additionally, using a commercial VPN or proxy essentially directs my whole traffic through them. It should do that, but I should be able to trust the VPN provider with my entire traffic, including sensitive data such as system telemetry, my DNS traffic, etc. Therefore, I set up my own proxy server through the cloud provider DigitalOcean. DigitalOcean can still see my traffic, but it would be more cumbersome than VPN providers’ ability to see it.

Proxies and VPNs use the client-server model. There is a server serving the service to clients. In this project, my proxy server is the server, and my Macbook is the client. I planned how it would look using network diagrams. Before setting up my proxy server, my connection would look like this:


![Network Diagram Describing the Topology of my Network without the Proxy](/images/shadowsocks/diagram-noproxy.png "Network Diagram Describing the Topology of my Network without the Proxy")


And this would be after setting up my proxy server:


![Network Diagram Describing the Topology of my Network with the Proxy](/images/shadowsocks/diagram-proxy.png "Network Diagram Describing the Topology of my Network with the Proxy")


The green lock represents AES-256 encryption, which is nearly impossible to break using traditional methods. The school firewall will only see that I am sending and receiving regular HTTPS traffic and won’t be able to inspect the inside of my traffic.


## Preparation

I bought a server from the cloud provider DigitalOcean. It had 2 GBs of RAM, a single CPU, 50 GBs of SSD capacity, and 2 TBs of transfer bandwidth. These features would be enough for my use case.




![My Server Configuration 1](/images/shadowsocks/digitalocean-conf1.png "My Server Configuration 1")


I set up Ubuntu on it:



![My Server Configuration 2](/images/shadowsocks/digitalocean-conf2.png "My Server Configuration 2")


I wanted it to be physically close to me to lower latency. Therefore I choose Frankfurt as the server location.




![My Server Configuration 3](/images/shadowsocks/digitalocean-conf3.png "My Server Configuration 3")


After creating the server, I accessed it via SSH. SSH is a protocol that gives you the ability to remotely execute commands in a server. It emulates the server’s shell.

First, I wanted to do a system-wide package upgrade. This is like going to the App Store and updating every app. In Linux-based operating systems, there is usually a package manager that manages all software. In Ubuntu, it is “apt.” I updated the package manager’s indexes with the 


```
apt update
```


command and the packages with the 


```
apt upgrade
```


command.


## Securing my server first

I had to secure my server first to prevent unauthorized access by malicious actors. If I had a passwordless proxy server open to the internet, it would be easy for malicious actors to use the proxy, and I would be legally liable for all their actions. Additionally, if my server was ever exploited and owned, I could get in trouble for what malicious actors are using the server. For example, if a malicious actor hacks my server and starts to use it as a C&C (command and control) server for their own botnet, I would be assumed the owner of the botnet and thus a cybercriminal.

To achieve basic security, I took two simple security measures: using SSH with keys instead of passwords and configuring a firewall. I created an SSH key on my Macbook with the command 


```
ssh-keygen -b 4096
```


I ran the command with the flag -b 4096 because I wanted my SSH key to be larger than usual and thus more secure. I also set a passphrase to this key so that even if the malicious actors seized my SSH key, they would need the passphrase to use it, adding a factor of authentication. I then executed the command 


```
ssh-copy-id root@[IP_OF_MY_SERVER]
```


and it copied my SSH key to my server. Then I edited my SSH config on my server by executing the command 


```
nano /etc/ssh/sshd_config
```


 and set the related variable to no: 


```
PasswordAuthentication no
```


Then I had to configure a firewall so malicious actors could not attack my server. I choose ufw (uncomplicated firewall) as the firewall software because it was easy to use. I installed ufw with the command 


```
apt install ufw
```


and added some rules.

First I allowed all outgoing traffic with the command 


```
ufw default allow outgoing
```


and denied all incoming traffic with the command 


```
ufw default deny incoming
```


Then I allowed ssh so that I could SSH into the machine to continue with my project. I used the command 


```
ufw allow ssh
```


 for that. Then I added the rules 


```
ufw allow from [SCHOOL'S IP] to any port 443
ufw allow from [MY IP] to any port 443
```


so that only I can connect from my home and my school to my server’s port 443, which is the port that my proxy will run on. Then I enabled the firewall with the command 


```
ufw enable
```


and double-checked my rules with the command 


```
ufw status
```




![Terminal Output of ufw status Command](/images/shadowsocks/ufw-rules.png "Terminal Output of ufw status Command")



## Setting up the proxy server

Shadowsocks had a lot of versions in different coding languages that I could choose from. I did not want to use the main port, which uses Python 2, because setting up Python 2 software in 2023 is a nightmare. I tried the Go version, but I could not get it to work. Finally, I decided to use the Rust port, which was called “shadowsocks-rust.” Another reason that I choose to use Rust was that it is deemed a memory-safe language, which means that software written with Rust is usually much more secure in terms of memory-related vulnerabilities.

I installed Rust with the 


```
apt install rust
```


command. 




![Terminal Output of apt install rust Command](/images/shadowsocks/apt-install-rust.png "Terminal Output of apt install rust Command")


Like most languages, Rust has its own package manager called Cargo in which packages are called “crates.” Shadowsocks-rust was available as a crate. 

I then installed shadowsocks-rust, using Cargo, with the 


```
cargo install shadowsocks-rust
```


command.




![Terminal Output of cargo install shadowsocks-rust Command](/images/shadowsocks/cargo-rust.png "Terminal Output of cargo install shadowsocks-rust Command")


After installing shadowsocks-rust, I had to add Cargo’s binary directory to my shell’s PATH so I could run shadowsocks commands from my shell. I did that first by entering my shell config with the command 


```
nano .profile
```


and adding the line 


```
export PATH="/root/.cargo/bin:$PATH"
```


It looked like this:



![My Bash Profile](/images/shadowsocks/bash-profile.png "My Bash Profile")


Then I reassured that by instructing my shell to use the profile I just changed with the command 


```
source /root/.profile
```


I had to create a configuration file for my server to run accordingly. I used the command 


```
mkdir /etc/shadowsocks
```


to create the directory that the config file will be in, then I created the config file with the command 


```
nano /etc/shadowsocks/config.json
```


I created the file like this:


```
{
    "server": "[IP_OF_MY_SERVER]",
    "server_port": 443,
    "password": "[PASSWORD]",
    "method": "aes-256-gcm",
}
```







![My Proxy Server Configuration](/images/shadowsocks/server-config.png "My Proxy Server Configuration")


Then I ran the command


```
ssserver -c /etc/shadowsocks/config.json
```


to start shadowsocks.

It looked like this:



![Terminal Output of My Server's First Logs](images/shadow-on-cli.png "Terminal Output of My Server's First Logs")



## Configuring the client

Then I installed the Shadowsocks client on my MacBook. I couldn’t get the shadowsocks-rust client, which is run from the command line, to work, so I just installed shadowsocksX-ng from Github. It had a nice and easy-to-use GUI. I configured it to use my server:





![Client Configuration](/images/shadowsocks/client-config.png "Client Configuration")


I enabled “global mode”, which forces all system applications to use the proxy, and it worked.

GUI of the client:




![GUI of the client](/images/shadowsocks/shadow-on.png "GUI of the client")


Proof that it is working:



![My Public IP Address located in Frankfurt, whereas I am located in Turkey](/images/shadowsocks/public-ip-proof.png "My Public IP Address located in Frankfurt, whereas I am located in Turkey")



## Performance

I tested and compared my internet performance with and without using the proxy server. I used the cloud provider Cloudflare’s speed test service [speed.cloudflare.com](speed.cloudflare.com).

When not using the proxy server:



![Speedtest Result 1 of when I am not using the proxy server](/images/shadowsocks/perf-noproxy.png "Speedtest Result 1 of when I am not using the proxy server")







![Speedtest Result 2 of when I am not using the proxy server](/images/shadowsocks/location-noproxy.png "Speedtest Result 2 of when I am not using the proxy server")


When using the proxy server:




![Speedtest Result 1 of when I am using the proxy server](/images/shadowsocks/perf-proxy.png "Speedtest Result 1 of when I am using the proxy server")







![Speedtest Result 2 of when I am using the proxy server](/images/shadowsocks/location-proxy.png "Speedtest Result 2 of when I am using the proxy server")


A 43% drop in download speed can be seen. Nevertheless, it is 50 mbit/s, and this is still good for everyday usage. There was no difference in upload speed. Theoretically, latency should have been higher when using the proxy server, but there was no significant difference in latency, which is unusual.


## Test at School

	The proxy does work at school. I have tested it with the website www.kali.org.

Blocked at school without using proxy:



![Website of Kali Linux is Unreachable](/images/shadowsocks/kali-unreachable.png "Website of Kali Linux is Unreachable")


Accessible at school using proxy:




![Website of Kali Linux opens perfectly](/images/shadowsocks/kali-reachable.png "Website of Kali Linux opens perfectly")






![Proof that I am connected to CAMPUS Network](/images/shadowsocks/campus-network-proof.png "Proof that I am connected to CAMPUS Network")



## Conclusion

I’ve set up a proxy server using shadowsocks-rust and a client using shadowsocksX-NG. It successfully circumvents the restrictions placed by the school firewall. Although a decrease in performance is observed, it is still suitable for everyday usage.


## Notes



* I accessed my server through SSH and performed all operations on my server using the BASH shell, only executing commands and using no GUI at all.
* I accessed the server via SSH as root. This is a bad security practice, but it saved me time since I did not have to add “sudo” in front of every command I executed.