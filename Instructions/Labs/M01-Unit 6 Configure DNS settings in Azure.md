# Module01 - Unit 6 Configure DNS settings in Azure

## Exercise scenario 
In this unit, you will configure DNS name resolution for Contoso Ltd. You will create a private DNS zone named contoso.com, link the VNets for registration and resolution, and then create two virtual machines and test the configuration.

In this exercise, you will:

+ Task 1: Create a private DNS Zone
+ Task 2: Link subnet for auto registration
+ Task 3: Create Virtual Machines to test the configuration
+ Task 4: Verify records are present in the DNS zone

**Note:** An **[interactive lab simulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20DNS%20settings%20in%20Azure)** is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

#### Estimated time: 25 minutes

## Task 1: Create a private DNS Zone

1. On the Azure portal locate the search bar at the top of the page. Search for **Private DNS zones** and select **Private DNS zones** under services that appears in the results under **Services**.
  ‎![](../media/create-private-dns-zone.png)

1. In Private DNS zones, select **+ Create**.

1. Use the information in the following table to create the private DNS zone.

   | **Tab**         | **Option**                             | **Value**            |
   | --------------- | -------------------------------------- | -------------------- |
   | Basics          | Resource group                         | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> |
   |                 | Name                                   | Contoso.com          | 

1. Select **Review + create** and **create**

   **Note**: Please Wait until the deployment is completed, and then select **Go to resource**.

1. Verify that the zone has been created.

## Task 2: Link subnet for auto registration

1. In Contoso.com, under **Settings**, select **Virtual network links**.

1. On Contoso.com | Virtual network links, select **+ Add**.

   ![](../media/add-network-link-dns.png)

1. Use the information in the following table to add the virtual network link.

   | **Option**                          | **Value**                               |
   | ----------------------------------- | --------------------------------------- |
   | Link name                           | CoreServicesVnetLink                    |
   | Subscription                        | No changes required                     |
   | Virtual Network                     | Select CoreServicesVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
   | Enable auto registration            | Selected                                |
   | Review your settings and select OK. |                                         |

   **Note**: Please Wait for 2 mins to Successfully create virtual network link
   
1. Select **Refresh**.

1. Verify that the CoreServicesVnetLink has been created, and that auto-registration is enabled.

1. Repeat steps 2 - 5 for the ManufacturingVnet, using the information in the following table: 

   | **Option**                          | **Value**                                |
   | ----------------------------------- | ---------------------------------------- |
   | Link name                           | ManufacturingVnetLink                    |
   | Subscription                        | No changes required                      |
   | Virtual Network                     | select ManufacturingVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
   | Enable auto registration            | Selected                                 |
   | Review your settings and select OK. |                                          |

   **Note**: Please Wait for 2 mins to Successfully create virtual network link

1. Select **Refresh**.

1. Verify that the ManufacturingVnetLink has been created, and that auto-registration is enabled.

1. Repeat steps 2 - 5 for the ResearchVnet, using the information in the following table: 

   | **Option**                          | **Value**                           |
   | ----------------------------------- | ----------------------------------- |
   | Link name                           | ResearchVnetLink                    |
   | Subscription                        | No changes required                 |
   | Virtual Network                     | select ResearchVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
   | Enable auto registration            | Selected                            |
   | Review your settings and select OK. |                                     |
  
   **Note**: Please Wait for 2 mins to Successfully create virtual network link

1. Select **Refresh**.

1. Verify that the ResearchVnetLink has been created, and that auto-registration is enabled.

 ##  Task 3: Create Virtual Machines to test the configuration

In this section, you will create two test VMs to test the Private DNS zone configuration.

1. On the Azure portal, open the **PowerShell** session within the **Cloud Shell** pane.
    
    > **Note:** If this is the first time opening Cloud Shell, you might be prompted to create a storage account. Select **Create storage**.

2. On the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu, select **Upload** and upload the following files **azuredeploy.json** and **azuredeploy.parameters.json** into the Cloud Shell home directory one by one from the source folder **C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M01**.

3. Deploy the following ARM templates to create the VMs needed for this exercise:

    >**Note**: You will be prompted to provide an Admin password.

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```
  
4. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines**.

5. Verify that both virtual machines have been created.

 ## Task 4: Verify records are present in the DNS zone

1. On the Azure Portal home page, select **Private DNS zones**.

2. On Private DNS zones, select **contoso.com**.

3. Verify that host (A) records are listed for both VMs, as shown:

![Contoso.com DNS zone showing auto-registered host A records.](../media/contoso_com-dns-zone.png)

 

4. Make a note of the names and IP addresses of the VMs.

 ### Connect to the Test VMs using RDP

1. On the Azure Portal home page, select **Virtual Machines**.

1. Select **TestVM1**.

1. On TestVM1, select **Connect &gt; RDP** and download the RDP file.

    ![TestVM1 with Connect and RDP highlighted.](../media/connect-to-am.png)

1. Save the RDP file to your desktop.

1. Follow the same steps for **TestVM2**

1. Connect to TestVM1 using the RDP file, and the username **TestUser** and the password you provided during deployment.

1. Connect to TestVM2 using the RDP file, and the username **TestUser** and the password you provided during deployment.

1. On both VMs, in **Choose privacy settings for your device**, select **Accept**.

1. On both VMs, if prompted, in **Networks**, select **Yes**.

1. On TestVM1, open a command prompt and enter the command ipconfig /all.

1. Verify that the IP address is the same as the one you noted in the DNS zone.

1. Enter the command ping TestVM2.contoso.com.

1. Verify that the FQDN resolves to the IP address that you noted in the Private DNS zone. The ping itself will timeout because of the Windows Firewall that is enabled on the VMs.

1. Alternatively, you can enter the command nslookup TestVM2.contoso.com and verify that you receive a successful name resolution record for VM2
 

Congratulations! You have created a private DNS Zone, added a name resolution and auto-registration link, and tested name resolution in your configuration. 
