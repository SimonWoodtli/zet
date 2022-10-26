# What are Docker Containers?

## What is Docker doing?

Traditional virtualization in Virtual Machines are virtualizing the hardware. However Docker virtualizes the operating system. Meaning with Docker you have a Host OS that has Docker installed. And Docker can now install containers. 

Containers are used to create an environment that your application needs to run, be tested and deployed. But that environments can be a different OS to the Dockers Host OS. Meaning you can have an Ubuntu Host OS and a Docker container Arch Linux OS with a specific environment and all the dependencies installed that your applications needs, but only that. So Docker containers are often minimalistic environments to run a specific app and not a full blown OS with systemd and all that jazz. But you can also configure a container to be a full blown OS.

## What are Containers?

Crazy fast light-weight micro computers. But they can be as powerful as you want them to be.

They have:
* own Operating System
* own CPU
* own RAM
* own Network
* and most importantly they are totally isolated
* Name Spaces
* Control Groups

## Why are Containers lightweight and wicked fast?

It only requires one Kernel. The Host OS is running Linux and shares the Kernel with the Containers. Since the Kernel of the Host OS is already up and running it is super fast to startup a container. So Containers are lightweight because they don't need more than one Linux Kernel.

On the other hand if you use a Virtual Machine the VM, while booting, spins up its own Linux Kernel. If you use a type 2 Hypervisor you might have 2 Kernels running one from the Host OS and another one from the VM.

### Limitations

* You can only run Linux Containers if the Host OS is Linux.
* You can only run Windows Containers if the Host OS is Windows.
* You could sort of bypass that with 'Docker Desktop' which creates a Linux VM in the background to have Linux Containers available on Windows

Related:

* [20221026000728](/20221026000728/) 🖼️  Comparison: Docker and VM

Tags:

    #linux #containers #docker #virtualization
