---
title: 管理读取副本-Azure 门户-Azure Database for PostgreSQL-单一服务器
description: 了解如何从 Azure 门户管理 Azure Database for PostgreSQL 单一服务器的读取副本。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: 5e031354d4695a1d7eb6f199e23e74b796273230
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770214"
---
# <a name="create-and-manage-read-replicas-in-azure-database-for-postgresql---single-server-from-the-azure-portal"></a>在 Azure 门户中创建和管理 Azure Database for PostgreSQL 单服务器中的读取副本

本文介绍如何使用 Azure 门户在 Azure Database for PostgreSQL 中创建和管理只读副本。 若要详细了解只读副本，请参阅[概述](concepts-read-replicas.md)。


## <a name="prerequisites"></a>必备组件
用作主服务器的 [Azure Database for PostgreSQL 服务器](quickstart-create-server-database-portal.md)。

## <a name="prepare-the-master-server"></a>准备主服务器
必须使用以下步骤在“常规用途”和“内存优化”层中准备主服务器。 主服务器已准备好通过设置 azure. replication_support 参数进行复制。 更改复制参数时，需要重新启动服务器才能使更改生效。 在 Azure 门户中，这两个步骤通过一个按钮进行封装，**启用复制支持**。

1. 在 Azure 门户中，选择用作主服务器的现有 Azure Database for PostgreSQL 服务器。

2. 在服务器边栏上的 "**设置**" 下，选择 "**复制**"。

3. 选择 "**启用复制支持**"。 

   ![启用复制支持](./media/howto-read-replicas-portal/enable-replication-support.png)

4. 确认你要启用复制支持。 此操作将重新启动主服务器。 

   ![确认启用复制支持](./media/howto-read-replicas-portal/confirm-enable-replication.png)
   
5. 操作完成后，你会收到两个 Azure 门户通知。 有一个更新服务器参数的通知。 现在会出现另一个有关服务器重启的通知。

   ![成功通知-启用](./media/howto-read-replicas-portal/success-notifications-enable.png)

6. 刷新 Azure 门户页面以更新复制工具栏。 你现在可以为此服务器创建读取副本。

   ![更新的工具栏](./media/howto-read-replicas-portal/updated-toolbar.png)
   
为每个主服务器启用复制支持一次。 为方便起见，提供了**禁用复制支持**按钮。 我们不建议禁用复制支持，除非你确定不会在此主服务器上创建副本。 如果你的主服务器有现有副本，则无法禁用复制支持。


## <a name="create-a-read-replica"></a>创建只读副本
若要创建只读副本，请遵循以下步骤：

1. 选择用作主服务器的现有 Azure Database for PostgreSQL 服务器。 

2. 在服务器边栏上的 "**设置**" 下，选择 "**复制**"。

3. 选择“添加副本”。

   ![添加副本](./media/howto-read-replicas-portal/add-replica.png)

4. 输入只读副本的名称。 

    ![为副本命名](./media/howto-read-replicas-portal/name-replica.png)

5. 选择副本的位置。 默认位置与主服务器的位置相同。

    ![选择一个位置](./media/howto-read-replicas-portal/location-replica.png)

   > [!NOTE]
   > 若要了解有关可以在中创建副本的区域的详细信息，请访问[读取副本概念一文](concepts-read-replicas.md)。 

6. 选择“确定”以确认创建该副本。

使用与 master 相同的计算和存储设置创建副本。 创建副本后，可以独立于主服务器更改多项设置：计算代系、vCore 数、存储和备份保留期。 定价层也可以独立更改，但“基本”层除外。

> [!IMPORTANT]
> 将主服务器设置更新为新值之前，请将副本设置更新为一个相等或更大的值。 此操作可帮助副本保持对主副本所做的任何更改。

创建只读副本后，可以在“复制”窗口中查看它：

![在“复制”窗口中查看新副本](./media/howto-read-replicas-portal/list-replica.png)
 

## <a name="stop-replication"></a>停止复制
可以停止主服务器与只读副本之间的复制。

