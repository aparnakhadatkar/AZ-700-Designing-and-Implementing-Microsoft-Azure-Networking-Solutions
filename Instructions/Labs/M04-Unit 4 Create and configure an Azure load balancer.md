# Module 04-Unit 4 Create and configure an Azure load balancer

## Lab scenario 
In this lab, you will create an internal load balancer for the fictional Contoso Ltd organization.

**Note:** An **[interactive lab simulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Create%20and%20configure%20an%20Azure%20load%20balancer)** is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

The steps to create an internal load balancer, are very similar to those you have already learned about in this module, to create a public load balancer. The key difference is that with a public load balancer the front end is accessed via a public IP address, and you test connectivity from a host which is located outside your virtual network; whereas, with an internal load balancer, the front end is a private IP address inside your virtual network, and you test connectivity from a host inside the same network.

 
## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create the virtual network
+ Task 2: Create backend servers
+ Task 3: Create the load balancer
+ Task 4: Create load balancer resources
+ Task 5: Test the load balancer

## Estimated time: 60 minutes

## Architecture diagram

![internal standard loadbalancer diagram](../media/exercise-internal-standard-load-balancer-environment-diagram.png)

## Task 1: Create the virtual network

In this section, you will create a virtual network and a subnet.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual networks(1)**, and then select **Virtual networks(2)** under services.

    ![](../media/VN.png)

1. Select **+ Create** on the Virtual networks page.  

1. On the **Basics** tab, use the information in the table below to create the virtual network.

   | **Setting**    | **Value**                                  |
   | -------------- | ------------------------------------------ |
   | Subscription   | Select your subscription                   |
   | Resource group | Select **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/>** |
   | Name           | **IntLB-VNet**                                                      |
   | Region         | **<inject key="Region" enableCopy="false"/>**                   |

1. Select **Next** on the **Security** tab, under **Azure Bastion** select **Enable Azure Bastion**, then enter the information from the table below.

    | **Setting**                       | **Value**                                     |
    | --------------------------------- | --------------------------------------------- |
    | Azure Bastion host name           | **myBastionHost**                             |
    | Azure Bastion public IP address   | Select **Create a public IP address**  Name: **myBastionIP** |

1. Select **OK**.

1. Select **Next**.
   
1. On the **IP Addresses** tab, in the **IPv4 address space** box, don't remove the default, click on **Add IPV4 address space**, in new **IPV4 address space**, enter **10.1.0.0** in address space and **/16** in size field and select **+ Add a subnet** in the new IPv4 address space.

   ![](../media/L4U4-1.png)

1. On **Add a subnet** blade, specify the following and select **Add**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Name                         | **myBackendSubnet** |
   | Starting address             | **10.1.0.0**        |
   | Size                         | **/24**             |
   |||

    ![](../media/mod4-u4-2.png)
   
1. On the Create virtual network of **IP address** tab select **+ Add a subnet** in the new IPv4 address space. On **Add a subnet** blade specify the following and select **Add**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Name                         | **myFrontEndSubnet** |
   | Starting address             | **10.1.2.0**        |
   | Size                         | **/24**             |
   |||

   ![](../media/mod4-u4-3.png)
   
1. Select **Review + create**.

1. Select **Create**.

1. It will take 10-15 minutes to complete the deployment.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
   <validation step="f3f7612a-945c-453b-87cc-9ac4592a00cc" />

## Task 2: Create backend servers

In this section, you will create three VMs, that will be in the same availability set, for the backend pool of the load balancer, add the VMs to the backend pool, and then install IIS on the three VMs to test the load balancer.

1. On the Azure portal select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

   ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account** then under **Storage account subscription** select your available subscription from the dropdown and click on **Apply**.
   
     ![](../media/pwershell3.png)

1. Within the Mount storage account pane, select **I want to create a storage account** and click **Next**.

     ![](../media/pwershell4.png)
       
1. Please make sure you have selected your resource group **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/>** then select **Region** **<inject key="Region" enableCopy="false"/>** and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account name** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** for the  **File share** , then click on **Create**.
    
1. On the toolbar of the Cloud Shell pane, select the Select **Manage files** icon, in the drop-down menu, select **Upload** and upload the following files **azuredeploy.json**, **azuredeploy.parameters.vm1.json**, **azuredeploy.parameters.vm2.json** and **azuredeploy.parameters.vm3.json** from **C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M04** folder into the Cloud Shell home directory one by one.

     ![](../media/pwershell2.png)

1. Deploy the following ARM templates to create the VMs needed for this exercise:

   ```powershell
   $RGName = "IntLB-RG-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm1.json
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm2.json
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.vm3.json
   ```

 1. You will be prompted to provide an admin password, provide adminPassword: **Pa55w.rd!!**.

 1. It may take 20-25 mins to create these three VMs. Please wait until this job completes, and you will be prompted to provide password three times for each VM deployment.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
    <validation step="3107853e-92fb-4898-a59b-a7cb10f5051a" />  

