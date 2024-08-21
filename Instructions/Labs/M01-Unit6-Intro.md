# Getting Started with Your AZ-700: Designing and Implementing Microsoft Azure Networking Solutions Workshop
---
## Overall Estimated Duration: 40 Minutes

## Overview

In this hands-on lab, you’ll configure DNS settings in Azure. You’ll learn to create a private DNS zone for internal domain name resolution, link it to a subnet, and enable virtual machines (VMs) to auto-register their hostnames. By the end of the lab, you’ll have practical experience in managing private DNS zones, essential for secure and efficient network operations in Azure.

## Objective

   - **Create a Private DNS Zone** : Set up a private DNS zone in Azure to manage internal domain name resolution within a virtual network.
   - **Link Subnet for Auto Registration** : Connect the private DNS zone to a subnet, enabling automatic hostname registration for VMs in that subnet.
   - **Create Virtual Machines to Test the Configuration** : Deploy virtual machines within the linked subnet to test the DNS configuration and auto-registration.
   - **Verify Records are Present in the DNS Zone** : Check that the DNS records for the created VMs are correctly registered in the private DNS zone.

## Pre-requisites

1. **Basic Understanding of DNS Concepts** : Familiarity with DNS fundamentals, including DNS zones, records, and name resolution.
1. **Experience with Azure Portal** : Ability to navigate and manage resources within the Azure Portal.
1. **Knowledge of Virtual Networks in Azure** : Understanding of how virtual networks and subnets function within Azure.
1. **Prior Experience with Virtual Machines** : Experience creating and managing virtual machines in Azure.


## Architecture

In this hands-on lab, the architecture diagram illustrates a DNS configuration within a virtual network in Azure. The setup is part of the ContosoResourceGroup located in the East US region. It includes a Private DNS Zone named contoso.com, which is linked to a virtual network (CoreServicesVnet) using a VNetLink. This virtual network has a CIDR block of 10.20.0.0/16 and contains a subnet called DatabaseSubnet with a CIDR block of 10.20.20.0/24. Within this subnet, two virtual machines, TestVM1 and TestVM2, are deployed, with IP addresses 10.20.20.4 and 10.20.20.5, respectively. The DNS zone enables internal name resolution, allowing these VMs to automatically register their hostnames (TestVM1.contoso.com and TestVM2.contoso.com) within the DNS zone, ensuring seamless name resolution across the network.

## Architecture Diagram

   ‎![](../media/az700-m1-unit6.png)

## Explanation of Components

1. **Azure Resource Group** : The Resource Group is a logical container in Azure that holds related resources for an Azure solution. It helps organize and manage resources like virtual networks, DNS zones, and virtual machines in a single, manageable group.
1. **Azure Private DNS Zone** : The Private DNS Zone (contoso.com) is a service that enables you to manage and resolve domain names within a virtual network without exposing them to the public internet. It allows resources like virtual machines to automatically register their hostnames, providing seamless internal name resolution.
1. **Azure Virtual Network (VNet)** : The Virtual Network (CoreServicesVnet) is a fundamental building block in Azure, enabling secure communication between different Azure resources. It provides an isolated environment where resources like virtual machines can interact securely within the specified address range (10.20.0.0/16).
1. **Azure Subnet** : The Subnet (DatabaseSubnet) is a subdivision of the virtual network, defined by a smaller CIDR block (10.20.20.0/24). Subnets allow for more granular control of network settings and segmentation within the VNet, enabling better management and security of resources.
1. **Azure Virtual Machines (VMs)** : Virtual Machines (TestVM1 and TestVM2) are scalable compute resources in Azure. These VMs are deployed within the DatabaseSubnet and are automatically configured to register their internal IP addresses with the Private DNS Zone, allowing for internal name resolution within the virtual network.

## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and lab guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](../media/labguide-1.png)

### Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment Details** tab.
 
![Explore Lab Resources](../media/env-1.png)
 
## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the Top right corner.
 
![Use the Split Window Feature](../media/spl.png)
 
## Managing Your Virtual Machine
 
Feel free to start, stop, or restart your virtual machine as needed from the **Resources** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](../media/res.png)

## **Lab Duration Extension**

1. To extend the duration of the lab, kindly click the **Hourglass** icon in the top right corner of the lab environment. 

    ![Manage Your Virtual Machine](../media/gext.png)

    >**Note:** You will get the **Hourglass** icon when 10 minutes are remaining in the lab.

2. Click **OK** to extend your lab duration.
 
   ![Manage Your Virtual Machine](../media/gext2.png)

3. If you have not extended the duration prior to when the lab is about to end, a pop-up will appear, giving you the option to extend. Click **OK** to proceed.
 
## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
 
   ![Launch Azure Portal](../media/sc900-image(1).png)

2. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
       ![Enter Your Username](../media/sc900-image-1.png)
 
3. Next, provide your password:
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>
 
     ![Enter Your Password](../media/sc900-image-2.png)
 
4. If prompted to stay signed in, you can click "No."
 
5. If a **Welcome to Microsoft Azure** pop-up window appears, simply click **Cancel**.
 
6. Click "Next" from the bottom right corner to embark on your Lab journey!
 
   ![Start Your Azure Journey](../media/sc900-image(3).png)
 
Now you're all set to explore the powerful world of technology. Feel free to reach out if you have any questions along the way. Enjoy your workshop!
