---
title: Red Hat Enterprise Linux 自带订阅 Azure 映像 |Microsoft Docs
description: 了解 Azure 上的 Red Hat Enterprise Linux 的自带订阅映像
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 1/14/2020
ms.author: alsin
ms.openlocfilehash: e9cbd98afd5f9ed5561cba2236d85da331db7895
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75944699"
---
# <a name="red-hat-enterprise-linux-bring-your-own-subscription-gold-images-in-azure"></a>在 Azure 中 Red Hat Enterprise Linux 自带订阅金牌映像
Red Hat Enterprise Linux （RHEL）映像可通过即用即付（PAYG）或自带订阅（Red Hat 黄金图）模型在 Azure 中使用。 本文档概述了 Azure 中的 Red Hat 黄金映像。

## <a name="important-points-to-consider"></a>需要考虑的要点

- 此程序中提供的 Red Hat 金牌映像与 Azure 库/Marketplace 中的 RHEL PAYG 映像类似。

- 映像遵循在[Azure 上 Red Hat Enterprise Linux 映像](./redhat-images.md)中描述的当前策略

- 标准支持策略适用于通过这些映像创建的 Vm

- 从 Red Hat 黄金映像预配的 Vm 不会携带 rhel PAYG 映像关联的 RHEL 费用

- 映像是未获授权的，因此你必须使用订阅管理器注册并订阅 Vm 才能直接从 Red Hat 获取更新

- 目前不能在 Linux 映像的 BYOS 和 PAYG 计费模型之间动态切换。 若要切换计费模型，需要从各自的映像重新部署 VM

## <a name="requirements-and-conditions-to-access-the-red-hat-gold-images"></a>访问 Red Hat 黄金映像的要求和条件