## Task 3: Create the load balancer

In this section, you will create an internal Standard SKU load balancer. The reason we are creating a Standard SKU load balancer here in the exercise, instead of a Basic SKU load balance, is for later exercises that require a Standard SKU version of the load balancer.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Load Balancer**, and then select **Load Balancer** under services.

1. Select **+ Create** on **Load balancing | Load Balancer** page.

1. On the **Basics** tab, use the information in the table below to create the load balancer.

   | **Setting**           | **Value**                |
   | --------------------- | ------------------------ |
   | Subscription          | Select your subscription |
   | Resource group        | **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/>**             |
   | Name                  | **myIntLoadBalancer**    |
   | Region                | **<inject key="Region" enableCopy="false"/>**         |
   | SKU                   | **Standard**             |
   | Type                  | **Internal**             |

1. Select **Next: Frontend IP configuration>** and click on  **+ Add a frontend IP configuration**.

1. On the **Add frontend IP configuration** blade, enter the information from the table below and select **Save**.
 
   | **Setting**     | **Value**                |
   | --------------- | ------------------------ |
   | Name            | **LoadBalancerFrontEnd** |
   | IP version      | **IPv4**                 |
   | Virtual network | **IntLB-VNet**           |
   | Subnet          | **myFrontEndSubnet**     |
   | Assignment      | **Dynamic**              |
   
1. Select **Review + create** and  **Create**. Wait for deployment to complete successfully.

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
    <validation step="0aa25baa-7ec4-49ad-a99e-43e629157f51" />

## Task 4: Create load balancer resources

In this section, you will configure load balancer settings for a backend address pool, then create a health probe and a load balancer rule.

### Task 4.1: Create a backend pool and add VMs to the backend pool

The backend address pool contains the IP addresses of the virtual NICs connected to the load balancer.

1. On the Azure portal, from top left corner of page click **Show portal menu** and select **All resources**, then select on **myIntLoadBalancer** from the resources list.

    ![](../media/unit4-image5.png)

1. On **myIntLoadBalancer** blade, from the left navigation menu, under **Settings** section, select **Backend pools**, and then select **+ Add**.

1. On the **Add backend pool** page, enter the information from the table below.

   | **Setting**     | **Value**            |
   | --------------- | -------------------- |
   | Name            | **myBackendPool**    |
   | Virtual network | **IntLB-VNet**       |

1. Under **IP configurations**, select **+ Add**.

    ![](../media/az7001.png)

1. Select the checkboxes for all 3 VMs (**myVM1**, **myVM2**, and **myVM3**), then select **Add**.

    ![](../media/az7002.png)

1. Select **Save**.

   ![Picture 7](../media/add-vms-backendpool.png)
   
### Task 4.2: Create a health probe

The load balancer monitors the status of your app with a health probe. The health probe adds or removes VMs from the load balancer based on their response to health checks. Here you will create a health probe to monitor the health of the VMs.

1. From **myIntLoadBalancer | Backend pools** blade, under **Settings** section, select **Health probes**, then select **+ Add**.

1. On the **Add health probe** page, enter the information from the table below.

   | **Setting**         | **Value**         |
   | ------------------- | ----------------- |
   | Name                | **myHealthProbe(1)** |
   | Protocol            | **HTTP(2)**          |
   | Port                | **80(3)**            |
   | Path                | **/(4)**             |
   | Interval            | **15(5)**            |

   ![Picture 7](../media/az70012.png)
   
1. Select **Save**.
 
### Task 4.3: Create a load balancer rule

A load balancer rule is used to define how traffic is distributed to the VMs. You define the frontend IP configuration for the incoming traffic and the backend IP pool to receive the traffic. The source and destination port are defined in the rule. Here you will create a load balancer rule.

1. From the **myIntLoadBalancer | Health probes** page of your load balancer, under **Settings** section, select **Load balancing rules**, then click on **+ Add**.

   ![Picture 7](../media/az7004.png)

1. On the **Add load balancing rule** page, enter the information from the table below.

   | **Setting**            | **Value**                |
   | ---------------------- | ------------------------ |
   | Name                   | **myHTTPRule(1)**           |
   | IP Version             | **IPv4(2)**                 |
   | Frontend IP address    | **LoadBalancerFrontEnd(3)** |
   | Backend pool           | **myBackendPool(4)**       |
   | Protocol               | **TCP(5)**                  |
   | Port                   | **80(6)**                   |
   | Backend port           | **80(7)**                   |
   | Health probe           | **myHealthProbe(8)**        |
   | Session persistence    | **None(9)**                 |
   | Idle timeout (minutes) | **15(10)**                   |
   | Enable Floating IP     | **Disabled(11)**             |

   ![Picture 7](../media/az7005.png)

   ![Picture 7](../media/az7006.png)

