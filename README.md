<p align="center">
<img src="https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/d1da9ad9-843e-4682-aab3-08bb3beec257"
</p>

<h1>How to install Active Directory in the Cloud (using Azure)</h1>
This turorial is to help you Install Active Directory on Azure Virtual Machine.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (22H2)


<h2>Configuration Steps</h2>
<p>In this tutorial we will create 2 Virtual Machines in the same VNET. One will be a Domain Controller, and the second one will be a Client Machine. We will change the DC to a static IP because you don’t want your Domain Controller to keep changing IP address.
The first thing you need to do is create an azure account. Once the account has been created go to portal.azure.com </p>


To create our first virtual machine. Create a Resource Group from the portal.azure.com site.
Search for “resource groups" on the search bar
- Click on Create (middle of the page)
-	Fill out the requested info
  ![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/5f940065-894b-448f-80d1-b0142469896e)
- Click Next: Tag (bottom left of page)
- Click next: Review + create
-	Click Create

Now Lets create our DC Virtual Machine. Search for virtual machine on the portal search bar.
Search for Virtual Machine on portal.azure.com
-	Virtual Machine Name DC-1
-	Region (US) East US 2
-	Image Windows Server 2022 Datacenter
-	Size Standard_E2s_V3 - 2vcpus, 16 GiB
-	Username: **labuser** (use whatever username you want just make sure you write the information down)
-	Password: **Password12345** (i know this is not a good practice to write down the password here... but this is just for training)
-	Click Review + Create
-	Click Create
-	Wait for the virtual machine  to be created and then do the following (You don't want the DC controller to be changing IP address so set to static)
-	Go to Virtual Machine and click on the DC-1 
-	Click on Network Settings (Left panel)
-	Click on Network Interface
-	Click on IP configurations (Left panel)
-	Click on ipconfig1
-	Check Allocation to Static
![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/35c50e4d-b329-49be-85d9-1bfab9709c07)

<br />

Now Create CLIENT-1 virtual machine
Search for Virtual Machine 
-	Make sure that the virtual machine is in the same Resource Group as your DC-1
  -	In this case the Resource Group is "Active-Directory"
-	Virtual Machine name Client-1
-	Region (US) East US (Same region as the DC-1)
-	Image Windows 10 Pro
-	Size 2vcpu, 16 GiB
-	Username: clientuser (not the same different computer)
-	Password:  Password12345
-	Scroll down and checked Licensing box
-	Click Next: Disks
-	Click Next: Networking
-	Choose the virtual network that the previous Virtual machine created "in this case DC-1-vnet"
-	Click Review + create
-	Click Create

To connect to Client-1 virtual machine… copy the public ip address and use remote desktop connection on your pc to connect to it.
-	Make sure you get the public IP address of the DC-1 (This is to connect via remote desktop connection)
-	Got to virtual machine and click on DC-1, and look for the public ip address (this is done on your main computer not the virtual machine)
-	Now ping from Client-1 virtual machine 
-	Ping -t 10.0.0.4
-	Now log in to DC-1 to enable firewall
-	Once log in search for wf.msc (click windows r... this will open the run command)
  - Click inbound rules
  -	Sort by clicking Protocol and enable ICMPv4
![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/9f41a6e9-a074-47d2-8d2b-01569bc80470)
- Now you should see that Client-1 is able to ping DC-1
![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/3745ace7-f2f7-44d4-876b-a57b43941586)

<br />

<h2> In this Section we are going to install Active Directory </h2>
Now that you are in DC-1 under server manager

-	Click Add roles and features
- Click Next, Next, Next

- Click on Active Directory Domain Services
-	Click Add features
  
![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/4cc8eac8-6c57-48aa-b2ec-b324193027dd)
- Click Next, Next, Next
- Click Install
-	When installation is finished click Close
-	On the Server Manager Dashboard (there is a flag top right with yellow triangle) Click on it
![image](https://github.com/josemiguel-nunez/Active-Directory-Install/assets/163205170/b24cb342-7b23-4941-b9db-66f92e5726a0)

-	On the Active Directory Domain Services Configuration Wizard pop up window
-	Click on Add a new forest
-	Root domain name "mydomain.com" (you can name this whatever you want)
-	Click Next
-	For DSRM Password just used "Password1" (remember this is not a good practice to write down your password on a training document)
-	Click Next, Next, Next
-	Just finish the installation and restart the DC-1
-	But if you notice you are unable to log in with labuser, now you need to log in with **mydomain.com\labuser** & whatever password you chose (because DC-1 became the controller)]

<p>Congratulations you just install Actve Directory on the Cloud</p>
<p>Happy Learning</p>
