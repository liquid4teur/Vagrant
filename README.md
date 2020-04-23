# Presentation 

Vagrant is a tool for building and managing virtual machine environments in a single workflow. It provides easy to configure, reproducible and portable work environments. Vagrant stands on the shoulders of providers like VirtualBox, VMWare, AWS and others. In our case, we'll use Vagrant and VirtualBox as a provider. 

For this demonstration, we are going to use Ubuntu 16.04. 

# Installation of Vagrant 

You have to go to the Vagrant [download page](https://www.vagrantup.com/downloads.html) and download the version of Vagrant depending on the OS you are running. 

Once you installed Vagrant, when running your command prompt you can see the different commands you can use through Vagrant: 

>vagrant list-commands 

**Warning**: If you wish to use VirtualBox on Windows, you must ensure that Hyper-V is not enabled on Windows. You can turn off the feature by running this Powershell command:

>Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All

You can also disable it by going through the Windows system settings:

- Right click on the Windows button and select ‘Apps and Features’.
- Select Turn Windows Features on or off.
- Unselect Hyper-V and click OK.
- You might have to reboot your machine for the changes to take effect.

Also, VirtualBox Guest Additions must be installed so that things such as shared folders can function. Installing guest additions also usually improves performance since the guest OS can make some optimizations by knowing it is running within VirtualBox.

# Configuration 

We first initialize a vagrant environment. We need an Ubuntu 16.04 environment and we have VirtualBox installed: 

- We have go the Vagrant box [page](https://app.vagrantup.com/boxes/search), 
- Then we searche the box we need, in our case we need an Ubuntu 16.04 environment working with a VirtualBox provider, 
- We see that bento/ubuntu-16.04 box matches and is the most downloaded. 

Note that you have to execute the vagrant commands where the vagrant environment is set. 

# Initialization 

In order to initialize our vagrant environment, we need to execute the following command: 

>vagrant init ubuntu/trusty64

The execution of this command creates a file called "Vagrantfile". Afterward, this file could be modified depending on the precise configuration you want. 

# Launch the VM

After the installation of Vagrant, we have to configure a provider. In fact, Vagrant lauches the VM with the help of a provider. VirtualBox is installed on my laptop. So for the first launch of the environment, I set the provider with VirtualBox:

>vagrant up --provider=virtualbox

It will automatically find, download (on vagrantcloud) and install the box. The box is added for the provider (VirtualBox).

You can see the status of the running VM by doing:

>vagrant status 

**Warning**: Vagrant create a ".vagrant" folder. Make sure you specified the .vagrant folder in the .gitignore file. In fact, if you upload the .vagrant folder, it will let your repository be heavier and sometimes it could "break" it. You don't need this folder in your repository, the vagrantfile is enough (it contains all the needed information to initialize a VM). 

# Connection to the VM

Now that your VM is running, you can directly connect to it through SSH with the command:

>vagrant ssh 

To exit the ssh session, simply execute the command:

>exit 

# Managing the VM 

You can suspend your VM with the command:

>vagrant suspend 

You can shutdown your VM forcefully with the command: 

>vagrant halt 

If you want to restart your VM, you can do it with the command: 

>vagrant up

# Managing the configuration 

The VirtualBox provider exposes some additional configuration options that allow you to more finely control your VirtualBox-powered Vagrant environments:

- You can customize the name that appears in the VirtualBox GUI by setting the name property:

```
config.vm.provider "virtualbox" do |vb|
  vb.name = "my_vm"
end
```

- Memory & CPU settings 

```
config.vm.provider "virtualbox" do |v|
  v.memory = 1024
  v.cpus = 2
end
```

- The network can also be managed, whether you need to put your VM on a private or public network, through a static or dynamic IP address. 

# To be developed 

shared folder 