1. Select **Save(12)**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
   <validation step="725af5b8-9a2d-4aeb-9adb-da47aa36d4aa" /> 
 
## Task 5: Test the load balancer

In this section, you will create a test VM, and then test the load balancer.

### Task 5.1: Create test VM

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual machines**, and then select **Virtual 
   machines** under services.

1. Select **+ Create** and choose **Azure virtual machine**. On the **Create a virtual machine** page, on the **Basics** tab, use the information in the table below to create the first VM.

    | **Setting**          | **Value**                                    |
    | -------------------- | -------------------------------------------- |
    | Subscription         | Select your subscription **(1)**                     |
    | Resource group       | **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**        |
    | Virtual machine name | **myTestVM(3)**                                 |
    | Region               |  **<inject key="Region" enableCopy="false"/> (4)**                              |
    | Availability options | **No infrastructure redundancy required(5)**    |
    | Security type        | **Standard**                                    |
    | Image                | **Windows Server 2019 Datacenter - x64 Gen 2(6)**   |
    | Size                 | **Standard_D2s_v3 - 2 vcpu, 8 GiB memory(7)**   |
    | Username             | **TestUser(8)**                                 |
    | Password             | **Provide a secure password(9)**                |
    | Confirm password     | **Provide a secure password(10)**                |

     ![Picture 7](../media/az7007.png)

     ![Picture 7](../media/az7008.png)

1. Select **Next : Disks(11)**, then select **Next : Networking**. 

1. On the **Networking** tab, use the information in the table below to configure networking settings.

    | **Setting**                                                  | **Value**                     |
    | ------------------------------------------------------------ | ----------------------------- |
    | Virtual network                                              | **IntLB-VNet**                |
    | Subnet                                                       | **myBackendSubnet**           |
    | Public IP                                                    | Change to **None**            |
    | NIC network security group                                   | **Advanced**                  |
    | Configure network security group                             | Select the existing **myNSG** |
    | Load balancing options                                       | **None**                      |

1. Select **Review + create**.

1. Select **Create**.

1. Wait for this last VM to be deployed before moving forward with the next task.

### Task 5.2: Connect to the test VM to test the load balancer

1. On the Azure portal home page, from top left corner of page click **Show portal menu** and select **All resources**, then select on **myIntLoadBalancer** from the resources list.

1. On the **Overview** page, make a note in notepad of the **Private IP address**, or copy it to the clipboard.

   **Note**: You may need to select **See more** in order to see the **Private IP address** field.

    ![Picture 7](../media/az7009.png)

1. Select **Home**, then on the Azure portal home page, select **All resources**, then select the **myTestVM** virtual machine that you just created.

1. On the **Overview** page, select **Connect**. Under Configured connection section, select **Connect via Bastion**.

1. In the **Username** box, enter **TestUser** and in the **Password** box, enter the password you created during **myTestVM** virtual machine deployment in task: 5.1, then select **Connect**.

    ![Picture 7](../media/az70010.png)
  
    **Note**: If popup blocker is preventing the new window, at top of the page select **Always allow pop-ups and redirects from hhtps://portal.azure.com** and select 
    **Done**, repeat step-5.

    ![Picture 7](../media/az70011.png)

1. The **myTestVM** window will open in another browser tab.

1. If a **Networks** pane appears, select **Yes**.

1. If you see **See text and images copied to the clipboard** then select **Allow** and minimize the server manager tab.

1. Select the **Internet Explorer** icon in the task bar to open the web browser.

1. Select **OK** on the **Set up Internet Explorer 11** dialog box.

1. Select **Close** on the Internet Explorer security alerts that may pop-up. 

1. Enter (or paste) the **Private IP address** (e.g. 10.1.0.4) from the previous step into the address bar of the browser and press Enter.

1. Select **Yes** on the Security Alert that may pop-up.

1. The default web home page of the IIS Web server is displayed in the browser window. One of the three virtual machines in the backend pool will respond.

    ![](../media/mod4-u4-12.png)
   
1. If you select the refresh button in the browser a few times, you will see that the response comes randomly from the different VMs in the backend pool of the internal load balancer.
   
    ![](../media/mod4-u4-11.png)

    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
    <validation step="d643606f-8c58-498a-a834-5c18e2d7072a" />

## Review

In this lab, you have completed:
+ Create the virtual network
+ Create backend servers
+ Create the load balancer
+ Create load balancer resources
+ Test the load balancer

   
## You have successfully completed the lab.
