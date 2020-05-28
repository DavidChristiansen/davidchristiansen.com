---
ID: 627
post_title: 'HowTo: Run Hyper-V/Docker on Hyper-V Virtual Machine'
post_name: run-hyper-v-docker-on-virtual-machine
author: David
post_date: 2016-11-17 14:18:27
layout: post
link: >
  https://blog.davidchristiansen.com/2016/11/run-hyper-v-docker-on-virtual-machine/
published: true
tags: [ ]
categories:
  - Development (General)
---
TL:DR; On the Hyper-V host run <span class="lang:c# decode:true crayon-inline">Set-VMProcessor -VMName "&lt;Your Virtual Machine Name&gt;" -ExposeVirtualizationExtensions $true</span>

<!--more-->

If you are like me, you may run all your Dev Environments from within Virtual Machines, leaving your base OS install as clean as possible - albeit with MS Office, Browsers etc. installed.

In recent projects I've been working with containerisation of system deployments with Docker. This has required me to have docker installed on my Virtual Machines.  In order to run docker on Windows 10, i've needed to have Hyper-V installed... Hyper-V running on a VM !?  Yup, it's possible!
<h2>Prerequisites</h2>
<ul>
 	<li>A Hyper-V host running Windows Server 2016 or Windows 10 Anniversary Update;</li>
 	<li>A Hyper-V VM running Windows Server 2016 or Windows 10 Anniversary Update;</li>
 	<li>A Hyper-V VM with configuration version 8.0 or greater;</li>
 	<li>An Intel processor with VT-x and EPT technology.</li>
</ul>
<h2>Configure Nested Virtualisation</h2>
<ol>
 	<li>Create a virtual machine.</li>
 	<li>While the virtual machine is in the OFF state, run the following command on the physical Hyper-V host. This enables nested virtualisation for the virtual machine.
<span class="lang:ps decode:true crayon-inline">Set-VMProcessor -VMName "&lt;Your Virtual Machine Name&gt;" -ExposeVirtualizationExtensions $true</span></li>
 	<li>Start the virtual machine.</li>
 	<li>Install Hyper-V within the virtual machine, just like you would for a physical server.</li>
</ol>