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

In order to initialize our vagrant environment, we need to execute the following command: 

>vagrant init ubuntu/trusty64

The execution of this command creates a file called "Vagrantfile". Afterward, this file could be modified depending on the precise configuration you want. 

After the installation of Vagrant, we have to configure a provider. VirtualBox is installed in my laptop. So the first launch of the environment, I set the provider with VirtualBox:

>vagrant up --provider=virtualbox

It will automatically find, download (on vagrantcloud) and install the box. The box is added for the provider (VirtualBox).




gitignore 

shared folder 

rename vm (web_1..)

command installed on mac 
- Brew http://macappstore.org/rename/
- Rename -> brew install rename 
- wget -> brew install wget 