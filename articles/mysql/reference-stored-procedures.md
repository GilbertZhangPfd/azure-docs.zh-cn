---
title: 管理存储过程-Azure Database for MySQL
description: 了解 Azure Database for MySQL 中哪些存储过程可用于帮助你配置数据复制、设置时区和终止查询。
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 7ab77f822ace61ccb023dffe6d79fb1d08278d11
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774934"
---
# <a name="azure-database-for-mysql-management-stored-procedures"></a>Azure Database for MySQL 管理存储过程

存储过程在 Azure Database for MySQL 服务器上可用以帮助管理 MySQL 服务器。 这包括管理服务器的连接、查询和设置数据传入复制。  

## <a name="data-in-replication-stored-procedures"></a>数据传入复制存储过程

借助数据复制，可以将本地运行的 MySQL 服务器、虚拟机或其他云提供程序托管的数据库服务中的数据同步到 Azure Database for MySQL 服务。

下面的存储过程用于在主体和副本之间设置或删除数据传入复制。

|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/A|若要使用 SSL 模式传输数据，请将 CA 证书的上下文传递到 master_ssl_ca 参数中。 </br><br>若要不使用 SSL 模式传输数据，请将空字符串传递到 master_ssl_ca 参数中。|
|mysql.az_replication _start|N/A|N/A|开始复制。|
|mysql.az_replication _stop|N/A|N/A|停止复制。|
|*mysql.az_replication _remove_master*|N/A|N/A|删除主体和副本之间的复制关系。|
|mysql.az_replication_skip_counter|N/A|N/A|跳过一个复制错误。|

若要设置 Azure Database for MySQL 中的主副本和副本之间的数据传入复制，请参阅[如何配置数据传入复制](howto-data-in-replication.md)。

## <a name="other-stored-procedures"></a>其他存储过程

Azure Database for MySQL 中提供了以下存储过程来管理服务器。

|**存储过程名称**|**输入参数**|**输出参数**|**用法说明**|
|-----|-----|-----|-----|
|*mysql. az_kill*|processlist_id|N/A|等效于[`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html)命令。 终止连接所执行的任何语句后，将终止与所提供的 processlist_id 关联的连接。|
|*mysql. az_kill_query*|processlist_id|N/A|等效于[`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html)命令。 将终止当前正在执行连接的语句。 保持连接的活动状态。|
|*mysql. az_load_timezone*|N/A|N/A|加载时区表以允许 `time_zone` 参数设置为命名值（例如 "美国/太平洋"）。|

## <a name="next-steps"></a>后续步骤
- 了解如何设置[数据传入复制](howto-data-in-replication.md)
- 了解如何使用时区[表](howto-server-parameters.md#working-with-the-time-zone-parameter)
