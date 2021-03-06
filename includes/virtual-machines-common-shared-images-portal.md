---
title: include 文件
description: include 文件
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/06/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 729e757c69887bbdce324e2d8383c970995dc94a
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903677"
---
## <a name="sign-in-to-azure"></a>登录 Azure 

通过 https://portal.azure.com 登录到 Azure 门户。

> [!NOTE]
> 如果在预览期间注册了使用共享图像库，则可能需要重新注册 `Microsoft.Compute` 提供程序。 打开[Cloud Shell](https://shell.azure.com/bash)并键入： `az provider register -n Microsoft.Compute`

## <a name="create-an-image-gallery"></a>创建映像库

映像库是用于启用映像共享的主要资源。 允许用于库名称的字符为大写或小写字母、数字、点和句点。 库名称不能包含短划线。  库名称在你的订阅中必须唯一。 

以下示例在“myGalleryRG”资源组中创建名为“myGallery”的库。

1. 在 Azure 门户的左上角选择“创建资源”。
1. 使用 "搜索" 框中的 "类型**共享图像库**"，并在结果中选择 "**共享图像库**"。
1. 在 "**共享图像库**" 页中，单击 "**创建**"。
1. 选择正确的订阅。
1. 在 "**资源组**" 中，选择 "**新建**"，然后键入*myGalleryRG*作为名称。
1. 在 "**名称**" 中，键入*myGallery*作为库的名称。
1. 保留 "**区域**" 的默认值。
1. 可以键入库的简短说明，如*用于测试的图像库。* 然后单击 "**查看 + 创建**"。
1. 验证通过后，选择 "**创建**"。
1. 部署完成后，选择 "**前往资源**"。
   
## <a name="create-an-image-definition"></a>创建映像定义 

映像定义为映像创建一个逻辑分组。 它们用于管理有关映像版本的信息，这些版本是在其中创建的。 映像定义名称可能包含大写或小写字母、数字、点、短划线和句点。 若要详细了解可以为映像定义指定的值，请参阅[映像定义](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#image-definitions)。

在库中创建库图像定义。 在此示例中，库图像名为*myImageDefinition*。

1. 在新图像库的页面上，从页面顶部选择 "**添加新的图像定义**"。 
1. 对于 "**映像定义名称**"，键入*myImageDefinition*。
1. 对于 "**操作系统**"，请根据源 VM 选择正确的选项。
1. 对于 " **VM 生成**"，请根据源 VM 选择选项。 在大多数情况下，这将是*第1代*。 有关详细信息，请参阅[第2代 Vm 支持](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2)。
1. 对于 "**操作系统状态**"，请根据源 VM 选择选项。 有关详细信息，请参阅[通用化和专用](../articles/virtual-machines/linux/shared-image-galleries.md#generalized-and-specialized-images)化。
1. 对于 "**发布服务器**"，请键入*myPublisher*。 
1. 对于 "**产品/服务**"，键入*myOffer*。
1. 对于 " **SKU**"，键入*mySKU*。
1. 完成后，选择 "**审核 + 创建**"。
1. 在映像定义通过验证后，选择 "**创建**"。
1. 部署完成后，选择 "**前往资源**"。


## <a name="create-an-image-version"></a>创建映像版本

从托管映像创建映像版本。 在此示例中，映像版本为 1.0.0，该版本被复制到美国中西部和美国中南部数据中心。 选择复制的目标区域时，请记住，你还需包括源区域作为复制的目标。

允许用于映像版本的字符为数字和句点。 数字必须在 32 位整数范围内。 格式： *MajorVersion*。*MinorVersion*。*修补程序*。

创建映像版本的步骤稍有不同，具体取决于源是通用映像还是专用 VM 的快照。 

### <a name="option-generalized"></a>选项：一般化

1. 在映像定义的页面中，选择页面顶部的 "**添加版本**"。
1. 在 "**区域**" 中，选择存储托管映像的区域。 映像版本需要在创建时所在的同一区域中创建。
1. 对于 "**名称**"，请键入*1.0.0*。 映像版本名称应遵循 "*主要*"。*次要*。使用整数的*修补*格式。 
1. 在 "**源映像**" 中，从下拉菜单中选择源托管映像。
1. 在 "**从最新中排除**" 中，保留默认值 "*否*"。
1. 对于 "**寿命结束日期**"，请从日历中选择将来几个月的日期。
1. 在 "**复制**" 中，保留**默认副本计数**为1。 你需要复制到源区域，因此，请将第一个副本保留为默认值，然后选择第二个副本区域为 "*美国东部*"。
1. 完成操作后，选择“查看 + 创建”。 Azure 将验证配置。
1. 当映像版本通过验证时，请选择 "**创建**"。
1. 部署完成后，选择 "**前往资源**"。

可能需要一段时间才能将映像复制到所有目标区域。

### <a name="option-specialized"></a>选项：专用化

1. 在映像定义的页面中，选择页面顶部的 "**添加版本**"。
1. 在 "**区域**" 中，选择存储快照的区域。 映像版本需要在创建时所在的同一区域中创建。
1. 对于 "**名称**"，请键入*1.0.0*。 映像版本名称应遵循 "*主要*"。*次要*。使用整数的*修补*格式。 
1. 在**OS 磁盘快照**中，从下拉菜单中选择源 VM 的快照。 如果源 VM 有一个要包含的数据磁盘，请从下拉端中选择正确的**LUN**号，然后选择数据**磁盘快照的快照。** 
1. 在 "**从最新中排除**" 中，保留默认值 "*否*"。
1. 对于 "**寿命结束日期**"，请从日历中选择将来几个月的日期。
1. 在 "**复制**" 中，保留**默认副本计数**为1。 你需要复制到源区域，因此，请将第一个副本保留为默认值，然后选择第二个副本区域为 "*美国东部*"。
1. 完成操作后，选择“查看 + 创建”。 Azure 将验证配置。
1. 当映像版本通过验证时，请选择 "**创建**"。
1. 部署完成后，选择 "**前往资源**"。

## <a name="share-the-gallery"></a>共享库

我们建议你在映像库级别共享访问权限。 下面会指导你完成刚刚创建的库的共享。

1. 打开 [Azure 门户](https://portal.azure.com)。
1. 在左侧菜单中，选择 "**资源组**"。 
1. 在资源组列表中，选择 " **myGalleryRG**"。 资源组的边栏选项卡将打开。
1. 在 " **myGalleryRG** " 页左侧的菜单中，选择 "**访问控制（IAM）** "。 
1. 在 "**添加角色分配**" 下，选择 "**添加**"。 将打开 "**添加角色分配**" 窗格。 
1. 在“角色”下，选择“读取者”。
1. 在 "分配对的**访问权限**" 下，保留**Azure AD 用户、组或服务主体**的默认值。
1. 在 "**选择**" 下，键入要邀请的人员的电子邮件地址。
1. 如果用户在你的组织之外，你将看到消息 "**此用户将发送一封电子邮件，使他们能够与 Microsoft 协作"。** 选择具有电子邮件地址的用户，然后单击 "**保存**"。

如果用户不在组织范围内，他们将收到一封电子邮件邀请，加入组织。 用户需要接受邀请，然后他们将能够在其资源列表中查看库以及所有图像定义和版本。

