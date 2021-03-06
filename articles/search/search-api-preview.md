---
title: REST API 版本 2019-05-06-预览版
titleSuffix: Azure Cognitive Search
description: Azure 认知搜索 service REST API 版本 2019-05-06-Preview 包括试验性功能，如用于增量扩充的知识存储和索引器缓存。
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/04/2020
ms.openlocfilehash: eb73d614ca94bc1fa007a14f3705e50c74ab9e4f
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2020
ms.locfileid: "75922477"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>Azure 认知搜索服务 REST api-版本 2019-05-06-预览版

本文介绍搜索服务 REST API 的 `api-version=2019-05-06-Preview` 版本，并提供尚未正式发布的实验性功能。

> [!NOTE]
> 预览功能可用于目的为收集反馈的测试和试验，可能随时更改。 强烈建议不要在生产应用程序中使用预览版 API。


## <a name="new-in-2019-05-06-preview"></a>2019-05-06 中的新增-预览

+ [增量扩充（预览版）](cognitive-search-incremental-indexing-conceptual.md)将缓存添加到扩充管道，这样，如果目标修改（如对技能组合或其他对象的更新）不更改内容，则可以重复使用现有输出。 缓存仅适用于技能组合生成的丰富文档。

+ [Cosmos DB 索引器](search-howto-index-cosmosdb.md)支持 MongoDB api （预览）、Gremlin api （预览）和 Cassandra API （预览）。

+ [Azure Data Lake Storage Gen2 索引器（预览版）](search-howto-index-azure-data-lake-storage.md)可通过 Data Lake Storage Gen2 为内容和元数据编制索引。

+ [文档提取（预览版）](cognitive-search-skill-document-extraction.md)是在编制索引期间使用的一种认知技巧，可用于从技能组合中提取文件的内容。 以前，文档破解仅在技能组合执行之前发生。 添加了这一技能后，还可以在执行技能组合的过程中执行此操作。

+ [文本翻译（预览）](cognitive-search-skill-text-translation.md)是在编制索引时使用的一项认知技巧，用于计算文本，对于每个记录，将返回转换为指定目标语言的文本。

+ [知识存储](knowledge-store-concept-intro.md)是基于 AI 的扩充管道的新目标。 物理数据结构存在于 Azure Blob 存储和 Azure 表存储中，并在运行具有附加认知技能组合的索引器时创建并填充。 在技能组合定义内指定知识存储本身的定义。 在知识存储定义中，通过确定数据的形状、数据是否存储在表存储或 Blob 存储中，以及是否有多个视图的*投影*元素来控制数据的物理结构。

## <a name="earlier-preview-features"></a>更早的预览功能

公共预览版中仍包含早期预览版中发布的功能。 如果你正在调用使用较早预览版 API 版本的 API，可以继续使用该版本或切换到 `2019-05-06-Preview`，而不会对预期行为作出任何更改。

+ [moreLikeThis 查询参数](search-more-like-this.md)查找与特定文档相关的文档。 早期预览版中已有此功能。 

+ [CSV blob 索引](search-howto-index-csv-blobs.md)为每行创建一个文档，而不是每个文本 blob 创建一个文档。

## <a name="how-to-call-a-preview-api"></a>如何调用预览版 API

早期预览版仍然可用，但随着时间推移会变得过时。 如果代码调用 `api-version=2016-09-01-Preview` 或 `api-version=2017-11-11-Preview`，那些调用仍然有效。 但是，只有最新预览版会获得改进。 

以下示例语法说明了对预览 API 版本的调用。

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure 认知搜索服务在多个版本中可用。 有关详细信息，请参阅 [API 版本](search-api-versions.md)。

## <a name="next-steps"></a>后续步骤

查看搜索 REST API 参考文档。 如果遇到问题，请向我们提供有关[StackOverflow](https://stackoverflow.com/)的帮助或[联系支持人员](https://azure.microsoft.com/support/community/?product=search)。

> [!div class="nextstepaction"]
> [搜索服务 REST API 引用](https://docs.microsoft.com/rest/api/searchservice/)