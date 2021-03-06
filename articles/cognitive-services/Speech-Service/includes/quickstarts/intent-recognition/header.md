---
title: 快速入门：识别语音、意向和实体 - 语音服务
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a87ed9355a5939393fd5e20f395cc96f35e7f150
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2019
ms.locfileid: "74816075"
---
在本快速入门中，我们将使用[语音 SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) 以交互方式识别从麦克风捕获的音频数据中的语音。 满足几个先决条件后，通过麦克风识别语音只需四个步骤：
> [!div class="checklist"]
>
> * 通过订阅密钥和区域创建 ````SpeechConfig```` 对象。
> * 使用以上的 ````SpeechConfig```` 对象创建 ````IntentRecognizer```` 对象。
> * 使用 ````IntentRecognizer```` 对象，开始单个言语的识别过程。
> * 检查返回的 ````IntentRecognitionResult````。
