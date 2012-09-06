# Wakanda Deployment Guide

This guide will walk you through the process of deploying your Wakanda app in the Cloud. We will be installing Windows 2008 Server and Ubuntu 12.04 Linux server operating systems on Windows Azure and Amazon EC2 infrastructure. It should take no more than 20 minutes to get your app running, including signing up to Windows Azure or Amazon accounts.

We would love to see if you could share the URL of your app on our forum at http://forum.wakanda.org. Or drop us an email about your experience at wakanda@wakanda.org. Looking forward to hearing from you and happy deploying!

The guide was written by [Juergen Fesslmeier](wakanda@wakanda.org).

Special thanks to [David Robbins](http://wakanda.org) for sharing his [vacation management solution (PTO)](https://github.com/davidrobbins/pto).

# Setup on Windows Azure

## Windows 2008 Server

Go ahead and sign up to Windows Azure's 90-day free trial (you will need a valid credit card, a phone or mobile phone) at:

    http://www.windowsazure.com/en-us/pricing/free-trial/

After you sign in to your new Azure account, you will need to register for the "Virtual Machines & Virtual Networks" section in the Preview Features in your account. Sign up by navigating to:

    https://account.windowsazure.com/PreviewFeatures/. 

You are approved instantaneously after rolling in and are ready to go to start your first virtual server.

Let's do that and navigate to your Azure Portal, bring up the Azure manager at

    http://manage.windowsazure.com/

select the "Virtual Machines" tab on the left, and click on "+ New" on the  bottom left menu, select Virtual Machine again and proceed with "From Gallery" in the selection.

From the list, choose 

    "Windows Server 2008 R2 SP1 July 2012"

and hit Next.

You are then brought to the "VM Configuration" page of wizard. Choose a virtual machine name, e.g. Win8Server1, along with a password (confirm password) and select 

    "Medium (2 cores, 3.5 GB Memory)"

and hit Next.

Note: At this point, Wakanda requires a two core CPU (or higher) on Windows to run properly. A smaller, e.g. 1 GB memory, or higher, configuration not enough right now.

At VM Mode, select 

    "Standalone Virtual Machine"

and a DNS name, e.g. wakobar.cloudapp.net, the URL under which your app will be available at. For the Storage Account" keep the "Automatically Generated Storage Account", and pick your region, e.g. "West US", or whatever is closest to you as latency time may vary. Hit Next.

On VM Options, under "Availability Set" keep "(None)" and hit Next. 

You will now see a list of your virutal machines with the one you just added. Your newly created machine is starting up and should be ready within a few minutes. 

Once the VM is provisioned, you may need to start the server manually. So select the virtual machine in the list, e.g. "Win8Server1" and click on "Restart" in the menu (bottom).

Let's configure the endpoints of the VM. With endpoints you define outbound communication by opening additional ports and protocols. So on the VM detail page, click on Endpoints (in the top bar just below the server name). There is one endpoint configured ("Remote Desktop", tcp, port 3389).

Select "Add Endpoint" (menu bar in the bottom), and click on Add Endpoint again in the radio buttons, now hit Next. Enter "http" as the name of the new endpoint, select protocol TCP, enter 80 for public port, enter 8081 for private port, now hit Next. 

Note: We assume that the Wakanda app we want to install on the server is running on port 8081. Otherwise, change your private port to whatever your Wakanda solution is running on.

Repeat the previous step for each of the following ports/apps:

    PUBLIC PORT  DESCRIPTION                                PRIVATE PORT
    80           "http", standard http port                 8081
    8080         "wak-admin", Wakanda Admin                 8080
    4443         "wak-debug", Wakanda remote debugging      4443

Note: It may take a few minutes before each endpoint becomes active.

Now, on the VM detail page, click on "Connect". A remote desktop protocol configuration file is generated and downloaded to your machine (make sure you hold on to it). Go ahead and open it (given, you must have Remote Desktop Connection installed on your machine). When prompted for the login credentials provide your user:Administrator, password:<password you chose when creating the VM>, and optionally, the domain e.g. wakobar.cloudapp.com. If prompted for a certificate, just confirm and continue. You should now get connected to the server instance.

Optional: 

Attach storage to the VM. Go to the list of virtual machines and select yours. Select "Attach", keep the default selection and enter 5 GB for the drive size and hit Next. It will take some time to attach the storage. Connect to the virtual machine again. After you log on to the virtual machine, open Server Manager (icon in the status bar). In the left pane, expand "Storage" (bottom), and then click "Disk Management" (bottom). Right-click Disk 2, and then select "New Simple Volume...". Keep on clicking next on the wizard until done.

You can install your Software on this storage and reuse the storage from each VM you install.

## Ubuntu Server 12.04

Go to your Azure manager https://manage.windowsazure.com/, click on Virtual Machines and hit "New". Select Virtual Machine and "From Gallery".

In the Wizard, select:

    "Ubuntu Server 12.04 LTS"

and hit Next.

On VM Configuration choose a virtual machine name, e.g. Ubuntu12Server1, select a new user, e.g. "deploy", add password, select 

    "Small (1 core, 1.75 GB Memory)"

You can leave "Upload SSH key for authentication" unchecked at this point and hit Next.

On VM Mode select Standalone Virtual Machine, DNS Name, e.g. wakofoo.cloudapp.net, storage account "Use Automatic generated storage account", keep region "West Coast" or change, hit Next.

VM Options, no additions, next to finish the wizard and create the VM (takes a few minutes). Note: if the virtual machine shows "Stopped" in Status, try to restart it from the menu (below).

Before we can access the server admin, we need to configure endpoints to make your app publicly available. Navigate to:

    https://manage.windowsazure.com

Click on your Ubuntu VM, click on "ENDPOINTS" (top, under your server name), select "Add endpoint" and hit next, enter a name, e.g. "wak-manager", protocol, e.g. TCP, public port 8080, and private port 8080, and hit Next/Finish (this may take a few minutes).

Create endpoints for each of the following ports: 

    PUBLIC PORT  DESCRIPTION                                PRIVATE PORT
    80           "http", standard http port                 8081
    8080         "wak-admin", Wakanda Admin                 8080
    4443         "wak-debug", Wakanda remote debugging      4443

Click on your Ubuntu virtual machine https://manage.windowsazure.com to get to the detail page. On the machine's dashboard page navigate to "SSH Details" and get copy the location of your machines public IP address or DNS.

To login to your VM, install Putty first (if you are on Windows and need a SSH client) or use the command line on OS X/Linux. 

    $ ssh <user>@<server>

E.g. remember you can paste the public server IP/DNS into the terminal:

    $ ssh -p 22 deploy@wakofoo.cloudapp.net 

or,

    $ ssh deploy@186.10.2.123 

# Setup on Amazon EC2

## Setup of Windows Server 2008

Note: This section needs more work.

Signup or sign in to your to Amazon AWS. When you sign in, make sure you will have a telephone number and your credit card ready. Amazon will only charge your card if you using computing time. You can always shut-down your server.

Go to the Management Console at https://console.aws.amazon.com in your browser and click on EC2.

Select Launch Instance. It brings up a setup wizard, sinde choose the first option Classic Wizzard.

On the Choose an AMI page, click on the "Community AMIs" tab, set search filter to "All Images" and look for the following "Windows Server with Wakanda" images:

    (ami-14ce687d)
    -> ami-cbc87da2  Default Wakanda Server 2008 R2 64-bit

Continue by clicking "Select" next to the matching AMI in the list.

On the "Instance Details" step of the wizard, select 

    * Number of instances: 1
    * Instance Type: Micro (t1.micro, 613 MiB)
    * Select Launch Instances, as EC2, Availability Zone: "No Preference" (or us-east-1a, etc. if you prefer)

Hit Next onto the Advanced options.

    * Kernel ID: Use Default
    * RAM Disk ID: Use Default
    * Monitoring: off
    * Keep everything else as default

Hit Continue to the next step. 

On the Storage Device Configuration you should see the following default storage mountpoint in your list:

    Type    Device       Snapshot ID      Size     Volume Type   Delete on Termination
    Root    /dev/sda1    snap-946b85eb    30GiB    standard      true

Hit the Edit button next to the storage device and change Deleteon Terminate to false, select Save, and then hit Next to continue.

On the adding tags to your instance hit Next to continue.

On the Create Key Pair step of the Wizard select "Create a new Key Pair", 1. Enter a name for your key pair, e.g. "wakowin" and 2. Click to create your key pair: Hit Create & Download your Key Pair. It will then bring you to the next step of the Wizard.

On the Configure Firewall, choose the default item in the Security Groups. Hit Next to Continue to the Review page and hit Launch to launch the instance. Click "View your instance on the Instance page" or Close and select Instances.

To connect to your newly created instance, open "Remote Desktop Connection" either on Windows or your Mac.

## Setup of Ubuntu 12.04 Server










# Installing Wakanda

## Installing Wakanda on Windows 2008 Server

Login to your server instance using Remote Desktop Client.

* On Windows Azure: double-click on the generated Remote Desktop Client file you have previously downloaded, e.g. Win8Server1.rdp

* On Amazon EC2: Copy & paste the public server location into the Remote Desktop Client location, using username: Administrator, password: <decrypted password, see Amazon section>

Open IE on the Server machine and direct to the downloads area of Wakanda:

    http://www.wakanda.org/downloads

Go to the Wakanda for Windows section and download:

    "Server 64-bit"

Open the archive and copy the contents to Desktop. You should now see a folder:

    /Users/Administrator/Desktop/Wakanda\ Server

We need to configure the Firewall to let incoming traffic through the Wakanda Server. In Control Panel navigate to "Configure Windows Firewall". In the top-left corner, click on "Allow a program or feature through Windows Firewall". Click on "Allow another program..." and browse to "/Users/Administrator/Wakanda\ Server\Wakanda\ Server.exe", click on "Add" and OK.

Launch the Wakanda Server in "/Users/Administrator/Wakanda\ Server\Wakanda\ Server.exe". Click on "Run" if you should see a security warning.

You should see the following message in a console windows:

    Welcome to Wakanda Server!
    
    Publishing solution "DefaultSolution"
        Project "ServerAdmin" published at 127.0.0.1 on port 8080

Note: If you don't see the full text output (avove), you may have configured your server incorrectly. Remember, your server instance needs to run 2 cores, or more.

To check if you server is publicly accessible, open up a browser (Chrome, Safari, Firefox) and enter the location (you get the IP address or DNS of your public server from the Azure/Amazon console.), e.g.:

    http://168.62.204.176:8080

You should see Wakanda admin interface launch inside the browser.

Back in the RDC open an IE to download an already written Wakanda solution. Navigate to 

    https://github.com/davidrobbins/pto

Click on the ZIP button next to the repository address and download the archive (select open). Copy the files and rename the folder:

    /Users/Administrator/apps/pto

Finally, you want to add the application to the Wakanda app server. Navigate to the sever admin, either on the Windows server or on your local browser, e.g. http://168.62.204.176:8080/. 

Click on "Browse" and enter the following location of your app:

    C:/Users/Administrator/apps/pto/PTOb201.waSolution

Checking back on your server in the Wakanda Server window, you should see something like the following:

    Welcome to Wakanda Server!

    Publishing solution "DefaultSolution"
        Project "ServerAdmin" published at 127.0.0.1 on port 8080
    
    Solution closed
    
    Published solution "PTOb201"
        Project "PTOb201" published at 127.0.0.1 on port 8081

Access the app on your public IP/DNS, like for example:

    http://168.62.7.116

Or in case you are not re-routing port 80 to your private 8081 port you can access it like this:

    http://168.62.7.116:8081

You should see the wonderful PTO application in action. Enjoy!

## Installing Wakanda on Ubuntu Linux

In a terminal (on your local machine) connect to your Ubuntu server, deploy account, e.g.

    > ssh deploy@wakofoo.cloudapp.net
    Password: <enter password>
    deploy@ubuntu:~$ 
    
You will be in the `deploy` user's home folder. When you enter `pwd` at the prompt, you should see `/home/deploy`. Otherwise, just enter `cd`, which brings you back to your home folder.

Download the latest (production) release. Outside your terminal, browse to

    http://download.wakanda.org/ProductionChannel

and pick a build the you want to download and install. Right click on the downloadable package and copy the link. Then return to the terminal, at the prompt (the `$` indicates a command line prompt) enter the following (paste the link):

    $ wget http://download.wakanda.org/.../Wakanda-Server-x64-v2.tar.gz

Unzip the downloaded `.tar.gz` file into the deploy user's root folder, e.g. like this:

    $ tar xzvf Wakanda-Server-x64-v2.tar.gz

Launch the Server like this (don't omit the ampersand, it will make Wakdanda run in the background):

    $ ~/Wakanda\ Server/Wakanda &
    Welcome to Wakanda Server!
    Publishing solution "DefaultSolution"
    Project "ServerAdmin" published at 127.0.0.1 on port 8080

Access the server manager from your public browser like (you get the public IP address from your Azure managing console):

    http://168.62.7.116:8080
    
Install a solution on the machine, e.g.:

    mkdir ~/apps
    cd apps
    sudo apt-get install git
    git clone git://github.com/davidrobbins/PTO.git

Inside the server manager, add the PTO solution. Hit the "Browse" button in the status bar. As Solution path point to the solution location on the server and hit "Launch", such as: 

    /home/deploy/apps/PTO/PTOb201.waSolution

On the server terminal you should get a log message like

    Publishing solution "PTOb201"
    Project "PTOb201" published at 127.0.0.1 on port 8081 

Before you can access the application, add another endpoint for port 8081.

Access the app here:

    http://168.62.7.116

In case you are not re-routing port 80 to your private 8081 port you can access it here:

    http://168.62.7.116:8081

You should see the wonderful PTO application in action. Enjoy!

