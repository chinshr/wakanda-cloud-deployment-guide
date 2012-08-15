# Wakanda Deployment Guide

## Setup on Windows Azure 2008 Server

Signup to Windows Azure 90-day free trial (you will need a valid credit card, a phone or mobile phone) at http://www.windowsazure.com/en-us/pricing/free-trial/

After you have signed up you will need to sign up to "Virtual Machines & Virtual Networks" in the Preview Features section of your account at https://account.windowsazure.com/PreviewFeatures/. You will be immediately approved.

Go to the Manage section at http://manage.windowsazure.com/, select Virtual Machines on the left, select "Create a virtual machine" and click on "From Gallery".

From the list, select "Windows Server 2008 R2 SP1", hit Next.

At VM Configuration, choose a virtual machine name, e.g. Win8Server1, choose a password and select "Small (1 core, 1.75 GB Memory)" and hit Next.

At VM Mode, select "Standalone Virtual Machine", DNS name e.g. wakobar.cloudapp.net, Storage Account "Use Automatically Generated Storage Account", Region, e.g. "West US" and hit Next.

On VM Options, keep "(None)" and click Next. You will see a virutal machines list. Your machine will startup and should be ready within 15 minute. 

Now click on "Connect" a remote desktop protocol configuration file is generated. Go ahead and open it. When prompted for the login information provide your user:Administrator, password:<password you chose>, and domain:wakobar.cloudapp.com. If prompted for a certificate, just confirm and continue.

Now attach storage to the VM, go to the list of virtual machines and select yours. Select "Attach", keep the default selection and enter 5 GB for the drive size and hit Next. It will take some time to attach the storage.

Connect to the virtual machine again. After you log on to the virtual machine, open Server Manager (icon in the status bar). In the left pane, expand "Storage" (bottom), and then click "Disk Management" (bottom). Right-click Disk 2, and then select "New Simple Volume...". Keep on clicking next on the wizard until done.


Sign in back to https://manage.windowsazure.com, click on your running virtual machine and click on Endpoints (below the server name). Select "Add Entpoint" (action bar below). 

In the modal, chose "Add Endpoint", hit next. Enter "MyTCPEndpoint1" as name, keep protocol TCP, enter 80 for public port, enter 80 for private port, hit next. Wait until finished (5 minutes).



## Setup up on Ubuntu Server 12.04

Go to your Azure manager https://manage.windowsazure.com/, click on Virtual Machines and hit "New". Select Virtual Machine and "From Gallery".

In the Wizard "Ubuntu Server 12.04 LTS" and hit Next.

On VM Configuration choose a virtual machine name, e.g. Ubuntu12Server1, select a new user, e.g. "deploy", add password, select "Small (1 core, 1.75 GB Memory)", leave "Upload SSH key for authentication" unchecked and hit Next.

On VM Mode select Standalone Virtual Machine, DNS Name, e.g. wakofoo.cloudapp.net, storage account "Use Automatic generated storage account", keep region "West Coast" or change, hit Next.

VM Options, no additions, next to finish the wizard and create the VM (takes a few minutes). Note: if the virtual machine shows "Stopped" in Status, try to restart it from the menu (below).

Click on your Ubuntu virtual machine https://manage.windowsazure.com to get to the detail page. On the machine's dashboard page navigate to "SSH Details" and get get the location to your machine.

To login to your VM, install Putty first (you need a SSH client) or use the command line on OS X/Linux. Inside the console, e.g., enter:

    ssh -p 22 deploy@wakofoo.cloudapp.net 

Download the latest (production) release. Navigate to http://download.wakanda.org/ProductionChannel and pick the one you want to download and install, the `>` indicates a command line prompt:

    > wget http://download.wakanda.org/.../Wakanda-Server-x64-v2.tar.gz

Unzip the downloaded `.tar.gz` file into the deploy user's root folder, e.g. like this:

    > tar xzvf Wakanda-Server-x64-v2.tar.gz

Launch the Server like this (don't omit the ampersand, it will make Wakdanda run in the background):

    > ~/Wakanda\ Server/Wakanda &
    Welcome to Wakanda Server!
    Publishing solution "DefaultSolution"
    Project "ServerAdmin" published at 127.0.0.1 on port 8080

Before we can access the server admin, we need to configure an endpoint for port 8080. Navigate to https://manage.windowsazure.com click  your ubuntu VM, click on "ENDPOINTS", select "Add endpoint" and hit next, call it e.g. "wak-manager", Protocol: TCP, public port 8080, private port 8080, hit Next/Finish (this may take a few minutes).

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

    http://168.62.7.116:8081

Go crazy!