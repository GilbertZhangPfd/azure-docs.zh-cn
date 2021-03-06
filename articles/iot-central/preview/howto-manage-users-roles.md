---
title: 在 Azure 中管理用户和角色 IoT Central 应用程序 |Microsoft Docs
description: 作为管理员，如何管理 Azure IoT Central 应用程序中的用户和角色
author: lmasieri
ms.author: lmasieri
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: corywink
ms.openlocfilehash: 9729a51c36a520a2c196fb83515c9fa616411cf3
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2019
ms.locfileid: "74974421"
---
# <a name="manage-users-and-roles-in-your-iot-central-application-preview-features"></a>在 IoT Central 应用程序中管理用户和角色（预览功能）

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

本文介绍如何以管理员身份在 Azure IoT Central 应用程序中添加、编辑和删除用户。 本文还介绍了如何管理 Azure IoT Central 应用程序中的角色。

只有 Azure IoT Central 应用程序的“管理员”角色才能访问和使用“管理”部分。 如果创建 Azure IoT Central 应用程序，系统会自动将其添加到该应用程序的**管理员**角色。

## <a name="add-users"></a>添加用户

每个用户必须有一个用户帐户才能登录和访问 Azure IoT Central 应用程序。 Azure IoT Central 支持 Microsoft 帐户 (MSA) 和 Azure Active Directory (Azure AD) 帐户。 Azure IoT Central 目前不支持 Azure Active Directory 组。

