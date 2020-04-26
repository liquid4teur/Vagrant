# Presentation 

Vagrant is a tool for building and managing virtual machine environments in a single workflow. It provides easy to configure, reproducible, customized and portable work environments. Vagrant stands on the shoulders of providers like VirtualBox, VMWare, AWS and others. In our case, we'll use Vagrant and VirtualBox as a provider. It perfectly fits to use virtual machines as isolated sandboxes and to create quickly environments for development & testing (and consequently reduce hardware requirements). 

It's good to know that Vagrant uses a single file format to build, run and manage any VM for almost any hypervisor and drastically reduces the amount of time needed to do so.  

In order to work with Vagrant, there are three important components:
- The CLI: you will use it to start and stop Vagrant VMs, initialize new Vagrant environments and manage running VMs. 
- The Vagrantfile: it defines Vagrant VMs and is a small program written in Ruby language and executed by the CLI in order to run a Vagrant VM.
- Vagrant cloud: Hashicorp, the maker of Vagrant, provides a [vagrant cloud](https://app.vagrantup.com) as an online market place and a browser for public virtual machines. Users of Vagrant can also publish their own Vagrant VMs publicly or use a private account to restrict access.

For this demonstration, we are going to use Ubuntu 16.04. 

# Installation of Vagrant 

You have to go to the Vagrant [download page](https://www.vagrantup.com/downloads.html) and download the version of Vagrant depending on the OS you are running. 

Once you installed Vagrant, when running your command prompt you can see the different commands you can use through Vagrant: 

>vagrant list-commands 

**Warning (1)**: If you wish to use VirtualBox on Windows, you must ensure that Hyper-V is not enabled on Windows. You can turn off the feature by running this Powershell command:

>Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All

You can also disable it by going through the Windows system settings:

- Right click on the Windows button and select ‘Apps and Features’.
- Select Turn Windows Features on or off.
- Unselect Hyper-V and click OK.
- You might have to reboot your machine for the changes to take effect.

**Warning (2)**: Also, VirtualBox Guest Additions must be installed so that things such as shared folders can function. Installing guest additions also usually improves performance since the guest OS can make some optimizations by knowing it is running within VirtualBox.

**Warning (3)**: Verify that Intel VT-x (or Hardware Virtualization Feature) is enabled. It helps accelerate virtual machines created in a hypervisor. 

# Configuration 

We first initialize a vagrant environment. We need an Ubuntu 16.04 environment and we have VirtualBox installed: 

- We have go the Vagrant box [page](https://app.vagrantup.com/boxes/search), 
- Then we searche the box we need, in our case we need an Ubuntu 16.04 environment working with a VirtualBox provider, 
- We see that bento/ubuntu-16.04 box matches and is the most downloaded. 

Also, note that "box" is a general term for a Vagrant VM. A box is a copressed file that contains all the files needed to start a Vagrant virtual machine. 

# Initialization 

In order to initialize our vagrant environment, we need to execute the following command (the first part of the command is the organization name and the second part is the distribution inversion): 

>vagrant init ubuntu/xenial64

The execution of this command creates a file called "Vagrantfile". Afterward, this file could be modified depending on the precise configuration you want. 
By executing this command, the current directory is now a Vagrant environment. Consequently, this is where you have to execute the Vagrant commands for your future running VM for example. This aspect facilitates the isolation of different Vagrant environment directories. The environment on a directory will have no impact on other environment on the computer. 

# Launching the VM

After the installation of Vagrant, we have to configure a provider. In fact, Vagrant lauches the VM with the help of a provider. VirtualBox is installed on my laptop. So for the first launch of the environment, I set the provider with VirtualBox:

>vagrant up --provider=virtualbox

It will automatically search, find, download (on vagrantcloud) and install the box. The box is added for the provider (VirtualBox).

You can see the current status (running or not) of the running VM by doing (on the concerned directory):

>vagrant status 

Particular cases:

- If you have many VMs running on your machine and need to list the status of every VMs, you can also execute a command from any directory:

>vagrant global-status

- If you want to execute a command on a VM from another directory, you can do it using its ID rather that to change to its environment directory (the ID is displayed when using the command vagrant global-status).

**Warning**: Vagrant create a ".vagrant" folder. Make sure you specified the .vagrant folder in the .gitignore file. In fact, if you upload the .vagrant folder, it will let your repository be heavier and sometimes it could "break" it. You don't need this folder in your repository, the vagrantfile is enough (it contains all the needed information to initialize a VM). 

# Connection to the VM/Box

Vagrant includes a built-in SSH system to authenticate and connect to a running VM that supports the SSH (Secure Socket Shell) protocol. Vagrant configures boxes at start-up with a common security configuration and a standard key pair for SSH authentication. This authentication method works on boxes/VMs that supports SSH (Linux operating system for example).

Now that your VM is running, you can directly connect to it through the Vagrant CLI command from the Vagrant environment directory with the command:

>vagrant ssh 

To exit the ssh session, simply execute the command:

>exit 

# Managing the VM 

You can suspend your VM with the command:

>vagrant suspend 

You can shutdown your VM gracefully with the command (after that, you can restart it at any time wi): 

>vagrant halt 

If you want to restart your VM, you can do it with the command: 

>vagrant up

You can destroy your VM (it will delete all of the resources associated with the box in the environment, but it will leave the vagrantfile intact), you can do it with the command: 

>vagrant destroy 

After destroying your VM, if you can also remove the box installed in the system with the command:

>vagrant box remove

# Managing the configuration 

The VirtualBox provider exposes some additional configuration options that allow you to more finely control your VirtualBox-powered Vagrant environments:

- You can customize the name that appears in the VirtualBox GUI by setting the name property:

```
config.vm.provider "virtualbox" do |vb|
  vb.name = "my_vm"
end
```

- The amount of memory to allocate to the box & virtual CPUs settings can also be customized:

```
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
```

- Vagrant supports multiple networking configuration options for virtual machines. The network can also be managed, whether you need to put your VM on a private or public network, through a static or dynamic IP address. 

- When running your VM for the first time, you can configure provisioners that will install softwares and set configurations, like Ansible, Chef, Docker, Puppet, Salt, Apache, etc. 

# Shared Folder

Files on the host can be synchronized with files in a Vagrant VM and vice versa. 

# Go Further 

You can sign up for a Vagrant cloud account, which is free, and upload your own public boxes. If you want to store private boxes, you can sign up for paid account tiers. Vagrant cloud is not the only option to store boxes, you can set up your own private store with a simple web server for a private company network for example. 