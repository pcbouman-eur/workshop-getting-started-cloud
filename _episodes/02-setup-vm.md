---
title: "Setting up a machine in the cloud"
teaching: 10
exercises: 0
questions:
- "What settings should you consider for my virtual machine"
- "What options are there to connect to and control your virtual machine"
objectives:
- "Learn some historic developments that lead to cloud computing"
- "Understand the main drivers of cloud computing"
- "Know typical use cases of cloud computing relevant for your work"
keypoints:
- "Explain concepts of centralization, outsourcing and virtualization"
- "Discuss different cloud service models such as IaaS and SaaS"
- "Name the important cloud service providers such as Amazon and Microsoft"
---

> ## You do not need to perform these steps during this workshop!
>
> The steps in this lesson are only meant to discuss how you can start up
> your own virtual machine. In this lesson, your instructor will have done
> these already, and just provide you with a username and password that
> you can work with. The reason we still discuss it is to give you an
> idea of what needs to be done to start up the computer.
{: .prereq }

As a first step, we will start a Virtual Machine in the cloud. In this example
we will use Microsoft Azure, but if you use another provider (e.g. Amazon, Google)
the procedure will be similar.

## Starting a new Virtual Machine

After you set up an account (and possibly configure how billing works), most cloud providers
have some portal or dashboard. There you can typically find some option to create a new
Virtual Machine (or whatever the service is called by your cloud provider of choice).

<figure>
    <img src="../fig/azure-create-vm.png" alt="Creating a new VM from the Azure Portal">
    <figcaption>Creating a VM from the Azure portal
    </figcaption>
</figure>

When you want to start a new instance, there are typically a number of things you need
to choose and configure. Typical things you can choose are:

* The type of machine, in particular how many (v)CPU's are available and how much RAM
* Which image or operating system is used to create the machine.
* (With big providers) in which part of the world the server should be hosted. If possible, try to minimize geographic distance!

The number of vCPU's typically determines how many computations you can run in parallel.
Most current day laptop have 2 or 4 CPU cores, an 8GB to 16GB of RAM. However, if your
program is unable to make use of multiple cores (as most programs are), having a single
vCPU would be sufficient unless you want to run the same program multiple times in parallel.

Furthermore, you typically also can assign a name to the Virtual Machine, and link
it to some billing account. In the Azure portal, choose which type of Virtual Machine
can be done under the *size* setting:

<figure>
    <img src="../fig/azure-create-vm-2.png" alt="Configure the properties of a new VM">
    <figcaption>Configuring the properties of a new VM in Azure</figcaption>
</figure>

With Azure, you can choose between *Windows Server* or *Linux* based virtual machine.
In this workshop, we will go with Linux.

> ## Which Operating System to Use
>
> If you are familiar with Windows, and want to run a program that you use on your
> own Windows computer, you can consider using a Windows VM. Remote control of such
> virtual machines usually happens via *Remote Desktop*, which behaves very similar
> to working on a local Windows computer. However, Windows Virtual Machines are 
> more expensive due to licensing costs, not all cloud providers offer them and 
> for servers, Linux is in general more popular for servers. There are estimates
> that over 90% of the servers in the world and in cloud computing run Linux,
> and even on Microsoft's Azure cloud platform Linux virtual machines are more
> popular than Windows virtual machines.
> 
> There are many different Linux based operating systems, often called distributions.
> For server computing, some notable ones are Ubuntu, Debian, Red Hat, CentOS an SUSE.
> For beginners, Ubuntu and Debian are perhaps easier to start with. As Ubuntu offers
> a bit more recent packages, whereas Debian favors stability, we will work with
> Ubuntu in this lesson. However, almost all the things you learn will be applicable
> in any kind of Linux/UNIX system. The major differences turn up when you want to
> install software. Ubuntu and Debian use that `apt` package manager, whereas Red Hat
> and CentOS use the `yum` package manager. Package managers are like app stores:
> they make it really easy to install new software on a system.
{: .callout }

The next step is to set up a user. This will typically be an administrator account
that we can use to connect to and manage the machine. We also want to enable SSH
access to the machine (port 22 should be open, and not blocked by a firewall),
so we can connect to the machine and login as this user.

<figure>
    <img src="../fig/azure-create-vm-3.png" alt="Configuring an administrative user">
    <figcaption>Configuring an administrative user for the Virtual Machine</figcaption>
</figure>

> ## Security is important!
>
> Please take care in securing your virtual machine, even if you only use it for simple
> computations. For hackers and criminals, controlling machines can help them do all
> kinds of malicious things, and if this happens on a virtual machine you have created
> this can cause you trouble. Therefore, you should always us a strong, unique
> password for an account with administrator privileges and keep that password safe.
> It would be even better to consider using *private key authentication*, if possible.
{: .caution }

Finally, your virtual machine needs storage space (the virtual equivalent of a hard drive).
Unless you need to store vast amounts of data, you can typically stick with a default amount
(on a Linux system something like 20GB should be sufficient for many use cases). Typically,
we can also add additional storage, but for this lesson we do not need to and stick to
the default.

<figure>
    <img src="../fig/azure-create-vm-4.png" alt="Configuring storage on the Virtual Machine">
    <figcaption>Configuring storage on the Virtual Machine</figcaption>
</figure>

Once we have configured CPU's, memory, storage, the operating system and a standard user,
we are ready to go. Review other settings, but you will probably be fine with the defaults.
At the end, you will problably have to confirm that you want to start up the Virtual Machine.

<figure>
    <img src="../fig/azure-create-vm-5.png" alt="Confirm the creation of the Virtual Machine">
    <figcaption>Confirm the creation of the Virtual Machine</figcaption>
</figure>

It may take a little bit of time before the machine is configured at has started, but at some
point you should get a notification that your machine is ready to use.

<figure>
    <img src="../fig/azure-create-vm-6.png" alt="Our Virtual Machine is ready to use!">
    <figcaption>Our Virtual Machine is ready to use!</figcaption>
</figure>

Now that our Virtual Machine is started, we should connect to it. Typically, when we
navigate to the Virtual Machine in the portal of the cloud provider, we should see some
information needed to connect to it. In Azure, there is a nice landing page that contains
multiple option, include a *Connect* button that will give you more information on how
to connect, the option to *Stop* or *Delete* the virtual machine, as well as the IP address
of the virtual machine.

<figure>
    <img src="../fig/azure-create-vm-7.png" alt="Landing page of the virtual machine with management options.">
    <figcaption>Landing page of the virtual machine</figcaption>
</figure>

The *IP-adress* is important information that will allow you to connect to the remote
computer. In case you created a Windows based virtual machine, you can enter this into
a *Remote Desktop Connection* (RDP) client to connect to the computer. In case of a Linux
virtual machine, you need to this connect to enter this into a *Secure Shell* (SSH) client
to connect and control the computer.

In the next episode we use this information to connect to the virtual machine.
{% include links.md %}

