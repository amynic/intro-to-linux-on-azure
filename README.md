# Women in Tech: Introduction to Linux on Azure

This workshop has been a collaboration between **Researc/hers Code** and **Microsoft**. Researc/hers Code supports women in tech and academia by running skills workshops and podcasting the talent of women in tech and research. To find out more about this story: [https://twitter.com/VictoriaCarr_/status/1098488439081783302](https://twitter.com/VictoriaCarr_/status/1098488439081783302)
and follow [@ResearcHersCode](https://twitter.com/ResearcHersCode), [@AmyKateNicho](https://twitter.com/AmyKateNicho) and [@VictoriaCarr_](https://twitter.com/VictoriaCarr_)

### Check out the GitHub Pages view here: [https://amynic.github.io/intro-to-linux-on-azure/](https://amynic.github.io/intro-to-linux-on-azure/)

![]()
<img src="img/researcherscode.png" alt="Researc/hers Code Logo" width="150" align="middle"/>

## Useful Links
* [Workshop slides for Introduction to Linux with Azure 18/02/19](https://docs.google.com/presentation/d/1Kf0gqkoRqnZmLpbkB-6iiQvCZ9o2l-UMAcPv2C6aZOk/edit?usp=sharing)
* [Linux on Azure Documentation](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm/?WT.mc_id=aiml-0000-amynic)
* [Linux commands](unix_shell.md)
* [Azure Batch Documentation](https://docs.microsoft.com/azure/batch/quick-create-cli/?WT.mc_id=aiml-0000-amynic)

## Introduction

To start this workshop, we will create a Linux Ubuntu Virtual machine (VM) using Microsoft Azure. As this VM will be hosted in the cloud, we will walk through creating ssh keys using the Azure Shell Environment, creating an Ubuntu VM image and connecting to the machine once created

### Pre-requisites
- Have access to a Microsoft Azure Subscription, get it free for 12 months here: [https://azure.microsoft.com/free?WT.mc_id=aiml-0000-amynic](https://azure.microsoft.com/free?WT.mc_id=aiml-0000-amynic)
- Have access to a modern web browser (content tested using Edge and Chrome)
Getting Setup

Go to [https://azure.microsoft.com/?WT.mc_id=aiml-0000-amynic](https://azure.microsoft.com/?WT.mc_id=aiml-0000-amynic) and sign in to the Azure Portal in the top right corner using the email and password you created whilst redeeming the azure pass voucher code.

![Azure Portal](img/portal.jpg)

Now you will enter the Azure Portal Homepage. In the top toolbar by the search box you will see the Azure Shell button (highlighted below) – click this button to open the Azure Shell in the web browser. Once open select ‘Bash’

![Cloud Shell](img/cloud-shell.JPG)

We are now going to create the ssh keys we need to access a Linux machine securely. We can create these keys using the Azure Shell (they will also be stored in your Azure Storage Account).

**Enter the command:**

`ssh-keygen -t rsa -b 2048`

![SSH Key Command](img/ssh-key.JPG)

Keep the default storage location of the key to `/home/<username>/.ssh/id_rsa` by hitting enter
We do not want to create a passphrase for this key, so hit enter again. 

![SSH Key Complete](img/key-created.JPG)

To retrieve your key enter the command:
`cat ~/.ssh/id_rsa.pub`

Now you will see your ssh key, **copy this long value, you will need this later**

![Key View](img/key.JPG)

Close the Azure shell using the small `x` in the top right of the shell window to view the Azure portal in full screen

Create a Virtual Machine in Azure, by clicking `create a resource` in the top left corner and use the search bar. Type `linux ubuntu` and hit enter.

![Create Resource](img/create-resource.JPG)

A long list of possible matches will be listed. Select `Ubuntu Server 18.04 LTS` – a new window (these are called blades in Azure) will open with an information summary about this virtual machine and what it contains.

Select `create` in the bottom left corner

![Create Linux Ubuntu machine](img/ubuntu-server-18.04-LTS.JPG)

Now we need to provide some information to create and access our Linux virtual machine.

- Choose your Azure pass subscription (this may already be selected by default)
- We will `create new` resource group instead of using one from the dropdown – call the resource group a sensible name (such as `linuxvm`)
- choose `OK`

![Resource Group Creation](img/new-resource-group.JPG)

Next, we create some details about the virtual machine instance:
- **Virtual Machine Name:** &lt;choose a name> (example: ‘&lt;alias>linuxvm’)
- **Region:** North Europe
- **Availability Options:** ‘no infrastructure redundancy required’
- **Image:** Ubuntu Server 18.04 LTS
- **Size:** Standard D2s v3

![Setup Image and Size](img/image-and-size.JPG)

Now select how to access the account on the VM. In this case we will choose SSH key, however you can choose username and password instead

- **Authentication Type:** SSH public key
- **Username:** &lt;alias> (example: ‘amyboyd’)
- **SSH public key:** enter the long SSH key we created in the Azure shell earlier
- In the **port selection**, choose ‘Allow selected ports’ and choose SSH(22)
- Then select **‘Next :Disks >’**

![Setup Keys and Ports](img/key-and-ports.JPG)

On the Disks page keep all the defaults selected and choose Next

![Disks Page](img/disks.JPG)


On the Networking page, keep all the defaults again and select next.

![Networking Page](img/networking.JPG)

On the management page, 
- **Enable auto-shutdown** to ‘on’ choose a sensible shutdown time and timezone. 
- Also select ‘on’ for **Notification before shutdown** and enter your email address here. 
- Now choose **Next**

![Management Options](img/auto-shutdown.JPG)

Choose next on Guest Config with no changes

You can, if you wish, add tags to your virtual machine which can be viewed in the portal and during your Azure Billing statement.

Add a key value pair of **‘Workshop : Linux’**
Then choose **Next**

![Add Tags to your resource](img/tags.JPG)

Now you are on the **final validation page** which lists the cost and details of your VM. 

Once Azure completes its validation check you can select create.

![Validation of VM](img/validation-review.JPG)

This will take you to a deployment progress page. 
**This deployment will take around 2 mins 30 seconds in total**

![Deployment Started](img/deployment-started.JPG)

During the deployment services will be created and green ticks will be shown once completed

![Resources populating](img/resources-populate.JPG)

Once the full deployment is complete a notification appears and you can click `Go to resource` to go to the Linux Virtual Machine instance page.

![Completed Deployment](img/complete-deployment.JPG)

The virtual machine page will look like the below and contain all information and settings for the virtual machine

![VM Page and Settings](img/vm-portal.JPG)

Lets now connect to our Linux virtual machine. 

Select `connect` in the top bar. This will bring up the ssh command and information you need to connect to this machine remotely

![Connect to VM](img/connect-info-vm.JPG)

Open the Azure Shell again using the icon next to the top search bar.

Enter the ssh command which consists of:
`ssh <vm-username>@<vm-ip-address>`

hit enter, the command will connect you to the linux machine.

![Connect to machine in Azure Shell](img/connect-vm-azure-shell.JPG)

You can test you are connected using a simple command to create a folder on the system and view it:

- Initially use `ls` to show no information
- Now `mkdir test`
- Now run `ls` again to view the newly created folder

![Test commands on Linux machine](img/testing.JPG)

> Find out more about running Linux VM's on Azure here: [Linux on Azure Documentation](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm/?WT.mc_id=aiml-0000-amynic)

## [Click here to learn Linux commands and execute them in the Azure Virtual Machine in the cloud.](unix_shell.md)
