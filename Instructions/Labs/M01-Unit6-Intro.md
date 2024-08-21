# Getting Started with Your AZ-700: Designing and Implementing Microsoft Azure Networking Solutions Workshop

## Overall Estimated Duration: 4 Hours

## Overview

In this hands-on lab, participants will configure DNS settings in Azure by following a structured process. First, you'll create a private DNS zone, which allows for internal domain name resolution within a virtual network. Next, you'll link this DNS zone to a subnet, enabling virtual machines (VMs) within the subnet to automatically register their hostnames with the DNS zone. Afterward, you'll create VMs to test the DNS configuration and ensure that the auto-registration is functioning correctly. Finally, you'll verify that the DNS records for the VMs are present in the private DNS zone, confirming that the configuration is working as expected. This lab provides practical experience in managing private DNS zones in Azure, essential for secure and efficient network operations in a cloud environment.

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

Welcome to your AZ-700: Designing and Implementing Microsoft Azure Networking Solutions workshop! We've prepared a seamless environment for you to explore and learn about planning, implementing, and managing Azure networking solutions. Let's begin by making the most of this experience:
 

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
