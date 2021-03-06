---
title: 所有 Azure 安全中心警报的引用表
description: 本文列出了 Azure 安全中心的威胁防护模块中可见的安全警报。
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: 4ef2987ee72348fb4353ba735d6da76fb218f01e
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942141"
---
# <a name="security-alerts---a-reference-guide"></a>安全警报-参考指南

本文列出了你可能在 Azure 安全中心的威胁防护模块中看到的安全警报。 环境中显示的警报取决于要保护的资源和服务，以及自定义的配置。

若要了解如何响应这些警报，请参阅[管理和响应 Azure 安全中心的安全警报](security-center-managing-and-responding-alerts.md)。

若要了解如何导出警报（和建议），请参阅[导出安全警报和建议（预览版）](continuous-export.md)。

警报表下面是一个表，用于描述用于对这些警报的目的进行分类的 Azure 安全中心终止链。 

## <a name="azure-security-center-alerts"></a>Azure 安全中心警报

|警报|Description|意向（[了解详细信息](#intentions)）|
|----|----|:----:|
||<a name="alerts-windows"></a><h3>Windows 计算机</h3> [更多详细信息和说明](security-center-alerts-iaas.md#windows-)||
|**已发现代码注入**|代码注入是将可执行模块插入到正在运行的进程或线程中。 恶意软件使用此方法来访问数据，同时成功地隐藏自身以防止找到和删除。<br>此警报指示故障转储中存在注入模块。 为了区分恶意和非恶意注入的模块，安全中心会检查注入的模块是否符合可疑行为的配置文件。|-|
|**检测到可疑的代码段**|指示已使用非标准方法（如反射注入和进程替换所）分配了代码段。 该警报提供代码段的其他特征，这些特征已经过处理，可为所报告代码段的功能和行为提供上下文。|-|
|**已发现外壳代码**|Shellcode 是在恶意软件利用软件漏洞之后运行的有效负载。<br>此警报指示故障转储分析检测到可执行代码，该代码展示了恶意有效负载通常会执行的行为。 尽管非恶意软件也可以执行此行为，但它不是一般的软件开发实践。|-|
|**检测到的 Fileless 攻击方法**|指定进程的内存包含 fileless 攻击工具包： Meterpreter。 Fileless 攻击工具包通常不会在文件系统上存在，这使得传统的防病毒软件难以检测。|DefenseEvasion/执行|
||<a name="alerts-linux"></a><h3>Linux 计算机</h3> [更多详细信息和说明](security-center-alerts-iaas.md#linux-)||
|**以异常方式访问 SSH 授权密钥文件的进程**|已在类似于已知恶意软件活动的方法中访问了 SSH 授权密钥文件。 此访问可能表明攻击者正尝试获取计算机的持久访问权限。|-|
|**检测到的永久性尝试**|主机数据分析检测到已安装了单用户模式的启动脚本。<br>由于在此模式下运行时，很少需要合法的进程，这可能表明攻击者已向每个运行级别添加了恶意进程以保证持久性。 |持久性|
|**检测到计划任务的操作**|主机数据分析检测到计划任务的可能操作。 攻击者通常会将计划的任务添加到其泄露的计算机上以获得持久性。|持久性|
|**可疑文件时间戳修改**|主机数据分析检测到可疑的时间戳修改。 攻击者经常将时间戳从现有的合法文件复制到新工具，以避免检测到这些新删除的文件。|持久性/DefenseEvasion|
|**已将新用户添加到 sudoers 组**|主机数据分析检测到已将用户添加到 sudoers 组，从而使其成员能够运行具有高权限的命令。|PrivilegeEscalation|
|**检测到与数字货币挖掘关联的进程**|主机数据分析检测到通常与数字货币挖掘关联的进程的执行。|利用/执行|
|**可能端口转发到外部 IP 地址**|主机数据分析检测到启动了到外部 IP 地址的端口转发。|渗透/CommandAndControl|
||<a name="alerts-azureappserv"></a><h3>Azure App Service</h3> [更多详细信息和说明](security-center-alerts-compute.md#azure-app-service-)||
|**检测到可疑的 WordPress 主题调用**|应用服务活动日志表示应用服务资源上可能的代码注入活动。<br>此可疑活动类似于操作 WordPress 主题以支持服务器端代码执行的活动，后跟一个直接 web 请求来调用操作的主题文件。 此类活动可以是通过 WordPress 的攻击活动的一部分。|-|
|**检测到 Web 指纹**<br>（NMAP/盲大象）|应用服务活动日志指出了应用服务资源上可能存在的 web 指纹活动。<br>此可疑活动与名为盲大象的工具相关联。 工具指纹 web 服务器并尝试检测已安装的应用程序及其版本。 攻击者通常使用此工具来探测 web 应用程序以查找漏洞。 |-|
|**检测到可能易受到攻击的网页的可疑访问**|应用服务活动日志表明访问了看似敏感的网页。<br>此可疑活动源自其访问模式与 web 扫描程序类似的源地址。 这种活动通常与攻击者尝试扫描您的网络以尝试访问敏感或易受攻击的网页有关。 |-|
|**在威胁智能中找到连接到 Azure App Service FTP 接口的 IP**|应用服务 FTP 日志分析检测到来自威胁情报源中找到的源地址的连接。 在此连接期间，用户访问了列出的页面。|-|
|**尝试在 Windows 应用服务上运行 Linux 命令**|分析应用服务进程检测到尝试在 Windows 应用服务上运行 Linux 命令。 此操作已被 web 应用程序运行。 此行为在市场活动中经常出现，利用公共 web 应用程序中的漏洞。|-|
|**检测到可疑 PHP 执行**|计算机日志指示可疑 PHP 进程正在运行。 操作包含尝试使用 PHP 进程从命令行运行操作系统命令或 PHP 代码。 虽然这种行为是合法的，但在 web 应用程序中，这种行为可能表明恶意活动，例如尝试通过 web 外壳感染网站。|执行|
|**从临时文件夹执行进程**|应用服务流程分析检测到从应用的临时文件夹执行进程。 尽管此行为很合理，但在 web 应用程序中，这种行为可能表明存在恶意活动。|-|
|**检测到尝试运行高特权命令**|分析应用服务进程检测到尝试运行需要高权限的命令。 命令在 web 应用程序上下文中运行。 尽管此行为很合理，但在 web 应用程序中，这种行为可能表明存在恶意活动。|-|
|**检测到磁盘上检测到磁盘的卷输出**|分析应用服务进程检测到正在运行将输出保存到磁盘的卷命令。 虽然这种行为是合法的，但在 web 应用程序中，这种行为也会在恶意活动中被发现，如尝试利用 web shell 感染网站。|-|
|**检测到原始数据下载**|分析应用服务进程检测到从原始-数据网站（如 Pastebin）下载代码的尝试。 此操作是由 PHP 进程运行的。 此行为与尝试将 web shell 或其他恶意组件下载到应用服务的操作相关联。|-|
|**检测到漏洞扫描程序**<br>（Joomla/WordPress/CMS）|Azure App Service 活动日志表示应用服务资源上使用了可能的漏洞扫描程序。 检测到的可疑活动类似于面向 Joomla 应用程序/WordPress 应用程序/内容管理系统（CMS）的工具。|-|
|**检测到垃圾邮件文件夹引用**|Azure App Service 活动日志指示标识为来自与垃圾邮件活动关联的网站的 web 活动。 如果你的网站遭到入侵并用于垃圾邮件活动，就会发生这种情况。|-|
|**检测到从异常 IP 地址连接到网页**|"Azure App Service 活动日志" 表示从以前从未连接到的源 IP 地址（% {Source IP Address}）到敏感网页的连接。 这可能表示有人正在尝试对你的 web 应用管理页面进行暴力攻击。 它也可能是合法用户使用的新 IP 地址的结果。|-|
|**检测到可疑的用户代理**|Azure App Service 活动日志指示具有可疑用户代理的请求。 此行为可以指示尝试利用应用服务应用程序中的漏洞。|-|
|**上传文件夹中的 PHP 文件**|Azure App Service 活动日志指示对位于上传文件夹中的可疑 PHP 页面的访问。 这种类型的文件夹通常不包含 PHP 文件。 存在这种类型的文件可能表示利用了任意文件上传漏洞的利用。|-|
|**检测到异常请求模式**|Azure App Service 活动日志指示应用服务来自% {Source IP} 的异常 HTTP 活动。 此活动类似于模糊化 \ 强力强制活动模式。|-|
||<a name="alerts-akscluster"></a><h3>AKS 群集级别</h3> [更多详细信息和说明](security-center-alerts-compute.md#azure-containers-)||
|**预览-检测到群集-管理员角色的角色绑定**|Kubernetes 审核日志分析检测到群集管理角色的新绑定，导致管理员权限。 不必要地提供管理员权限可能会导致群集中的权限提升问题。|持久性|
|**检测到预览版的 Kubernetes 仪表板**|Kubernetes 审核日志分析检测到由 LoadBalancer 服务公开的 Kubernetes 仪表板。 公开的仪表板允许未经身份验证的群集管理访问，并带来安全威胁。|持久性|
|**预览-检测到新的高特权角色**|Kubernetes 审核日志分析检测到具有高权限的新角色。 与具有高权限的角色的绑定在群集中为用户/组提升的权限。 不必要地提供提升的权限可能会导致群集中的权限提升问题。|持久性|
|**预览-检测到 kube 命名空间中的新容器**|Kubernetes 审核日志分析检测到 kube 命名空间中的新容器，该容器不在通常在此命名空间中运行的容器中。 Kube 命名空间不应包含用户资源。 攻击者可以使用此命名空间隐藏恶意组件。|持久性|
|**预览-检测到数字货币挖掘容器**|Kubernetes 审核日志分析检测到一个容器具有与数字货币挖掘工具关联的图像。|执行|
|**预览-检测到特权容器**|Kubernetes 审核日志分析检测到新的特权容器。 特权容器可以访问节点的资源，并打破容器之间的隔离。 如果受到侵害，攻击者可以使用特权容器获取对节点的访问权限。|PrivilegeEscalation|
|**预览-检测到敏感卷装载的容器**|Kubernetes 审核日志分析检测到具有敏感卷装入的新容器。 检测到的卷是将敏感文件或文件夹从节点装入容器的 hostPath 类型。 如果容器被泄露，则攻击者可以使用此装载获取对节点的访问权限。|PrivilegeEscalation|
||<a name="alerts-containerhost"></a><h3>容器主机级别</h3> [更多详细信息和说明](security-center-alerts-compute.md#azure-containers-)||
|**检测到特权容器**|计算机日志指示特权 Docker 容器正在运行。 特权容器对宿主的资源具有完全访问权限。 如果受到侵害，攻击者可以使用特权容器获取对主机的访问权限。|PrivilegeEscalation/执行|
|**特权命令在容器中运行**|计算机日志指示已在 Docker 容器中运行特权命令。 特权命令具有主机上的扩展权限。|PrivilegeEscalation|
|**检测到暴露的 Docker 守护程序**|计算机日志指示 Docker 守护程序（dockerd.exe）公开 TCP 套接字。 默认情况下，启用 TCP 套接字后，Docker 配置不使用加密或身份验证。 可以访问相关端口的任何人均可获取对 Docker 守护程序的完全访问权限。|利用/执行|
|**SSH 服务器在容器中运行**|计算机日志表示 SSH 服务器在 Docker 容器中运行。 尽管此行为是有意的，但它通常表示容器配置不正确或被破坏。|执行|
|**检测到挖掘器映像的容器**|计算机日志表示执行运行与数字货币挖掘关联的映像的 Docker 容器。 此行为可能表示资源被滥用。|执行|
|**Kubernetes API 的可疑请求**|计算机日志表示对 Kubernetes API 发出了可疑的请求。 请求是从 Kubernetes 节点发送的，该节点可能来自节点中运行的某个容器。 尽管此行为是有意的，但它可能指示节点正在运行已泄露的容器。|执行|
|**向 Kubernetes 仪表板发出可疑请求**|计算机日志指示对 Kubernetes 仪表板发出了可疑的请求。 请求是从 Kubernetes 节点发送的，该节点可能来自节点中运行的某个容器。 尽管此行为是有意的，但它可能指示节点正在运行已泄露的容器。|-|
||<a name="alerts-sql-db-and-warehouse"></a><h3>SQL 数据库和 SQL 数据仓库</h3> [更多详细信息和说明](security-center-alerts-data-services.md#sql-database-and-sql-data-warehouse-) ||
|**可能的 SQL 注入漏洞**|应用程序在数据库中生成了错误的 SQL 语句。 这可能表示存在 SQL 注入攻击的漏洞。 错误的语句有两个可能的原因。 应用程序代码中的缺陷可能构造了错误的 SQL 语句。 或者，应用程序代码或存储过程在构造出现错误的 SQL 语句时，不会清理用户输入，这可被用于 SQL 注入。|-|
|**潜在 SQL 注入**|对已识别的应用程序进行主动攻击时，易受 SQL 注入攻击。 这意味着攻击者正尝试使用有漏洞的应用程序代码或存储过程注入恶意 SQL 语句。|-|
|**从异常位置登录**|访问模式已更改为 SQL Server，其中有人从异常的地理位置登录到了服务器。 在某些情况下，警报会检测合法操作（发布新应用程序或开发人员维护）。 在其他情况下，警报会检测恶意操作（以前的员工或外部攻击者）。|漏洞利用|
|**不熟悉的主体登录**|访问模式已更改为 SQL Server。 有人使用异常的主体（用户）登录到服务器。 在某些情况下，警报会检测合法操作（发布新应用程序或开发人员维护）。 在其他情况下，警报会检测恶意操作（以前的员工或外部攻击者）。|漏洞利用|
|**尝试由可能有害的应用程序登录**|已使用可能有害的应用程序访问数据库。 在某些情况下，警报会检测操作中的渗透测试。 在其他情况下，警报会检测使用常见工具的攻击。|探测|
|**潜在的 SQL 暴力尝试尝试**|发生了具有不同凭据的异常数量异常的登录失败。 在某些情况下，警报会检测操作中的渗透测试。 在其他情况下，警报会检测暴力破解攻击。|探测|
|**从异常的 Azure 数据中心登录**|对 SQL Server 的访问模式进行了更改，在这种情况下，有人从异常的 Azure 数据中心登录到了服务器。 在某些情况下，警报会检测合法操作（新应用程序或 Azure 服务）。 在其他情况下，警报会检测恶意操作（攻击者在 Azure 中从违例资源运行）。|探测|
|**可能不安全的操作**|在 SQL Server 中，经常在恶意会话中使用的高特权 SQL 命令已执行。 建议默认禁用这些命令。 在某些情况下，警报会检测合法操作（运行管理脚本）。 在其他情况下，警报会检测恶意操作（攻击者使用 SQL 信任来破坏 Windows 层）。|执行|
|**异常导出位置**|SQL 导入和导出操作的导出存储目标发生了更改。 在某些情况下，警报会检测到合法的更改（新的备份目标）。 在其他情况下，警报会检测恶意操作（攻击者很容易将数据 exfiltrated 到文件中）。|渗透|
||<a name="alerts-azurestorage"></a><h3>Azure 存储器</h3> [更多详细信息和说明](security-center-alerts-data-services.md#azure-storage-)||
|**从异常位置访问存储帐户**|指示访问模式对 Azure 存储帐户进行了更改。 与最近的活动进行比较时，有人从一个 IP 地址访问此帐户，该地址被视为不熟悉。 攻击者已获得帐户的访问权限，或者合法用户已从新的或异常的地理位置进行连接。 后者的一个示例是从新的应用程序或开发人员进行远程维护。|漏洞利用|
|**异常应用程序访问了存储帐户**|指示异常应用程序已访问此存储帐户。 可能的原因是攻击者通过使用新的应用程序访问了你的存储帐户。|漏洞利用|
|**匿名访问存储帐户**|指示存储帐户的访问模式发生更改。 例如，帐户已被匿名访问（无需任何身份验证），这与此帐户上的最新访问模式有意外的对比。 可能的原因是攻击者利用了对保存 blob 存储的容器的公共读取访问权限。|漏洞利用|
|**从 Tor exit 节点访问存储帐户**|指示已从称为 Tor 的活动退出节点的 IP 地址（匿名代理）成功访问此帐户。 此警报的严重性考虑使用的身份验证类型（如果有），以及这是否是此类访问的第一种情况。 可能的原因可能是已使用 Tor 访问了你的存储帐户的攻击者，或者是通过使用 Tor 访问了你的存储帐户的合法用户。|探测/利用|
|**从存储帐户提取的异常数据量**|表示与此存储容器上的最近活动相比，已提取异常大量的数据。 可能的原因是，攻击者从容纳 blob 存储的容器中提取了大量数据。|渗透|
|**存储帐户中的异常删除**|指示存储帐户中发生了一个或多个意外的删除操作，与此帐户上的最近活动相比。 可能的原因是攻击者已从存储帐户中删除数据。|渗透|
|**将 .cspkg 的异常上传到存储帐户**|表示 Azure 云服务包（.cspkg 文件）已以异常方式上传到存储帐户，与此帐户上的最近活动相比。 可能的原因是攻击者已准备好将恶意代码从存储帐户部署到 Azure 云服务。|LateralMovement/执行|
|**将 .exe 的异常上传到存储帐户**|表示与此帐户上的最近活动相比，.exe 文件已以异常方式上传到存储帐户。 可能的原因是攻击者已将恶意可执行文件上传到存储帐户，或者合法用户已上传可执行文件。|LateralMovement/执行|
|**存储帐户中访问权限的异常更改**|指示此存储容器的访问权限已以异常方式更改。 可能的原因是攻击者已更改容器权限以降低其安全状况或获取持久性。|持久性|
|**存储帐户中的异常访问检查**|表示与此帐户上的最近活动相比，对存储帐户的访问权限的检查方式与此相同。 一个可能的原因是攻击者为将来的攻击执行了侦测。|集合|
|**存储帐户中的异常数据浏览**|表示与此帐户上的最近活动相比，存储帐户中的 blob 或容器已以异常方式进行枚举。 一个可能的原因是攻击者为将来的攻击执行了侦测。|集合|
|**预览-可能已上传到存储帐户的恶意软件**|指示包含潜在恶意软件的 blob 已上传到存储帐户。 可能的原因包括攻击者或无意中上传恶意软件（由合法用户进行）。|LateralMovement|
||<a name="alerts-azurecosmos"></a><h3>Azure Cosmos DB</h3> [更多详细信息和说明](security-center-alerts-data-services.md#azure-cosmos-db)||
|**从异常位置访问 Cosmos DB 帐户**|指示 Azure Cosmos DB 帐户的访问模式发生了更改。 用户已从不熟悉的 IP 地址（与最近的活动）访问了此帐户。 攻击者已访问该帐户，或者合法用户已从新的和不正常的地理位置访问了它。 后者的一个示例是从新的应用程序或开发人员进行远程维护。|漏洞利用|
|**从 Cosmos DB 帐户提取的异常数据量**|指示 Azure Cosmos DB 帐户的数据提取模式发生了更改。 与最近的活动相比，有人提取了异常的数据量。 攻击者可能已从 Azure Cosmos DB 数据库（例如，数据渗透或泄露或未经授权的数据传输）提取了大量数据。 或者，合法用户或应用程序可能已从容器提取了异常数据量（例如，对于维护备份活动）。|渗透|
||<a name="alerts-azurenetlayer"></a><h3>Azure 网络层</h3> [更多详细信息和说明](security-center-alerts-service-layer.md#azure-network-layer)||
|**建议用于阻止的 IP 地址的流量**|Azure 安全中心检测到来自建议被阻止的 IP 地址的入站流量。 这通常发生在此 IP 地址不会与此资源定期通信的情况下。 或者，安全中心的威胁智能源将 IP 地址标记为 "恶意"。|探测|
|**可疑的传出 RDP 网络活动**|采样的网络流量分析检测到异常传出远程桌面协议（RDP）通信，源自部署中的资源。 此环境的此活动被视为不正常。 这可能表示资源已泄露，并且现在正在用于对外部 RDP 终结点进行暴力攻击。 这种类型的活动可能会导致你的 IP 被外部实体标记为恶意。|-|
|**到多个目标的可疑传出 RDP 网络活动**|采样网络流量分析检测到异常传出 RDP 通信，从部署中的资源到多个目标。 此环境的此活动被视为不正常。 这可能表示资源已泄露，并且现在正被用于暴力攻击外部 RDP 终结点。 这种类型的活动可能会导致你的 IP 被外部实体标记为恶意。|-|
|**可疑的传出 SSH 网络活动**|采样的网络流量分析检测到异常传出安全外壳（SSH）通信，该通信源自于你的部署中的资源。 此环境的此活动被视为不正常。 这可能表示资源已泄露，并且现在正在用于对外部 SSH 终结点进行暴力攻击。 这种类型的活动可能会导致你的 IP 被外部实体标记为恶意。|-|
|**到多个目标的可疑传出 SSH 网络活动**|采样网络流量分析检测到异常传出 SSH 通信，从部署中的资源到多个目标。 此环境的此活动被视为不正常。 这可能表示资源已泄露，并且现在正被用于暴力攻击外部 SSH 终结点。 这种类型的活动可能会导致你的 IP 被外部实体标记为恶意。|-|
|**来自多个源的可疑传入 SSH 网络活动**|采样网络流量分析检测到从多个源到部署中资源的异常传入 SSH 通信。 在此环境下，连接到资源的不同唯一 Ip 被视为异常。 此活动可能表示尝试从多个主机（僵尸网络）对 SSH 接口进行暴力攻击。|-|
|**可疑的传入 SSH 网络活动**|采样网络流量分析检测到与部署中的资源的异常传入 SSH 通信。 在此环境中，与资源相对较多的传入连接被视为不正常。 此活动可能表示尝试暴力破解 SSH 接口。|-|
|**来自多个源的可疑传入 RDP 网络活动**|采样网络流量分析检测到从多个源到部署中资源的异常传入 RDP 通信。 在此环境下，连接到资源的不同唯一 Ip 被视为异常。 此活动可能表示尝试从多个主机（僵尸网络）对 RDP 接口进行暴力攻击。|-|
|**可疑的传入 RDP 网络活动**|采样网络流量分析检测到与部署中的资源的异常传入 RDP 通信。 在此环境中，与资源相对较多的传入连接被视为不正常。 此活动可能表示尝试暴力破解 SSH 接口。|-|
|**检测到与恶意计算机的网络通信**|采样网络流量分析检测到的通信是使用可能的命令和控制（C & C）服务器从你的部署中的某个资源发起的通信。 这种类型的活动可能会导致你的 IP 被外部实体标记为恶意。|-|
||<a name="alerts-azureresourceman"></a><h3>Azure 资源管理器（预览版）</h3> [更多详细信息和说明](security-center-alerts-service-layer.md#azure-management-layer-azure-resource-manager-preview)||
|**已检测到 MicroBurst 工具包函数运行**|已在你的环境中检测到一个已知的云环境侦测工具包。 攻击者（或渗透测试人员）可以使用工具[MicroBurst](https://github.com/NetSPI/MicroBurst)来映射订阅的资源、标识不安全的配置以及泄露机密信息。|-|
|**已检测到 Azurite 工具包**|已在你的环境中检测到一个已知的云环境侦测工具包。 攻击者（或渗透测试人员）可以使用工具[Azurite](https://github.com/mwrlabs/Azurite)来映射订阅的资源并标识不安全的配置。|-|
|**预览-检测到使用非活动帐户的可疑管理会话**|订阅活动日志分析检测到可疑行为。 一段很长一段时间内未使用的主体现在正在执行可确保攻击者持久性的操作。|持久性|
|**预览-检测到使用 PowerShell 的可疑管理会话**|订阅活动日志分析检测到可疑行为。 不经常使用 PowerShell 来管理订阅环境的主体现在使用 PowerShell，并执行可保护攻击者持久性的操作。|持久性|
|**使用高级 Azure 持久性技术**|订阅活动日志分析检测到可疑行为。 自定义角色已被 legitimized 标识实体提供。 这可能会使攻击者在 Azure 客户环境中获得持久性。|-|
|**来自不常见国家/地区的活动**|来自组织中任何用户最近或从未访问过的位置中的活动。<br>此项检测考虑过去的活动位置，以确定新的和不常见的位置。 异常情况检测引擎将存储组织中用户以往用过的位置的相关信息。|-|
|**来自匿名 IP 地址的活动**|检测到来自已被标识为匿名代理 IP 地址的 IP 地址的用户活动。<br>要隐藏其设备 IP 地址的用户使用这些代理，并可用于恶意目的。 此检测使用机器学习算法，该算法可减少误报，例如组织中用户广泛使用的、带有错误标记的 IP 地址。|-|
|**无法进行的旅行活动**|发生了两个用户活动（在单个或多个会话中），源自地理位置遥远的位置。 这种情况发生在短于用户从第一个位置到达第二个位置所需的时间段内。 这表示其他用户正在使用相同的凭据。<br>此检测使用机器学习算法，该算法会忽略导致不可能旅行情况的明显误报，如组织中其他用户定期使用的 Vpn 和位置。 该检测具有7天的初始学习期间，在此期间，它将学习新用户的活动模式。 |-|
||<a name="alerts-azurekv"></a><h3>Azure Key Vault （预览）</h3> [更多详细信息和说明](security-center-alerts-service-layer.md#azure-keyvault)||
|**从 TOR exit 节点访问 Key Vault**|使用 TOR IP 匿名系统的用户访问了 Key Vault，以隐藏其位置。 当尝试获取对连接到 internet 的资源的未经授权的访问权限时，恶意执行组件通常会尝试隐藏其位置。|-|
|**Key Vault 中的可疑策略更改和机密查询**|已进行 Key Vault 策略更改，然后执行列出和/或获取机密操作的操作。 此外，用户通常不会对此保管库执行此操作模式。 这强烈表明 Key Vault 受到威胁，恶意的执行组件已经窃取了中的机密。|-|
|**Key Vault 中的可疑机密列表和查询**|密码列表操作后跟许多机密 Get 操作。 此外，用户通常不会对此保管库执行此操作模式。 这表明有人可以转储存储在 Key Vault 中的机密，以实现潜在的恶意目的。|-|
|**访问 Key Vault 的异常用户-应用程序对**|Key Vault 已被通常不访问的用户应用程序配对访问。 这可能是合法的访问尝试（例如，在基础结构或代码更新后）。 这也可能表示你的基础结构受到危害，恶意执行组件正在尝试访问你的 Key Vault。|-|
|**异常应用程序访问 Key Vault**|通常不能访问 Key Vault 的应用程序已访问了该。 这可能是合法的访问尝试（例如，在基础结构或代码更新后）。 这也可能表示你的基础结构受到危害，恶意执行组件正在尝试访问你的 Key Vault。|-|
|**异常用户访问了 Key Vault**|Key Vault 已被通常不访问的用户访问。 这可能是合法的访问尝试（例如，需要访问的新用户已加入组织）。 这也可能表示你的基础结构受到危害，恶意执行组件正在尝试访问你的 Key Vault。|-|
|**Key Vault 中的异常操作模式**|与历史数据相比较，执行了一组异常的 Key Vault 操作。 Key Vault 活动通常在一段时间内是相同的。 这可能是活动的合法更改。 或者，你的基础结构可能已泄露，并且需要进一步调查。|-|
|**Key Vault 中的大量操作**|与历史数据相比，已经执行了更大量的 Key Vault 操作。 Key Vault 活动通常在一段时间内是相同的。 这可能是活动的合法更改。 或者，你的基础结构可能已泄露，并且需要进一步调查。|-|
|**用户访问了大量的密钥保管库**|与历史数据相比，用户或应用程序访问的保管库的数目已更改。 Key Vault 活动通常在一段时间内是相同的。 这可能是活动的合法更改。 或者，你的基础结构可能已泄露，并且需要进一步调查。|-|
||<a name="alerts-azureddos"></a><h3>Azure DDoS 保护</h3> [更多详细信息和说明](security-center-alerts-integration.md#azure-ddos)||
|**检测到公共 IP 的 DDoS 攻击**|检测到公共 IP （IP 地址）的 DDoS 攻击，并被减轻。|探测|
|**为公共 IP 缓解 DDoS 攻击**|公共 IP （IP 地址）对 DDoS 攻击的缓解。|探测|
|**检测到容量耗尽攻击**|这种攻击的目标是通过大量看似合法的通信淹没网络层。 它包括 UDP 洪水、放大洪水以及其他欺骗性数据包洪水。 "DDoS 保护" 标准通过使用全局网络缩放自动吸收和清理这些潜在的多字节攻击，从而缓解这些攻击。|-|
|**检测到协议攻击**|这些攻击通过利用第3层和第4层协议堆栈中的漏洞，使目标无法访问。 它包括 SYN 洪水攻击、反射攻击和其他协议攻击。 DDoS 保护标准通过与客户端交互来区分恶意流量和合法流量并阻止恶意流量，从而缓解这些攻击。|-|
|**检测到资源（应用程序）层攻击**|这些攻击以 web 应用程序数据包为目标来中断主机之间的数据传输。 这些攻击包括 HTTP 协议冲突、SQL 注入、跨站点脚本和其他第 7 层攻击。 使用带有 DDoS 保护标准的 Azure 应用程序网关 WAF 来防范这些攻击。 Azure Marketplace 还提供了第三方 WAF 产品/服务。|-|
||||


## <a name="intentions"></a>举动

了解攻击的意图可帮助您更轻松地调查和报告事件。 Azure 安全中心警报包括 "目的" 字段来帮助进行这些工作。

描述受到从侦测到数据渗透的进度的一系列步骤通常称为 "终止链"。 

安全中心支持的 kill 链式方法基于[MITRE ATT & CK™框架](https://attack.mitre.org/matrices/enterprise/eh)，如下表中所述。

|Intent|Description|
|------|-------|
|**探测**|探测可以是尝试访问某个资源，而不考虑恶意意向，或尝试获取目标系统的访问权限，以在利用之前收集信息。 通常，此步骤被检测为从网络外部发起的尝试，用于扫描目标系统并标识入口点。|
|**受到**|利用这一阶段，攻击者可设法获得攻击资源的据点。 此阶段与计算主机和资源（例如用户帐户、证书等）相关。威胁参与者在此阶段之后常常能够控制资源。|
|**持久性**|持久性是对系统的任何访问、操作或配置更改，使威胁参与者在该系统上具有持久的状态。 威胁参与者通常需要通过中断来维护对系统的访问，例如系统重启、丢失凭据或其他可能需要远程访问工具重启的故障，或者提供备用后门来重新获得访问权限。|
|**PrivilegeEscalation**|权限升级是允许攻击者在系统或网络上获取更高级别的权限的操作结果。 某些工具或操作需要更高级别的特权才能工作，并且在操作过程中有可能在多个点执行此操作。 有权访问特定系统或执行攻击者所需的特定功能的用户帐户也可能被视为权限提升。|
|**DefenseEvasion**|防御规避包含一种技巧，攻击者可使用这些技术规避检测或避免其他防御措施。 有时，这些操作与其他类别中的技术（或不同的变体）相同，这些技术具有额外的破坏。|
|**CredentialAccess**|凭据访问表示一项技术，这些技术导致在企业环境中访问或控制系统、域或服务凭据。 攻击者可能会尝试从用户或管理员帐户（本地系统管理员或具有管理员访问权限的域用户）获取合法凭据，以便在网络中使用。 如果网络中有足够的访问权限，则攻击者可以创建帐户以供以后在环境中使用。|
|**发现**|发现包含允许攻击者获取有关系统和内部网络的知识的技术。 当攻击者获得对新系统的访问权限时，他们必须为其本身做出控制，以及从该系统运行的优点，并为其当前目标或在入侵期间的总体目标。 操作系统提供了许多可帮助此入侵后信息收集阶段的本机工具。|
|**LateralMovement**|横向移动包含一种技术，使攻击者能够访问和控制网络上的远程系统，但不一定包括在远程系统上执行工具。 横向移动技术可让攻击者从系统收集信息，而无需其他工具，如远程访问工具。 攻击者可以使用横向移动来实现多种目的，包括远程执行工具、旋转到其他系统、访问特定信息或文件、访问其他凭据或导致效果。|
|**执行**|执行策略表示可对本地或远程系统执行对手控制的代码的方法。 这一策略通常与横向移动一起使用，以扩展网络上远程系统的访问权限。|
|**集合**|集合包含用于在渗透之前从目标网络标识和收集信息（如敏感文件）的技术。 此类别还介绍了系统或网络上的一些位置，攻击者可能会在其中查找有关盗取的信息。|
|**泄露**|渗透是指使攻击者从目标网络中删除文件和信息的技术和特性。 此类别还介绍了系统或网络上的一些位置，攻击者可能会在其中查找有关盗取的信息。|
|**CommandAndControl**|命令和控制策略表示攻击者如何与目标网络中控制的系统进行通信。|
|**影响**|影响事件主要尝试直接降低系统、服务或网络的可用性或完整性;包括对数据的操作以影响业务或操作过程。 这通常是指勒索软件、篡改、数据操作等技术。|
||||


## <a name="next-steps"></a>后续步骤
若要了解有关警报的详细信息，请参阅以下内容：

* [Azure 安全中心中的安全警报](security-center-alerts-overview.md)
* [在 Azure 安全中心内管理和响应安全警报](security-center-managing-and-responding-alerts.md)
* [导出安全警报和建议（预览）](continuous-export.md)