> [!IMPORTANT]
> 停止复制到主服务器和只读副本后，无法撤消该操作。 只读副本将成为支持读取和写入的独立服务器。 独立服务器不能再次成为副本。

若要在 Azure 门户中停止主服务器与只读副本之间的复制，请遵循以下步骤：

1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 主服务器。

2. 在服务器菜单中的“设置”下，选择“复制”。

3. 选择要停止复制的副本服务器。

   ![选择副本](./media/howto-read-replicas-portal/select-replica.png)
 
4. 选择“停止复制”。

   ![选择停止复制](./media/howto-read-replicas-portal/select-stop-replication.png)
 
5. 选择“确定”以停止复制。

   ![确认停止复制](./media/howto-read-replicas-portal/confirm-stop-replication.png)
 

## <a name="delete-a-master-server"></a>删除主服务器
若要删除主服务器，请遵循删除独立 Azure Database for PostgreSQL 服务器的相同步骤。 

> [!IMPORTANT]
> 删除主服务器后，将停止复制到所有只读副本。 只读副本将成为支持读取和写入的独立服务器。

若要在 Azure 门户中删除服务器，请遵循以下步骤：

1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 主服务器。

2. 此时会打开该服务器的“概述”页。 选择“删除”。

   ![在服务器的“概述”页上，选择删除主服务器](./media/howto-read-replicas-portal/delete-server.png)
 
3. 输入要删除的主服务器的名称。 选择“删除”以确认删除主服务器。

   ![确认删除主服务器](./media/howto-read-replicas-portal/confirm-delete.png)
 

## <a name="delete-a-replica"></a>删除副本
可以像删除主服务器一样删除只读副本。

- 在 Azure 门户中，打开只读副本的“概述”页。 选择“删除”。

   ![在副本的“概述”页上，选择删除该副本](./media/howto-read-replicas-portal/delete-replica.png)
 
也可以在“复制”窗口中遵循以下步骤删除只读副本：

1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 主服务器。

2. 在服务器菜单中的“设置”下，选择“复制”。

3. 选择要删除的只读副本。

   ![选择要删除的副本](./media/howto-read-replicas-portal/select-replica.png)
 
4. 选择“删除副本”。

   ![选择“删除副本”](./media/howto-read-replicas-portal/select-delete-replica.png)
 
5. 输入要删除的副本的名称。 选择“删除”以确认删除该副本。

   ![确认删除副本](./media/howto-read-replicas-portal/confirm-delete-replica.png)
 

## <a name="monitor-a-replica"></a>监视副本
可以使用两个指标来监视只读副本。

### <a name="max-lag-across-replicas-metric"></a>“副本的最大滞后时间”指标
“副本的最大滞后时间”指标显示主服务器与滞后时间最长的副本之间的滞后时间（以字节为单位）。 

1.  在 Azure 门户中，选择 Azure Database for PostgreSQL 主服务器。

2.  选择“指标”。 在“指标”窗口中，选择“副本的最大滞后时间”。

    ![监视副本的最大滞后时间](./media/howto-read-replicas-portal/select-max-lag.png)
 
3.  对于“聚合”，请选择“最大”。


### <a name="replica-lag-metric"></a>“副本滞后时间”指标
“副本滞后时间”指标显示自从在副本上最后一次执行重放事务以来所经历的时间。 如果主服务器上未发生任何事务，则该指标会反映此滞后时间。

1. 在 Azure 门户中，选择 Azure Database for PostgreSQL 只读副本。

2. 选择“指标”。 在“指标”窗口中，选择“副本滞后时间”。

   ![监视副本滞后时间](./media/howto-read-replicas-portal/select-replica-lag.png)
 
3. 对于“聚合”，请选择“最大”。 
 
## <a name="next-steps"></a>后续步骤
* 详细了解 [Azure Database for PostgreSQL 中的只读副本](concepts-read-replicas.md)。
* 了解如何[创建和管理 Azure CLI 和 REST API 中的读取副本](howto-read-replicas-cli.md)。