---
title: TuningOptions 元素 (DTA)
description: 在 dta 实用工具中，TuningOptions 元素包含用于特定优化会话的优化选项。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- TuningOptions element
ms.assetid: 58a22ba1-8e03-411f-bd46-85e4540f217a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 5ee768da96546cfcc4619098f5f0c962205047d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351111"
---
# <a name="tuningoptions-element-dta"></a>TuningOptions 元素 (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

包含用于特定优化会话的优化选项。  
  
## <a name="syntax"></a>语法  
  
```  
  
<DTAInput>  
    <Server>  
...code removed...  
    <Workload>...</Workload>  
    <TuningOptions>...</TuningOptions>  
```  
  
## <a name="element-characteristics"></a>元素特征  
  
|特征|说明|  
|--------------------|-----------------|  
|**数据类型和长度**|无。|  
|**默认值**|无。|  
|**出现次数**|可选。 如果已使用，则每个 **DTAInput** 元素只能使用一次。|  
  
## <a name="element-relationships"></a>元素关系  
  
|关系|元素|  
|------------------|--------------|  
|**父元素**|[DTAInput 元素 (DTA)](../../tools/dta/dtainput-element-dta.md)|  
|**子元素**|**ReportSet** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> **TuningLogTable** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> **NumberOfEvents** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> [TuningTimeInMin 元素 (DTA)](../../tools/dta/tuningtimeinmin-element-dta.md)<br /><br /> [StorageBoundInMB 元素 (DTA)](../../tools/dta/storageboundinmb-element-dta.md)<br /><br /> **MaxKeyColumnsInIndex** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> **MaxColumnsInIndex** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> **MinPercentageImprovement** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)<br /><br /> [TestServer 元素 (DTA)](../../tools/dta/testserver-element-dta.md)<br /><br /> [FeatureSet 元素 (DTA)](../../tools/dta/featureset-element-dta.md)<br /><br /> [分区元素 (DTA)](../../tools/dta/partitioning-element-dta.md)<br /><br /> [DropOnlyMode 元素 (DTA)](../../tools/dta/droponlymode-element-dta.md)<br /><br /> [KeepExisting 元素 (DTA)](../../tools/dta/keepexisting-element-dta.md)<br /><br /> [OnlineIndexOperation 元素 (DTA)](../../tools/dta/onlineindexoperation-element-dta.md)<br /><br /> [DatabaseToConnect 元素 (DTA)](../../tools/dta/databasetoconnect-element-dta.md)<br /><br /> **IgnoreConstantsInWorkload** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。<br /><br /> **RetainShellDB** 元素。 有关详细信息，请参阅 [数据库引擎优化顾问 XML 架构](https://go.microsoft.com/fwlink/?linkid=43100)。|  
  
## <a name="example"></a>示例  
 有关 **TuningOptions** 元素的示例 ，请参阅 [XML 输入文件示例 (DTA)](../../tools/dta/xml-input-file-samples-dta.md)。  
  
## <a name="see-also"></a>另请参阅  
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
