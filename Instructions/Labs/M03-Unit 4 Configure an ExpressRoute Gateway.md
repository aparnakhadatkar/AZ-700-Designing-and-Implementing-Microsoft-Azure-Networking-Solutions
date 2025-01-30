# Module 03-Unit 4 Configure an ExpressRoute Gateway

## Deploy ExpressRoute gateways

## Lab Overview

To connect your Azure virtual network and your on-premises network via ExpressRoute, you must create a virtual network gateway first. A virtual network gateway serves two purposes: to exchange IP routes between the networks and to route network traffic. 

**Note:** An **[interactive lab simulation](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Configure%20an%20ExpressRoute%20gateway)** is available that allows you to click through this lab at your own pace. You may find slight differences between the interactive simulation and the hosted lab, but the core concepts and ideas being demonstrated are the same.

**Gateway types**

When you create a virtual network gateway, you need to specify several settings. One of the required settings, '-GatewayType', specifies whether the gateway is used for ExpressRoute, or VPN traffic. The two gateway types are:

- **VPN** - To send encrypted traffic across the public Internet, you use the gateway type 'VPN'. This is also referred to as a VPN gateway. Site-to-Site, Point-to-Site, and VNet-to-VNet connections all use a VPN gateway.
- **ExpressRoute** - To send network traffic on a private connection, you use the gateway type 'ExpressRoute'. This is also referred to as an ExpressRoute gateway and is the type of gateway used when configuring ExpressRoute.

Each virtual network can have only one virtual network gateway per gateway type. For example, you can have one virtual network gateway that uses -GatewayType VPN, and one that uses -GatewayType ExpressRoute.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create the VNet and gateway subnet
+ Task 2: Create the virtual network gateway

## Estimated time: 60 minutes

## Architecture diagram

   ‎![](../media/az700-m3-unit4.png)

## Task 1: Create the VNet and gateway subnet

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual networks (1)**, and then select **Virtual 
   networks (2)** under services.

    ![](../media/VN.png)
   
1. On the Virtual networks page, select **+ Create**.

1. On the **Create virtual networks** blade, on the **Basics** tab, use the information in the following table to create the VNet:

   | **Setting**          | **Value**                        |
   | -------------------- | -------------------------------- |
   | Subscription         | Leave default                    |
   | Resource Group       | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>**|
   | Virtual network name | CoreServicesVNet                 |
   | Region               | East US                          |

1. To add IPv4 address space, on the **IP Addresses** tab, perform the following sub-steps:
      - Click on **Add IPv4 address space (1)** in new address space box.
      - Enter **10.20.0.0 (2)** in address space field.
      - Enter **/16 (3)** in size filed.
      - Then click on the **+ Add a subnet (4)** button.

         ![Azure portal - add gateway subnet](../media/image-01.png)

1. In the **Add a subnet** pane, use the information in the following table to create the subnet and then click on **Add (6)**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Subnet purpose               | Select **Virtual Network Gateway (1)** |
   | Name                         | **GatewaySubnet (2)**  |
   | IPv4 address range           | Select **10.20.0.0/16 (3)**  |
   | Starting address             | **10.20.0.0 (4)**     |
   | Size                         | **/27 (5)**           |

    ![](../media/m3-u4-t1-s5.png)

1. On the Create virtual network page, select **Review + create**.

1. Confirm that the VNet passes the validation and then select **Create**.

   >**Note**: If you are using a dual stack virtual network and plan to use IPv6-based private peering over ExpressRoute, select Add IP6 address space and input IPv6 address range values.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="4b8c0911-f548-4fa0-ad62-298ff78aa0e5" />

## Task 2: Create the virtual network gateway

1. On any Azure Portal page, in **Search resources, services and docs (G+/)**, enter **Virtual network gateways**, and then select **Virtual network gateways** from the results.

1. On the Virtual network gateways page, select **+ Create**.

1. On the **Create virtual network gateway** page, use the information in the following table to create the gateway:

   **Note**: First try to select **Region** to get virtual network option to choose. It might take upto 15 minutes for the newly created virtual network option to show up. 

   | **Setting**               | **Value**                  |
   | ------------------------- | -------------------------- |
   | **Project details**       |                            |
   | Resource Group            | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>** (Auto-selected)     |
   | **Instance details**      |                            |
   | Name                      | CoreServicesVnetGateway    |
   | Region                    | East US                    |
   | Gateway type              | ExpressRoute               |
   | SKU                       | Standard                   |
   | Virtual network           | CoreServicesVNet           |
   | **Public IP address**     |                            |
   | Public IP address         | Create new                 |
   | Public IP address name    | CoreServicesVnetGateway-IP |
   | Public IP address SKU     | Standard                   |
   | Assignment                | Not configurable           |

1. Select **Review + create**.

1. Confirm that the Gateway configuration passes validation and then select **Create**.

1. When the deployment is complete, select **Go to Resource**.

   >**Note**: it can take up to 45 minutes to deploy a Gateway.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="15a76eb7-8705-4ab0-8c7d-c006a7ba304f" />

## Extend your learning with Copilot

Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.
+ How is Azure ExpressRoute different from Virtual WAN? Could you use the technologies together? Provide examples.
+ What should I consider when choosing between an ExpressRoute provider model and ExpressRoute Direct?
+ Create a table that summarizes the Azure ExpressRoute SKU and their features.

## Learn more with self-paced training

+ [Introduction to Azure ExpressRoute](https://learn.microsoft.com/training/modules/intro-to-azure-expressroute/). In this module, you learn what Azure ExpressRoute is and the functionality it provides.
+ [Design and implement ExpressRoute](https://learn.microsoft.com/training/modules/design-implement-azure-expressroute/). In this module, you learn how to design and implement Azure ExpressRoute, ExpressRoute Global Reach, ExpressRoute FastPath.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ Azure ExpressRoute allows an organization to connect their on-premises networks directly into the Microsoft Azure and Microsoft 365 clouds. Azure ExpressRoute uses a dedicated high-bandwidth connection provided by a Microsoft partner.
+ Microsoft guarantees a minimum of 99.95% availability for ExpressRoute dedicated connections. The connection is private and travels over a dedicated line, third parties can't intercept the traffic.
+ You can create a connection between your on-premises network and the Microsoft cloud in four different ways, CloudExchange Co-location, Point-to-point Ethernet Connection, Any-to-any (IPVPN) Connection, and ExpressRoute Direct.
+ ExpressRoute features is determined by the SKU: Local, Standard, and Premuium. 

### Review
In this lab, you have completed:

- Create the VNet and gateway subnet
- Create the virtual network gateway
  
## You have successfully completed the lab.
