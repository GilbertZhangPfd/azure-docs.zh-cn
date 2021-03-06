---
title: Azure 实验室服务中的示例类类型 | Microsoft Docs
description: 提供可以使用 Azure 实验室服务为其设置实验室的某些类型的类。
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: e1c5504b30c2784e8657ccc0dc4ec18689fe2a68
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806807"
---
# <a name="class-types-overview---azure-lab-services"></a>类类型概述 - Azure 实验室服务

使用 Azure 实验室服务可以在云中快速设置课堂实验室环境。 本部分列出的文章提供了有关如何使用 Azure 实验室服务设置多种类型的课堂实验室的指导。

## <a name="deep-learning-in-natural-language-processing"></a>自然语言处理中的深度学习

可以使用 Azure 实验室服务来设置一个专注于自然语言处理 (NLP) 中的深度学习的实验室。 自然语言处理 (NLP) 是某种形式的人工智能 (AI)，可在计算机中实现翻译、语音识别和其他语言理解功能。 使用 NLP 类的学生可以通过 Linux 虚拟机 (VM) 了解如何应用神经网络算法，以开发深度学习模型用于分析人类手写语言。

有关如何设置此类实验室的详细信息，请参阅[使用 Azure 实验室服务设置专注于自然语言处理中的深度学习的实验室](class-type-deep-learning-natural-processing.md)。

## <a name="shell-scripting-on-linux"></a>Linux 上的 Shell 脚本

可以设置一个实验室来讲解 Linux 上的 shell 脚本编写。 脚本编写是系统管理的有用组成部分，可让管理员避免重复性的任务。 在此示例场景中，类涵盖了传统的 bash 脚本和增强的脚本。 增强的脚本是结合了 bash 命令和 Ruby 的脚本。 这样，Ruby 便可以传递数据和 bash 命令来与 shell 交互。

使用这些脚本类的学生可以通过 Linux 虚拟机了解 Linux 的基础知识，并熟悉 bash shell 脚本。 该 Linux 虚拟机已启用远程桌面访问，并装有 [gedit](https://help.gnome.org/users/gedit/stable/) 和 [Visual Studio Code](https://code.visualstudio.com/) 文本编辑器。

有关如何设置此类实验室的详细信息，请参阅 [Linux 上的 Shell 脚本](class-type-shell-scripting-linux.md)。

## <a name="ethical-hacking"></a>道德黑客攻击

可以为专注于道德黑客取证方面的课程设置实验室。 渗透测试是道德黑客社区使用的一种做法，当某人试图获得对系统或网络的访问权限以证明恶意攻击者可能利用的漏洞时，就会进行渗透测试。

在道德黑客课程中，学生可以学习抵御漏洞的新式技术。 每个学生都获得一个 Windows Server 主机虚拟机，它包含两个嵌套虚拟机 - 一个是带有 [Metasploitable3](https://github.com/rapid7/metasploitable3) 映像的虚拟机，另一个是带有 [Kali Linux](https://www.kali.org/) 映像的虚拟机。 Metasploitable 虚拟机用于开发目的。  Kali Linux 虚拟机用于访问执行取证任务所需的工具。

有关如何设置此类实验室的详细信息，请参阅[设置实验室以教授道德黑客课程](class-type-ethical-hacking.md)。

## <a name="next-steps"></a>后续步骤

请参阅以下文章：

- [使用 Azure 实验室服务设置专注于自然语言处理中的深度学习的实验室](class-type-deep-learning-natural-processing.md)
- [Linux 上的 Shell 脚本](class-type-shell-scripting-linux.md)
- [道德黑客攻击](class-type-ethical-hacking.md)
