# Presentation 

Vagrant is a tool for building and managing virtual machine environments in a single workflow. It provides easy to configure, reproducible and portable work environments. Vagrant stands on the shoulders of providers like VirtualBox, VMWare, AWS and others. In our case, we'll use Vagrant and VirtualBox as a provider. 

For this demonstration, we are going to use Ubuntu 16.04. 

# Installation of Vagrant 

You have to go to the Vagrant [download page](https://www.vagrantup.com/downloads.html) and download the version of Vagrant depending on the OS you are running. 

Once you installed Vagrant, when running your command prompt you can see the different commands you can use through Vagrant: 

>vagrant list-commands 

# Configuration 

After the installation of Vagrant, we have to configure a provider. VirtualBox is installed in my laptop, so I have to set the provider with VirtualBox:

>vagrant up --provider=virtualbox