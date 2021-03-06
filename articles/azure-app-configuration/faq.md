---
title: Azure 应用配置常见问题解答 |Microsoft Docs
description: 有关 Azure 应用配置的常见问题
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 8d286cbab33a1fb6a2d2a2cb70caed11b21af735
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2019
ms.locfileid: "73904097"
---
# <a name="azure-app-configuration-faq"></a>Azure 应用配置常见问题

本文解决了有关 Azure 应用配置的常见问题。

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>应用程序配置与 Azure Key Vault 有何不同？

应用配置适用于一组不同的用例：它可帮助开发人员管理应用程序设置和控制功能可用性。 它旨在简化处理复杂配置数据的许多任务。

应用配置支持：

- 分层命名空间
- 标记
- 广泛的查询
- 批量检索
- 专用管理操作
- 功能管理用户界面

应用配置与 Key Vault 具有互补性，而这两个应用程序应在大多数应用程序部署中并行使用。

## <a name="should-i-store-secrets-in-app-configuration"></a>是否应在应用配置中存储机密？

尽管应用配置提供强化的安全性，但 Key Vault 仍是存储应用程序机密的最佳位置。 Key Vault 提供硬件级别的加密、精细的访问策略和管理操作，如证书轮换。

你可以创建引用存储在 Key Vault 中的密钥的应用配置值。 有关详细信息，请参阅[在 ASP.NET Core 应用中使用 Key Vault 引用](./use-key-vault-references-dotnet-core.md)。

## <a name="does-app-configuration-encrypt-my-data"></a>应用配置是否对数据进行加密？

可以。 应用配置对其持有的所有密钥值进行加密，并对网络通信进行加密。 密钥名称用作检索配置数据和未加密的索引。

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>应用配置与 Azure App Service 设置有何不同？

Azure App Service 允许你为每个应用服务实例定义应用设置。 这些设置作为环境变量传递给应用程序代码。 如果需要，可以将设置与特定的部署槽相关联。 有关详细信息，请参阅[配置应用设置](/azure/app-service/configure-common#configure-app-settings)。

相反，Azure 应用配置允许你定义可在多个应用之间共享的设置，包括应用服务中运行的应用。 可以通过 .NET 和 Java 的配置提供程序、通过 Azure SDK 或通过 REST Api 直接访问这些设置。

你还可以在应用服务和应用配置之间导入和导出设置。 这使你可以基于现有应用服务设置快速设置新的应用配置存储，或轻松地与依赖于应用服务设置的现有应用共享配置。

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>在应用配置中存储的密钥和值是否有任何大小限制？

单个键值项的限制为10KB。

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>如何为多个环境（测试、过渡、生产等）存储配置？

目前，你可以在每个商店级别控制谁有权访问应用配置。 对于需要不同权限的每个环境，请使用单独的存储区。 此方法可提供最佳的安全隔离。

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>使用应用配置的建议方法有哪些？

请参阅[最佳实践](./howto-best-practices.md)。

## <a name="how-much-does-app-configuration-cost"></a>应用配置的成本是多少？

在公共预览版期间，该服务可免费使用。

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>如何收到有关新版本以及与应用配置相关的其他信息的公告？

订阅[GitHub 公告](https://github.com/Azure/AppConfiguration-Announcements)存储库。

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>如何报告问题或提出建议？

你可以直接在[GitHub](https://github.com/Azure/AppConfiguration/issues)上联系我们。

## <a name="next-steps"></a>后续步骤

* [关于 Azure 应用配置](./overview.md)