1. 熟悉[Red Hat 云访问计划](https://www.redhat.com/en/technologies/cloud-computing/cloud-access)条款，并为 Red Hat[订阅管理器](https://access.redhat.com/management/cloud)的云访问启用 red hat 订阅。 需要将注册的 Azure 订阅作为云访问的权限进行注册。

1. 如果已为符合适当资格要求的云访问启用 Red Hat 订阅，将自动为你的 Azure 订阅启用金牌映像访问。

### <a name="expected-time-for-image-access"></a>图像访问的预计时间

完成云访问启用步骤后，Red Hat 将验证 Red Hat 黄金映像的资格。 如果验证成功，你将在三个小时内收到黄金映像的访问权限。

## <a name="use-the-red-hat-gold-images-from-the-azure-portal"></a>使用 Azure 门户中的 Red Hat 黄金映像

1. 在 Azure 订阅收到对 Red Hat 黄金映像的访问权限后，可以通过导航到 "**创建资源**" 并**查看全部**，在[Azure 门户](https://portal.azure.com)中找到它们。

1. 在页面顶部，你会看到你有专用产品/服务。

    ![Marketplace 专用产品/服务](./media/rhel-byos-privateoffers.png)

1. 你可以单击紫色链接或向下滚动到页面底部，查看你的专用产品/服务。

1. UI 中的其余设置与任何其他现有的 Red Hat 映像都没有什么不同。 选择 RHEL 版本，并按照提示进行操作以预配 VM。 此过程还可让你在最后一步接受映像的条款。

>[!NOTE]
>到目前为止，这些步骤不会启用适用于编程部署的 Red Hat 黄金图像映像，如下面的 "其他信息" 部分所述，需要额外的步骤。

本文档的其余部分重点介绍用于预配和接受有关映像的术语的 CLI 方法。 由于最终的结果（预配的 RHEL 黄金映像 VM），UI 和 CLI 是完全可互换的。

## <a name="use-the-red-hat-gold-images-from-the-azure-cli"></a>使用 Azure CLI 中的 Red Hat 黄金映像
以下一组说明将指导你完成使用 Azure CLI 的 RHEL VM 的初始部署过程。 这些说明假定已[安装 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

>[!IMPORTANT]
>请确保在发布者、产品/服务、计划和映像引用中为以下所有命令使用所有小写字母

1. 检查你是否处于所需的订阅中：
    ```azurecli
    az account show -o=json
    ```

1. 为 Red Hat 黄金映像 VM 创建资源组：
    ```azurecli
    az group create --name <name> --location <location>
    ```

1. 接受映像术语：
    ```azurecli
    az vm image terms accept --publisher redhat --offer rhel-byos --plan <SKU value here> -o=jsonc

    # Example:
    az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm75 -o=jsonc

    OR

    az vm image terms accept --urn RedHat:rhel-byos:rhel-lvm8:8.0.20190620
    ```
    >[!NOTE]
    >每*个 Azure 订阅（每个映像 SKU）需要接受一次*这些条款。

1. 可有可无通过以下命令验证 VM 部署：
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate

    # Example:
    az vm create -n rhel-byos-vm -g rhel-byos-group --image RedHat:rhel-byos:rhel-lvm75:7.5.20190620
    ```

1. 在不带 `--validate` 参数的情况下运行同一命令来预配 VM：
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate
    ```

1. 通过 SSH 连接到 VM，并验证是否有未获授权映像。 为此，请运行 `sudo yum repolist` （RHEL 8 使用 `sudo dnf repolist`）。 此输出将要求你使用订阅管理器向 Red Hat 注册 VM。

>[!NOTE]
>在 RHEL 8 `dnf` 和 `yum` 可互换，有关此方面的详细信息，请查阅[RHEL 8 管理员指南](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/installing-software-with-yum_configuring-basic-system-settings)。


## <a name="use-the-red-hat-gold-images-from-powershell"></a>在 PowerShell 中使用 Red Hat 黄金映像
下面是一个示例脚本。 应将资源组、位置、VM 名称、登录信息和其他变量替换为所选的配置。 发布服务器和计划信息必须为小写。

```powershell-interactive
    # Variables for common values
    $resourceGroup = "testbyos"
    $location = "canadaeast"
    $vmName = "test01"

    # Define user name and blank password
    $securePassword = ConvertTo-SecureString 'TestPassword1!' -AsPlainText -Force
    $cred = New-Object System.Management.Automation.PSCredential("azureuser",$securePassword)
    Get-AzureRmMarketplaceTerms -Publisher RedHat -Product rhel-byos -Name rhel-lvm75 | SetAzureRmMarketplaceTerms -Accept

    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroup -Location $location

    # Create a subnet configuration
    $subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

    # Create a virtual network
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location
    $location `-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

    # Create a public IP address and specify a DNS name
    $pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location
    $location `-Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

    # Create an inbound network security group rule for port 22
    $nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name
    myNetworkSecurityGroupRuleSSH -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -
    DestinationAddressPrefix * `-DestinationPortRange 22 -Access Allow

    # Create a network security group
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location
    $location `-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH

    # Create a virtual network card and associate with public IP address and NSG
    $nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -
    Location $location `-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

    # Create a virtual machine configuration
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D3_v2 |
    Set-AzureRmVMOperatingSystem -Linux -ComputerName $vmName -Credential $cred |
    Set-AzureRmVMSourceImage -PublisherName redhat -Offer rhel-byos -Skus rhel-lvm75 -Version latest | Add-     AzureRmVMNetworkInterface -Id $nic.Id
    Set-AzureRmVMPlan -VM $vmConfig -Publisher redhat -Product rhel-byos -Name "rhel-lvm75"

    # Configure SSH Keys
    $sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
    Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

    # Create a virtual machine
    New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

## <a name="additional-information"></a>其他信息
- 如果尝试在未针对此产品/服务启用的订阅上预配 VM，你将收到以下错误，你应与 Microsoft 或 Red Hat 联系以启用你的订阅。
    ```
    "Offer with PublisherId: redhat, OfferId: rhel-byos, PlanId: rhel-lvm75 is private and can not be purchased by subscriptionId: GUID"
    ```

- 如果从 RHEL BYOS 映像创建快照并在[共享映像库](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries)中发布映像，则需要提供与快照原始源匹配的计划信息。 例如，命令可能如下所示（请注意最后一行中的计划参数）：
    ```azurecli
    az vm create –image \
    "/subscriptions/GUID/resourceGroups/GroupName/providers/Microsoft.Compute/galleries/GalleryName/images/ImageName/versions/1.0.0" \
    -g AnotherGroupName --location EastUS2 -n VMName \
    --plan-publisher redhat --plan-product rhel-byos --plan-name rhel-lvm75
    ```

- 如果使用自动化从 RHEL BYOS 映像预配 Vm，则需要提供类似于上面所示内容的计划参数。 例如，如果使用的是 Terraform，则应在[计划块](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html#plan)中提供计划信息。

## <a name="next-steps"></a>后续步骤
* [Red Hat 云访问文档](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/index)中提供了云访问的分步指南和程序详细信息。
* 了解有关[Azure Red Hat 更新基础结构](./redhat-rhui.md)的详细信息。
* 若要详细了解 Azure 中的 Red Hat 映像，请转到此[文档页面](./redhat-images.md)。
* 可以在 [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata)（Red Hat Enterprise Linux 生命周期）页找到有关 RHEL 所有版本的 Red Hat 支持策略的信息。
