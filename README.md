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

You can suspend your VM. The suspend command is similar to the hibernate feature of a laptop. The box is unloaded from memory but not shot down. The contents of the box RAM are written to the disk. You can do so with the command:

>vagrant suspend 

After a "vagrant suspend", you can bring back the VM to exactly the same state as it was when it was suspended.

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

## Resources & Providers

Vagrant providers allow Vagrant to define a box for a particular hypervisor (Vagrant includes built-in providers for VirtualBox and Hyper-V). Providers section of a Vagrantfile (beginning at line 52 of the vagrantfile) is used to set provider-specific configurations such as CPU, memory and other configurations.

- The amount of memory to allocate to the box & virtual CPUs settings can also be customized:

```
config.vm.provider "virtualbox" do |vb|
  vb.memory = "1024"
  vb.cpus = 2
end
```

You can verify the memory allocated with the linux command:
>vmstat -s

You can verify the virtual CPUs allocated with the linux command:
>lscpu

- With the basic configuration of Vagrant, boxes are running in headless mode (we've been connecting via SSH using the vagrant ssh command to do work in the box), which means that it has not dedicated window of its own. You can also enable the possibility to display the VirtualBox GUI when booting the machine: 

```
vb.gui = true
```

You can add more video RAM allocated to the box to support a graphical interface. It will help you have a bigger window displayed. Add this line to the provider section of the Vagrantfile:
```
vb.customize ["modifyvm", :id, "--vram", "16"]
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
Now, if you open a browser on your host machine and go to localhost:8080 (or 127.0.0.1:8080), you should see the nginx default page appear. The nginx server working on your box is now accessible on your browser through the port forwarding (amazing!).

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

Vagrant provisioners are scripts that can be defined inline in a Vagrantfile or externally in a separate file (as external script). By default, provisioners are executed the first time a box runs. They are used to set up a base box for a particular purpose. Provisioners can:

- Install softwares,
- Download application code,
- Set configurations needed for a particular environment.

Previously, in the network part of this readme, we installed a nginx web server manually. This time we'll do it through a provisioner inside the Vagrantfile. To do so, we'll create an external bash script (executable on linux) nginx-install.sh with those two lines: 

```
sudo apt-get update
sudo apt-get install -y nginx
```

This script will be added to the Vagrantfile and will install a nginx web server. At line 67, we uncomment the provisioning section and precise the path to the bash script:
```
config.vm.provision "shell", path: "provisioners/nginx-install.sh"
```

You can verify the presence of nginx with the command:
>nginx -v

If the box was running before the configuration of the script, you have to destroy the box and start it again (because provisioners are executed the first time a box runs).

I also kept the previous network configuration with the port forwarding and consequently the nginx webserver is now accessible via my web browser.

You can configure provisioners that will install softwares and set configurations, like Ansible, Chef, Docker, Puppet, Salt, Apache, etc. Other provisioners include file provisioners which copy files from the host machine to the box on first boot.

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

# Go Deeper

## Multi-machine configuration & Production

By default, Vagrant files start a single box.

Vagrant is useful for testing software (through test environments) in a sandbox environment. But applications are usually run in production environments with application in one server and databases on a separate server. 

Vagrant can run boxes in the same way, with multi-machine vagrantfiles. Vagrant is able to define and control multiple guest machines per Vagrantfile inside one "multi-machine" environment. You don't need to create many environments to run different boxes. In this case, only one Vagrant environment can run multiple machines and it's useful in many cases:
- Modeling a multi-server production, such as separating a web and database server.
- Modeling a distributed system and how they interact with each other.
- Testing an interface, such as an API to a service component.
- Disaster-case testing: machines dying, network partitions, slow networs, etc.

We create our vagrantfile in "multi-env" folder:
>vagrant init bento/ubuntu-16.04

### Defining Multiple Machines

Multiple machines are defined within the same Vagrantfile using the **config.vm.define** method call. We create a new object for each box using **define** and with that we can introduce two different blocks for the two boxes we want to create.

```
Vagrant.configure("2") do |config|

  config.vm.define "db" do |db|
    db.vm.box = "bento/ubuntu-16.04"
    db.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
    db.vm.network "private_network", ip: "192.168.33.20"
    db.vm.provision "shell", path: "provisioners/install-mongo.sh"
  end 

  config.vm.define "web" do |web|
    web.vm.box = "bento/ubuntu-16.04"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
    web.vm.network "private_network", ip: "192.168.33.10"
    web.vm.provision "shell", path: "provisioners/nginx-install.sh"
  end
```

The two boxes will be on the same private network with a different fixed IP address.
- The "db" box will be based on ubuntu 16.04, with 512MB of memory, a fixed IP address "192.168.33.20" and the provisioner script to install mongo will be launched on boot.
- The "web" box will also be based on ubuntu 16.04, with 512MB of memory, a fixed IP address "192.168.33.10", a forwarded port from port 80 on the guest to the port 8080 on the host and the provisioner script to install nginx will be launched on boot.


We also comment the original line:
```
config.vm.box = "bento/ubuntu-16.04"
```
Now we can launch the vagrant environment by doing:
>vagrant up 

By doing "vagrant status" we can see that we have two virtual machines running.

Working this way, you have two boxes ("web" & "db") running in one Vagrant environment. If you want to connect to those boxes through SSH, the commands change a little bit by including the name of the box:
- To connect to the "web" box:
>vagrant ssh web

- To connect to the "db" box:
>vagrant ssh db

You'll have to work the same way if you want to only boot one of the two boxes separately (vagrant up web or vagrant up db).

On the host, we can also see that nginx is working well by going to localhost:8080 on the browser.

## Create your own box

You can sign up for a Vagrant cloud account, which is free, and upload your own public boxes. If you want to store private boxes, you can sign up for paid account tiers. Vagrant cloud is not the only option to store boxes, you can set up your own private store with a simple web server for a private company network for example.

The starter application you could add to the based box could be added by a provisioner. However, that provisioner take time to run and the first boot of a development environment could take 30 minutes or more. If a software should be part of the box from the beginning, it's better a create a custom box with the concerned software.

We'll demonstrate the creation of our own box into another Vagrant environment "box_creation".
In my case, I want to package a box with nginx already installed. 

As usual, we begin by initializing our Vagrantfile:

>vagrant init bento/ubuntu-16.04

We also parameter the networking forwarding in order to see directly the result on the host and the provider to install nginx:

```
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
```
```
config.vm.provision "shell", path: "provisioners/nginx-install.sh"
```

And now we launch our box with:
>vagrant up

Now, we have a box running with nginx and the port forwarding allows us to see it on the host on port 8080.
This box is ready and we want to package it with the Vagrantfile. 
To do so, we need to suppress the modifications we added to this Vagrantfile (with the provisioner). 
In fact, if we package a box already set with nginx (provisioner) and we keep the same Vagrantfile (with the nginx provisioner), it will install nginx a second time, and we don't want that (we'll lose a precious time). That's why we suppress the modifications we added on th provider we only keep the lines:

```
config.vm.box = "bento/ubuntu-16.04"
```
```
config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
```

Now we are ready to go and prepare our custom box. Vagrant includes a command that can produce a new base box:
>vagrant package

The package command takes an existing box and packages it as a new base box.

To create a custom box, the vagrantfile in the folder (without the modifications) will be packaged with the box and will be part of the vagrantfile load order.
Now, we have to enter the command: 

>vagrant package --vagrantfile Vagrantfile --output custom.box

- The "--vagrantfile" parameter instructs Vagrant to include the Vagrantfile indicated in the path.
- The "--output" parameter indicates the name of the file to create.

When the packaging process is finished, we'll see the custom.box file in the root of the environment directory.

The next step is to add this box to the local box cache. With the command "vagrant box list", we can list all the boxes in the local host box cache.
Now we add our custom box the box list with the command (first parameter: name we want to give, second parameter: name of the box we packaged):

>vagrant box add custom custom.box

Now our box is added to the local cache (you can verify it with "vagrant box list"). 
Also, if your repository is synchronized with github, don't forget to add the box you created into a .gitignore, otherwise you could accidentally upload it to github.

Now if you create another vagrant environment (in another folder), you can use this box (added to the local cache) when you initialize your vagrant environment:
>vagrant init custom

## Upload a box to Vagrant Cloud

You can upload your custom box to Vagrant Cloud and let it be available for the public or for a private group. 
You have to:
- Create an account on Vagrant Cloud,
- From the Dashboard, click on New Vagrant Box,
- The organization name will be your account name and then you have to name your box on Vagrant Cloud, 
- If you want to let your box private, you can do so but it requires a paid Vagrant Cloud account,
- Enter a short description about the box, 
- Click on "create your box",
- Version your box (even if it's the first release),
- Enter a short description about the version of the box
- And click on "create version".

The box is now created on Vagrant Cloud but it has no providers and is not released. The next step is to create a box file for upload.
For the next steps, we have to:
- Add a provider,
- Select VirtualBox as a provider,
- Select "Upload to Vagrant Cloud" for File Hosting,
- Then, after clicking on "Choose File", we select the custom.box we created (or any other box you created on your host) to upload it (the uploading could take time depending on your internet connection).

The box now has a provider with an associated box file. Now we have to release this version by clicking on "Realease".
The box is now published to the public Vagrant Cloud.

I personnaly did it on Vagrant Cloud, you can see it at this [URL](https://app.vagrantup.com/liquidateur/boxes/custom/versions/0.0.1).

We can give a try to it. The box is available at "liquidateur/custom" (account name and name of the box). You can initialize your Vagrant environment with this box by typing:
>vagrant init liquidateur/custom

It will create a Vagrantfile and now you can boot it with:
>vagrant up

The vagrant up command will locate the box on Vagrant Cloud, downloa it and add it to the local box cache (you can see it with vagrant box list).

# Kill a process

Rarely, a vagrant process could freeze (sometimes because of the provisioner). You can kill the process with the following command:

>pkill vagrant
or
>pkill ruby

# Go back to a previous state

You can save a current state of a box, in case your future developments could affect the box.
If the VM brakes, you can go back.
To do so, you can save the current state of a box with the command:
>vagrant snapshot save name_of_the_state

Then, in the future, you can restore the snapshot and go back to this state by doing:
>vagrant snapshot restore name_of_the_state

Keep in mind that each snapshot takes up some space on the hard drive.

# Do more

Vagrant is extensible and also has plugins for custom-synced folder capabilities, DNS, and many others. There is a list on Github where you can see all the HashiCorp and third party plugins available for Vagrant.

# Resources 

I managed this repo using VS Code wih Vagrantfile Support extension (syntax highlighting for Ruby). 

# Bibliographie

# SSH Trick

First of all, be sure that you set a private network with an IP address in the vagrantfile.
If you want to connect to the Vagrant box through SSH without having to be inside the vagrant environment, you can do it with these steps:

- Go the concerned vagrant environment and type:
> vagrant ssh-config 

It will print the configuration used by the vagrant environment, for example:
```
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2200
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/aey1/Sites/demo/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

We will copy this configuration inside a "config" file on the host (~/.ssh/config). But before that, we have to replace the IP address with the private IP address and change the configuration with this output:
```
Host 192.168.50.4
  User vagrant
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/aey1/Sites/demo/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

Now, in this case, you can connect to the Vagrant box with the command:
>ssh 192.168.50.4