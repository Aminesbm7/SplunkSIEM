# Starting Point

**As a starting point to the prject, i already have splunk installed in a Ubuntu Server and my windows client has alredy Sysmon installed and the Universal Forwarder.**


# Enable Remote Desktop on the Windows Client

![imatge](/images/1.png)

If needed you can choose wich user can access via RDP.

![imatge](/images/2.png)

# Performing a Brute Force Attack to our Windows Client

From my Kali machine i have a password.txt with 21 different passwords, where i included my windows client user password for testing.

![imatge](/images/3.png)

With crowbar i start the burteforce attack using the password list mentioned earlier and as you can see i got one rdp-success.

![imatge](/images/4.png)


# View Telemetry via Splunk

So now if i go to the Splunk web interface and i go to the "Search&Reporting" tab, i can filter by "endpoint" and the username of the windows client,
we will see various failed login attempts, splunk is collecting the security events and showing us everything that is happening.

![imatge](/images/5.png)
![imatge](/images/6.png)
![imatge](/images/7.png)

We can also filter by Event Code:

![imatge](/images/8.png)

The 4625 EventCode means login error, and it will show the 20 login attempts and from ***what ip they came from.***

![imatge](/images/9.png)

![imatge](/images/10.png)

![imatge](/images/11.png)

The 4624 EventCode means successfull login attempts, it will show the 1 login that went correctly and from **what ip it came from and the username that logged in.***

![imatge](/images/12.png)

![imatge](/images/13.png)

![imatge](/images/14.png)


# Atomic Red Team Test

First in our powershell, we need to set the execution policy bypass for the current user, we have to open powershell as administrator to do this.

![imatge](/images/15.png)


To install Atomic Red Team atomics, we just need to copy these 2 commands to our powershell, we need to disable the antivirus before downloading the Atomic Red Team tests.

Command 1: IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);

Command 2: Install-AtomicRedTeam -getAtomics

For the question, just hit 'Y'.
![imatge](/images/16.png)

Once its downloaded, we should see this folders in the "Program Files\AtomicRedTeam\atomics".

![imatge](/images/17.png)


Now to test one atomic, we can go to this site and check wich one we want to test: https://attack.mitre.org/

For example, im going to test the "T1136.001", it creates a local user and it adds to administrator group.

![imatge](/images/18.png)


If you execute the atomic test command and it gives you the same error as this one:

![imatge](/images/19.png)

We can solve it by executing these 3 commands in the screenshot:

![imatge](/images/20.png)


Now the command gets executed correctly without any errors, if u look closely, it says that a "NewLocalUser" has been created.

![imatge](/images/21.png)


If we go back to Splunk, and filter by that username we can see that a username with the same name has been created:

![imatge](/images/22.png)


We can also find the part where the net user command has been executed to create the user:

![imatge](/images/23.png)


***This project demonstrates the effectiveness of Splunk in monitoring security events on clients and the value of using Atomic Red Team for realistic attack simulations. By integrating these tools, we can significantly enhance our threat detection and response capabilities. Moving forward, continuous testing and tuning of our detection mechanisms are crucial to staying ahead of evolving threats.***

Amine Sbaay
