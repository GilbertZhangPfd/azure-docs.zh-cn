---
title: 适用于虚拟机的 Azure 专用主机概述
description: 了解有关如何将 Azure 专用主机用于部署虚拟机的详细信息。
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/09/2020
ms.author: cynthn
ms.openlocfilehash: 81749b8fb8b9e3f1a10b65b758b532773bdebdac
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/10/2020
ms.locfileid: "75833875"
---
# <a name="azure-dedicated-hosts"></a>Azure 专用主机

Azure 专用主机是一项服务，可提供物理服务器，这些服务器可以承载一个或多个虚拟机-专用于一个 Azure 订阅。 专用主机是数据中心中使用的物理服务器，作为资源提供。 你可以在区域、可用性区域和容错域中预配专用主机。 然后，你可以将 Vm 直接置于预配的主机中，这种配置最适合你的需求。


[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>后续步骤

- 你可以使用[Azure PowerShell](dedicated-hosts-powershell.md)、[门户](dedicated-hosts-portal.md)和[Azure CLI](../linux/dedicated-hosts-cli.md)部署专用主机。

- [这里](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)有一个示例模板，它使用区域和容错域实现了区域中的最大复原能力。
