#  Module 01-Unit 8 Connect two Azure Virtual Networks using global virtual network peering

## Lab scenario 
In this lab, you will configure connectivity between the CoreServicesVnet and the ManufacturingVnet by adding peerings to allow traffic flow. 

## Lab Objective

In this lab, you will complete the following tasks:

+ Task 1: Create a Virtual Machine to test the configuration
+ Task 2: Connect to the Test VMs using RDP
+ Task 3: Test the connection between the VMs
+ Task 4: Create VNet peerings between CoreServicesVnet and ManufacturingVnet
+ Task 5: Test the connection between the VMs
+ Task 6: Clean up resources

**Note:** An **[interactive lab simulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Connect%20two%20Azure%20virtual%20networks%20using%20global%20virtual%20network%20peering)** is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

## Estimated time: 20 minutes

## Architecture diagram

   ‎![](../media/az700-m1-unit8.png)

### Task 1: Create a Virtual Machine to test the configuration

In this section, you will create a test VM on the Manufacturing VNet to test if you can access resources inside another Azure virtual network from your ManufacturingVnet.

#### Create ManufacturingVM

1. On the Azure portal select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

    ![](../media/pwershell1.png)
   
1. On **Getting started** window choose **Mount storage account** then under **Storage account subscription** select your available subscription from the dropdown and click on **Apply**.
   
     ![](../media/pwershell3.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account** and click **Next**.

     ![](../media/pwershell4.png)

1. Please make sure you have selected your resource group **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>**.

1. Select the Region **<inject key="Region" enableCopy="false"/>**.
   
1. Enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account name**.

1. Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** for the  **File share** , then click on **Create**.

    ![](../media/pwershell5.png)

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files** icon, in the drop-down menu, select **Upload**.

     ![](../media/pwershell2.png)
   
1. Upload the following files **ManufacturingVMazuredeploy.json** and **ManufacturingVMazuredeploy.parameters.json** into the Cloud Shell home directory one by one from the source folder **C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M01**.

1. Deploy the following ARM templates to create the VMs needed for this exercise:

   ```powershell
   $RGName = "ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile ManufacturingVMazuredeploy.json -TemplateParameterFile ManufacturingVMazuredeploy.parameters.json
   ```
1. You will be prompted to provide an Admin password. Provide Admin password Password: **Pa$$w0rd1234**.   
   
1. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines**.

1. Verify that the virtual machine has been created.

   ![](../media/virtualmachine.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
   <validation step="53f858e3-8ea3-46e1-9e46-27d87801bd28" />

## Task 2: Connect to the Test VMs using RDP

In this task, your connecting to the Test VM using RDP.

1. On the Azure Portal home page, select **Virtual Machines**.

1. Select **ManufacturingVM**.

1. On ManufacturingVM, select **Connect** from the drop-down click on **Connect**.

   ![](../media/connect1.png)

1. On ManufacturingVM | Connect, select **Download RDP file**.

   ![](../media/download1.png)

1. Save the RDP file to your desktop. If any warning pops-up in "edge downloads" select **Keep**.

1. Connect to ManufacturingVM using the RDP file, and the username **TestUser** and the password **Pa$$w0rd1234** you provided during deployment.

1. On the Azure Portal home page, select **Virtual Machines**.

1. Select **testvm1**.

1. On testvm1, select **Connect** then from the drop-dwon click **Connect**. 

1. On **testvm1 | Connect** page, under **Native RDP** click on **Select** and on **Native RDP** window select and **Download RDP file**. 

1. Save the RDP file to your desktop.

1. Connect to testvm1 using the RDP file, and the username **TestUser** and the password **Pa$$w0rd1234**.

1. On both VMs, in **Choose privacy settings for your device**, select **Accept**.

1. On both VMs, in **Networks**, select **Yes**.

1. On testvm1, Right click on start and select windows PowerShell, and run the following command: **ipconfig**

1. Note the IPv4 address. 

## Task 3: Test the connection between the VMs

1. On the ManufacturingVM, Right click on start and select windows PowerShell.

1. Use the following command to verify that there is no connection to testvm1 on CoreServicesVnet. Be sure to use the IPv4 address for testvm1.

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. The test connection should fail, and you will see a result similar to the following:

   ![PowerShell window with Test-NetConnection 10.20.20.4 -port 3389 showing failed ](../media/test-netconnection-fail.png)

## Task 4: Create VNet peerings between CoreServicesVnet and ManufacturingVnet

1. On the Azure home page, select **Virtual Networks**, and then select **CoreServicesVnet**.

1. In CoreServicesVnet, under **Settings**, select **Peerings**.

   ![screen shot of core services VNet Peering settings ](../media/create-peering-on-coreservicesvnet.png)

1. On CoreServicesVnet | Peerings, select **+ Add**.

1. Use the information in the following table to create the peering.

   | **Section**                          | **Option**                                    | **Value**                             |
   | ------------------------------------ | --------------------------------------------- | ------------------------------------- |
   | Remote virtual network summary       |                                               |                                       |
   |                                      | Peering link name                             | ManufacturingVnet-to-CoreServicesVnet |
   |                                      | Virtual network deployment model              | Resource manager                      |
   |                                      | I know my resource ID                         | Not selected                          |
   |                                      | Subscription                                  | Select the Subscription provided      |
   |                                      | Virtual network                               | ManufacturingVnet                     |
   | Local virtual network summary        |                                               |                                       |
   |                                      | Peering link name                             | CoreServicesVnet-to-ManufacturingVnet |
   
1. Review your settings and select **Add**. 

1. In CoreServicesVnet | Peerings, verify that the **CoreServicesVnet-to-ManufacturingVnet** peering is listed.

1. Navigate back to the **Virtual networks** page.

1. In **ManufacturingVnet**, under **Settings**, select **Peerings**.

1. Verify the **ManufacturingVnet-to-CoreServicesVnet** peering is listed.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help you out.
   <validation step="430adf7c-4c27-47f2-b079-292ae528c898" />

## Task 5: Test the connection between the VMs

1. On the ManufacturingVM, open a PowerShell prompt.

1. Use the following command to verify that there is now a connection to TestVM1 on CoreServicesVnet. 

   ```powershell
    Test-NetConnection 10.20.20.4 -port 3389
    ```

1. The test connection should succeed, and you will see a result similar to the following:

   ![Powershell window with Test-NetConnection 10.20.20.4 -port 3389 showing TCP test succeeded: true](../media/test-connection-succeeded.png)

## Task 6: Clean up resources

   >**Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

1. On the Azure portal, open the **PowerShell** session within the **Cloud Shell** pane. (Create Cloud Shell storage if needed, using default settings.)

1. Delete all resource groups you created throughout the labs of this module by running the following command:

   ```powershell
   Remove-AzResourceGroup -Name 'ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>' -Force -AsJob
   ```

    >**Note**: The command executes asynchronously (as determined by the -AsJob parameter), so while you will be able to run another PowerShell command immediately afterwards within the same PowerShell session, it will take a few minutes before the resource groups are actually removed.

## Review

In this lab, you have completed:

+ Creating a Virtual Machine to test the configuration
+ Connecting to the Test VMs using RDP
+ Testing the connection between the VMs
+ Creating VNet peerings between CoreServicesVnet and ManufacturingVnet
+ Testing the connection between the VMs

## You have successfully completed the lab.

