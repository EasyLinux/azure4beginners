#Usar una imagen Linux propia en Azure


This article shows you how to create and upload a virtual hard disk (VHD) so you can use it as your own image to create virtual machines in Azure. You'll learn how to prepare the operating system so you can use it to create multiple virtual machines based on that image. 


NOTE:

You don't need any experience with Azure VMs to complete the steps in this article. However, you do need an Azure account. You can create a free trial account in just a couple of minutes. For details, see Create an Azure account. 

A virtual machine in Azure runs the operating system that's based on the image you choose when you create the virtual machine. Your images are stored in VHD format, in .vhd files in a storage account. For more information about disks and images in Azure, see Manage Disks and Images.

When you create the virtual machine, you can customize some of the operating system settings so they're appropriate for the application you want to run. For instructions, see How to Create a Custom Virtual Machine.

Important: The Azure platform SLA applies to virtual machines running the Linux OS only when one of the endorsed distributions is used with the configuration details as specified in this article. All Linux distributions that are provided in the Azure image gallery are endorsed distributions with the required configuration.

Prerequisites

This article assumes that you have the following items:

•A management certificate - You have created a management certificate for the subscription for which you want to upload a VHD, and exported the certificate to a .cer file. For more information about creating certificates, see Create a Management Certificate for Azure. 


•Linux operating system installed in a .vhd file. - You have installed a supported Linux operating system to a virtual hard disk. Multiple tools exist to create .vhd files. You can use a virtualization solutions such as Hyper-V to create the .vhd file and install the operating system. For instructions, see Install the Hyper-V Role and Configure a Virtual Machine. 

Important: The newer VHDX format is not supported in Azure. You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.

For a list of endorsed distributions, see Linux on Azure-Endorsed Distributions. Alternatively, see the section at the end of this article for Information for Non Endorsed Distributions.


•Linux Azure command-line tool. If you are using a Linux operating system to create your image, you use this tool to upload the VHD file. To download the tool, see Azure Command-Line Tools for Linux and Mac.


•Add-AzureVhd cmdlet, which is part of the Azure PowerShell module. To download the module, see Azure Downloads. For reference information, see Add-AzureVhd.


For all distributions please note the following:

•When installing the Linux system it is recommended to utilize standard partitions rather than LVM (often the default for many installations). This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.


•The Azure Linux Agent (waagent) is not compatible with NetworkManager. Networking configuration should be controllable via the ifup/ifdown scripts. Most of the RPM/Deb packages provided by distributions configure NetworkManager as a conflict to the waagent package, and thus will uninstall NetworkManager when you install the Linux agent package. The Azure Linux Agent also requires that the python-pyasn1 package is installed.


•NUMA is not supported for larger VM sizes due to a bug in Linux kernel versions below 2.6.37. Manual installation of waagent will automatically disable NUMA in the GRUB configuration for the Linux kernel. This issue primarily impacts distributions using the upstream Red Hat 2.6.32 kernel.


•It is recommended that you do not create a SWAP partition at installation time. You may configure SWAP space by using the Azure Linux Agent. It is also not recommended to use the mainstream Linux kernel with an Azure virtual machine without the patch available at the Microsoft web site (many current distributions/kernels may already include this fix).


•All of the VHDs must have sizes that are multiples of 1 MB




1.In the center pane of Hyper-V Manager, select the virtual machine.


2.Click Connect to open the window for the virtual machine.


3.Replace the current repositories in your image to use the azure repositories that carry the kernel and agent package that you will need to upgrade the VM. The steps vary slightly depending on the Ubuntu version.

Before editing /etc/apt/sources.list, it is recommended to make a backup sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

Ubuntu 12.04:
sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
sudo apt-add-repository 'http://archive.canonical.com/ubuntu precise-backports main'
sudo apt-get update

Ubuntu 12.10:
sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
sudo apt-add-repository 'http://archive.canonical.com/ubuntu quantal-backports main'
sudo apt-get update

Ubuntu 13.04+:
sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
sudo apt-get update


4.Update the operating system to the latest kernel by running the following commands : 

Ubuntu 12.04:
sudo apt-get update
sudo apt-get install hv-kvp-daemon-init linux-backports-modules-hv-precise-virtual
(recommended) sudo apt-get dist-upgrade
sudo reboot

Ubuntu 12.10:
sudo apt-get update
sudo apt-get install hv-kvp-daemon-init linux-backports-modules-hv-quantal-virtual
(recommended) sudo apt-get dist-upgrade
sudo reboot

Ubuntu 13.04, 13.10 and 14.04:
sudo apt-get update
sudo apt-get install hv-kvp-daemon-init
(recommended) sudo apt-get dist-upgrade
sudo reboot


