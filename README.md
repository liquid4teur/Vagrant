# Presentation 

Vagrant is a tool for building and managing virtual machine environments in a single workflow. It provides easy to configure, reproducible and portable work environments. Vagrant stands on the shoulders of providers like VirtualBox, VMWare, AWS and others. In our case, we'll use Vagrant and VirtualBox as a provider. 

For this demonstration, we are going to use Ubuntu 16.04. 

# Installation of Vagrant 

You have to go to the Vagrant [download page](https://www.vagrantup.com/downloads.html) and download the version of Vagrant depending on the OS you are running. 

Once you installed Vagrant, when running your command prompt you can see the different commands you can use through Vagrant: 

>vagrant list-commands 

# Configuration 

We first initialize a vagrant environment. We need an Ubuntu 16.04 environment and we have VirtualBox installed: 

- We have go the Vagrant box [page](https://app.vagrantup.com/boxes/search), 
- Then we searche the box we need, in our case we need an Ubuntu 16.04 environment working with a VirtualBox provider, 
- We see that ubuntu/trusty64 box matches and is the most downloaded. 

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

# To be developed 

gitignore 

shared folder 

rename vm (web_1..)

command installed on mac 
- Brew http://macappstore.org/rename/
- Rename -> brew install rename 
- wget -> brew install wget 