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

You can restart your VM if you want to refresh after some modifications (it's a combination of "vagrant halt" and "vagrant up"):

>vagrant reload

After destroying your VM, if you can also remove the box installed in the system with the command:

>vagrant box remove

# Managing the configuration through the Vagrantfile

Vagran boxes are configured using a vagrantfile. Vagrantfiles are simple Ruby programs (easy to read and write) used by the Vagrant CLI to set attributes of a vagrant box (such as CPU, memory, disk and network configurations). Vagrantfiles are mostly single line statements.

## Name

The VirtualBox provider exposes some additional configuration options that allow you to more finely control your VirtualBox-powered Vagrant environments:

- You can customize the name that appears in the VirtualBox GUI by setting the name property:

```
config.vm.provider "virtualbox" do |vb|
  vb.name = "my_vm"
end
```

## Resources

- The amount of memory to allocate to the box & virtual CPUs settings can also be customized:

```
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
```

## Network 

- Vagrant supports multiple networking configuration options for virtual machines. Vagrant supports 3 primary network configurations for VMs:

  - Port forwarding

A port is a number between 1 and 65555 and is used like a channel for TCP communication. Vagrant can forward requests that are sent to a port on the host network to the same or different port on the box network. 

To demonstrate the port forwarding, I installed a web server (nginx) on the ubuntu box:
```
>vagrant ssh
>sudo apt-get update
>sudo apt-get install -y nginx 
>curl localhost
```
The command "curl localhost" let us see the nginx default response in raw HTML directly in the command prompt. 

Now, for the port forwarding, we have to modify the Vagrantfile by uncommenting the line 31 (more secure than the configuration at line 26, the box is not exposed to the host network):
```
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
```
This configuration allows nginx to listen on port 80 on the box network interface. Requests sent to port 8080 on the host will be forwarded to the port 80 on the guest.
After the modification of the configuration through the Vagrantfile, reload your box (with "vagrant reload"). 
Now, if you open a browser on your host machine and go to localhost:8080, you should see the nginx default page appear. The nginx server working on your box is now accessible on your browser through the port forwarding (amazing!).

  - Private network

Following the port forwarding, we'll also put a private network for the box. It will isolated it from the public network and ensure its security.
At line 35, uncomment it: 
```
config.vm.network "private_network", ip: "192.168.33.10"
```
Instead of configure a precise ip address, replace the ip address by "dhcp":
```
config.vm.network "private_network", type: "dhcp"
```
This configuration will use a dhcp server to assign a private non-routable ip address. It can be accessed by any other subnet. In this case, it helps us create isolated networks and it's useful for security and testing purposes. 

After restarting your box (with "vagrant reload"), run the following command:
>ifconfig 
You'll see an IP address beginning with 172 which is one the private address ranges defined by the TCP protocol. Vagrant uses the virtual box host only network option to create a private network that is accessible to the host but no other systems. 

  - Public network

Vagrant boxes are insecure by default with known username and password combinations and insecure cryptographic keys. Securing a VM for use in a public network 

## Provisioners 

- When running your VM for the first time, you can configure provisioners that will install softwares and set configurations, like Ansible, Chef, Docker, Puppet, Salt, Apache, etc. 

# Shared Synchronized Folder

## The default folder

Files can be synchronized inside a folder on the host operating system to a folder in a virtual machine (Vagrant VM). Vagrant do it by default and synchronize two folders:
- On the host operating system, the folder synchronized is the folder where your Vagrant environment is running. 
- On the virtual machine, the folder synchronized (on the root) is the vagrant folder (/vagrant). 
Every files added, deleted or modified in this folder can be done from the host operating system or the virtual machine.

## Synchronize your own folder

You can modify the default configuration and synchronize your own folder. 
On the default Vagrantfile, at line 46, you have to uncomment the following line:

```
config.vm.synced_folder "../data", "/vagrant_data"
``` 

- The first parameter is the folder on the host to be mapped and the path is relative to the current Vagrant environment.
- The next parameter is the path to which the synced folder will be mapped in the box. 

# Go Further 

You can sign up for a Vagrant cloud account, which is free, and upload your own public boxes. If you want to store private boxes, you can sign up for paid account tiers. Vagrant cloud is not the only option to store boxes, you can set up your own private store with a simple web server for a private company network for example. 

# Resources 

I managed this repo using VS Code wih Vagrantfile Support extension (syntax highlighting for Ruby). 

Bibliographie.. 