5.Ubuntu will wait at the Grub prompt for user input after a system crash. To prevent this, complete the following steps:

a) Open the /etc/grub.d/00_header file.

b) In the function make_timeout(), search for if ["\${recordfail}" = 1 ]; then

c) Change the statement below this line to set timeout=5.

d) Run 'sudo update-grub'.


6.Modify the kernel boot line for Grub to include additional kernel parameters for Azure. To do this open /etc/default/grub in a text editor, find the variable called "GRUB_CMDLINE_LINUX_DEFAULT" (or add it if needed) and edit it to include the following parameters:
GRUB_CMDLINE_LINUX_DEFAULT="console=ttyS0 earlyprintk=ttyS0 rootdelay=300"

Save and close this file, and then run 'sudo update-grub'. This will ensure all console messages are sent to the first serial port, which can assist Azure technical support with debugging issues. 


7.In /etc/sudoers, comment out the following line, if it exists:
Defaults targetpw


8.Ensure that the SSH server is installed and configured to start at boot time.


9.Install the agent by running the following commands with sudo:
apt-get update
apt-get install walinuxagent

Note that installing the walinuxagent package will remove the NetworkManager and NetworkManager-gnome packages, if they are installed.


10.Do not create swap space on the OS disk.

The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure. Note that the local resource disk is a temporary disk, and might be emptied when the VM is deprovisioned. After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:
ResourceDisk.Format=y
ResourceDisk.Filesystem=ext4
ResourceDisk.MountPoint=/mnt/resource
ResourceDisk.EnableSwap=y
ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.


11.Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:
waagent -force -deprovision
export HISTSIZE=0
logout


12.Click Shutdown in Hyper-V Manager.

Step 2: Create a storage account in Azure

A storage account represents the highest level of the namespace for accessing the storage services and is associated with your Azure subscription. You need a storage account in Azure to upload a .vhd file to Azure that can be used for creating a virtual machine. You can create a storage account by using the Azure Management Portal.

1.Sign in to the Azure Management Portal.


2.On the command bar, click New.

Create storage account 


3.Click Storage Account, and then click Quick Create.

Quick create a storage account 


4.Fill out the fields as follows:

Enter storage account details 


5.Under URL, type a subdomain name to use in the URL for the storage account. The entry can contain from 3-24 lowercase letters and numbers. This name becomes the host name within the URL that is used to address Blob, Queue, or Table resources for the subscription.


6.Choose the location or affinity group for the storage account. By specifying an affinity group, you can co-locate your cloud services in the same data center with your storage.


7.Decide whether to use geo-replication for the storage account. Geo-replication is turned on by default. This option replicates your data to a secondary location, at no cost to you, so that your storage fails over to a secondary location if a major failure occurs that can't be handled in the primary location. The secondary location is assigned automatically, and can't be changed. If legal requirements or organizational policy requires tighter control over the location of your cloud-based storage, you can turn off geo-replication. However, be aware that if you later turn on geo-replication, you will be charged a one-time data transfer fee to replicate your existing data to the secondary location. Storage services without geo-replication are offered at a discount.


8.Click Create Storage Account.

The account is now listed under Storage Accounts.

Storage account successfully created 


 Step 3: Prepare the connection to Azure

Before you can upload a .vhd file, you need to establish a secure connection between your computer and your subscription in Azure. 

1.Open an Azure PowerShell window.


2.Type: 

Get-AzurePublishSettingsFile

This command opens a browser window and automatically downloads a .publishsettings file that contains information and a certificate for your Azure subscription. 


3.Save the .publishsettings file. 


4.Type:

Import-AzurePublishSettingsFile <PathToFile>

Where <PathToFile> is the full path to the .publishsettings file. 

For more information, see Get Started with Azure Cmdlets 


 Step 4: Upload the image to Azure

When you upload the .vhd file, you can place the .vhd file anywhere within your blob storage. In the following command examples, BlobStorageURL is the URL for the storage account that you created in Step 2, YourImagesFolder is the container within blob storage where you want to store your images. VHDName is the label that appears in the Management Portal to identify the virtual hard disk. PathToVHDFile is the full path and name of the .vhd file. 

Do one of the following:

•From the Azure PowerShell window you used in the previous step, type:

Add-AzureVhd -Destination <BlobStorageURL>/<YourImagesFolder>/<VHDName> -LocalFilePath <PathToVHDFile>

For more information, see Add-AzureVhd.


•Use the Linux command-line tool to upload the image. You can upload an image by using the following command:
Azure vm image create <image name> --location <Location of the data center> --OS Linux <Sourcepath to the vhd>