有关详细信息，请参阅 [Microsoft 帐户帮助](https://support.microsoft.com/products/microsoft-account?category=manage-account)和[快速入门：将新用户添加到 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)。

1. 若要将用户添加到 IoT Central 应用程序，请转到“用户”页中的“管理”部分。
    
    > [!div class="mx-imgBorder"]
    >![管理用户](media/howto-manage-users-roles/manage-users-pnp.png)

1. 若要添加用户，在“用户”页上，选择“+ 添加用户”。

1. 从“角色”下拉菜单中为用户选择一个角色。 在本文的[管理角色](#manage-roles)部分详细了解角色。

    > [!div class="mx-imgBorder"]
    >![添加用户并选择角色](media/howto-manage-users-roles/add-user-pnp.png)

    > [!NOTE]
    > 如果用户属于自定义角色，授予他们添加其他用户的权限，则仅可将用户添加到其权限与他们自己的角色具有相同或更少权限的角色。

### <a name="edit-the-roles-that-are-assigned-to-users"></a>编辑分配给用户的角色

在分配角色后，不能对其进行更改。 若要更改分配给某个用户的角色，请删除该用户，然后使用不同的角色再次添加该用户。

> [!NOTE]
> 分配的角色特定于 IoT Central 应用程序，不能从 Azure 门户进行管理。

## <a name="delete-users"></a>删除用户

若要删除用户，选择“用户”页上的一个或多个复选框。 然后选择“删除”。

## <a name="manage-roles"></a>管理角色

角色使你能够控制允许组织中的哪些用户在 IoT Central 执行各种任务。 你可以向应用程序的用户分配三个内置角色。 如果需要更精细的控制，还可以[创建自定义角色](#create-a-custom-role)。

> [!div class="mx-imgBorder"]
> ![管理角色选择](media/howto-manage-users-roles/manage-roles-pnp.png)

### <a name="administrator"></a>管理员

**管理员**角色中的用户可以管理和控制应用程序的每个部分，包括帐单。

创建应用程序的用户会自动分配到“管理员”角色。 必须始终有一个用户充当“管理员”角色。

### <a name="builder"></a>构建者

"**生成器**" 角色中的用户可以管理应用的每个部分，但不能在 "管理" 或 "连续数据导出" 选项卡上进行更改。

### <a name="operator"></a>运算符

**操作员**角色中的用户可以监视设备的运行状况和状态。 不允许用户对设备模板进行更改或管理应用程序。 操作员可以添加和删除设备、管理设备集以及运行分析和作业。 

## <a name="create-a-custom-role"></a>创建自定义角色

如果你的解决方案需要更精细的访问控制，则可以使用自定义权限集创建自定义角色。 若要创建自定义角色，请导航到应用程序的 "**管理**" 部分中的 "**角色**" 页。 然后选择 " **+ 新建角色**"，并为角色添加名称和说明。 选择你的角色所需的权限，然后选择 "**保存**"。

可以采用与将用户添加到内置角色相同的方式将用户添加到自定义角色。

> [!div class="mx-imgBorder"]
> ![生成自定义角色](media/howto-manage-users-roles/create-custom-role-pnp.png)

### <a name="custom-role-options"></a>自定义角色选项

定义自定义角色时，如果用户是角色的成员，则可以选择授予该用户的权限集。 某些权限依赖于其他权限。 例如，如果将**更新应用程序仪表板**权限添加到角色，则 "**查看应用程序仪表板**" 权限将自动添加。 下表汇总了可用权限及其依赖项，可以在创建自定义角色时使用。

#### <a name="managing-devices"></a>管理设备

**设备模板权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 管理 | 查看 <br/> 其他依赖关系：查看设备实例  |
| 完全控制 | 查看、管理 <br/> 其他依赖关系：查看设备实例 |

**设备实例权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None <br/> 其他依赖关系：查看设备模板和设备组 |
| 更新 | 查看 <br/> 其他依赖关系：查看设备模板和设备组  |
| Create | 查看 <br/> 其他依赖关系：查看设备模板和设备组  |
| 删除 | 查看 <br/> 其他依赖关系：查看设备模板和设备组  |
| 执行命令 | 更新、查看 <br/> 其他依赖关系：查看设备模板和设备组  |
| 完全控制 | 查看、更新、创建、删除、执行命令 <br/> 其他依赖关系：查看设备模板和设备组  |

**设备组权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None <br/> 其他依赖关系：查看设备模板和设备实例 |
| 更新 | 查看 <br/> 其他依赖关系：查看设备模板和设备实例   |
| Create | 查看、更新 <br/> 其他依赖关系：查看设备模板和设备实例   |
| 删除 | 查看 <br/> 其他依赖关系：查看设备模板和设备实例   |
| 完全控制 | 查看、更新、创建、删除 <br/> 其他依赖关系：查看设备模板和设备实例 |

**设备连接管理权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 读取实例 | None <br/> 其他依赖关系：查看设备模板、设备组和设备实例 |
| 管理实例 | None |
| 读取全局 | None   |
| 管理全局 | 读取全局 |
| 完全控制 | Read instance、Manage instance、Read global、Manage global。 <br/> 其他依赖关系：查看设备模板、设备组和设备实例 |

**作业权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None <br/> 其他依赖关系：查看设备模板、设备实例和设备组 |
| 更新 | 查看 <br/> 其他依赖关系：查看设备模板、设备实例和设备组 |
| Create | 查看、更新 <br/> 其他依赖关系：查看设备模板、设备实例和设备组 |
| 删除 | 查看 <br/> 其他依赖关系：查看设备模板、设备实例和设备组 |
| 执行 | 查看 <br/> 其他依赖关系：查看设备模板、设备实例和设备组;更新设备实例;对设备实例执行命令 |
| 完全控制 | 查看、更新、创建、删除、执行 <br/> 其他依赖关系：查看设备模板、设备实例和设备组;更新设备实例;对设备实例执行命令 |

**规则权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None <br/> 其他依赖关系：查看设备模板 |
| 更新 | 查看 <br/> 其他依赖关系：查看设备模板 |
| Create | 查看、更新 <br/> 其他依赖关系：查看设备模板 |
| 删除 | 查看 <br/> 其他依赖关系：查看设备模板 |
| 完全控制 | 查看、更新、创建、删除 <br/> 其他依赖关系：查看设备模板 |

#### <a name="managing-the-app"></a>管理应用

**应用程序设置权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| 复制 | 查看 <br/> 其他依赖关系：查看设备模板、设备实例、设备组、仪表板、数据导出、品牌、帮助链接、自定义角色、规则 |
| 删除 | 查看   |
| 完全控制 | 查看、更新、复制、删除 <br/> 其他依赖关系：查看设备模板、设备组、应用程序仪表板、数据导出、品牌、帮助链接、自定义角色、规则 |

**应用程序模板导出权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 导出 | 查看 <br/> 其他依赖关系：查看设备模板、设备实例、设备组、仪表板、数据导出、品牌、帮助链接、自定义角色、规则 |
| 完全控制 | 查看、导出 <br/> 其他依赖关系：查看设备模板、设备组、应用程序仪表板、数据导出、品牌、帮助链接、自定义角色、规则 |

**计费权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 管理 | None     |
| 完全控制 | 管理 |

#### <a name="managing-users-and-roles"></a>管理用户和角色

**自定义角色权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None |
| 更新 | 查看 |
| Create | 查看、更新 |
| 删除 | 查看 |
| 完全控制 | 查看、更新、创建、删除 |

**用户管理权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None <br/> 其他依赖关系：查看自定义角色 |
| 添加 | 查看 <br/> 其他依赖关系：查看自定义角色 |
| 删除 | 查看 <br/> 其他依赖关系：查看自定义角色 |
| 完全控制 | 查看、添加、删除 <br/> 其他依赖关系：查看自定义角色 |

> [!NOTE]
> 如果用户属于自定义角色，授予他们添加其他用户的权限，则仅可将用户添加到其权限与他们自己的角色具有相同或更少权限的角色。

#### <a name="customizing-the-app"></a>自定义应用

**应用程序仪表板权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| Create | 查看、更新 |
| 删除 | 查看   |
| 完全控制 | 查看、更新、创建、删除 |

**个人仪表盘权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| Create | 查看、更新   |
| 删除 | 查看   |
| 完全控制 | 查看、更新、创建、删除 |

**署名、favicon 和 colors 权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| 完全控制 | 查看、更新 |

**Help links 权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| 完全控制 | 查看、更新 |

#### <a name="extending-the-app"></a>扩展应用程序

**数据导出权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| 更新 | 查看   |
| Create | 查看、更新  |
| 删除 | 查看   |
| 完全控制 | 查看、更新、创建、删除 |

**API 令牌权限**

| 名称 | 依赖项 |
| ---- | -------- |
| 查看 | None     |
| Create | 查看   |
| 删除 | 查看   |
| 完全控制 | 查看、创建、删除 |

## <a name="next-steps"></a>后续步骤

现在，你已了解如何在 Azure IoT Central 应用程序中管理用户和角色，接下来建议的下一步是了解如何[管理你的帐单](howto-view-bill.md)。
