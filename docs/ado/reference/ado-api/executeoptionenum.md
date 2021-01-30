---
description: ExecuteOptionEnum
title: ExecuteOptionEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ExecuteOptionEnum
helpviewer_keywords:
- ExecuteOptionEnum enumeration [ADO]
ms.assetid: 68bfa83a-5df4-4bef-8736-0f88ae8c29ea
author: rothja
ms.author: jroth
ms.openlocfilehash: f88d235c7ec38cd2f60ab5e07e1590f0f28e97ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167412"
---
# <a name="executeoptionenum"></a>ExecuteOptionEnum
指定提供程序执行命令的方式。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adAsyncExecute**|0x10|指示该命令应以异步方式执行。<br /><br /> 此值不能与 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) 值 **adCmdTableDirect** 组合。|  
|**adAsyncFetch**|0x20|指示应异步检索在 [CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md) 属性中指定的初始数量后的其余行。|  
|**adAsyncFetchNonBlocking**|0x40|指示在检索时，主线程永远不会阻塞。 如果尚未检索请求的行，则当前行将自动移动到文件末尾。<br /><br /> 如果从包含持久存储的 **记录集** 的 [流](../../../ado/reference/ado-api/stream-object-ado.md)打开 [记录集](../../../ado/reference/ado-api/recordset-object-ado.md)，则 **adAsyncFetchNonBlocking** 将不起作用;操作将是同步和阻止操作。<br /><br /> 当使用 [adCmdTableDirect](../../../ado/reference/ado-api/commandtypeenum.md)选项打开 **记录集** 时， **adAsynchFetchNonBlocking** 不起作用。|  
|**adExecuteNoRecords**|0x80|指示命令文本是命令或存储过程，不返回行 (例如，仅将数据插入) 的命令。 如果检索到任何行，则它们将被放弃且不会返回。<br /><br /> 只能将 **adExecuteNoRecords** 作为可选参数传递到 **命令** 或 **连接执行** 方法。|  
|**adExecuteStream**|0x400|指示应以流的形式返回命令执行的结果。<br /><br /> 只能将 **adExecuteStream** 作为可选参数传递到 **命令执行** 方法。|  
|**adExecuteRecord**||指示 **CommandText** 是一个命令或存储过程，它返回应作为 **记录** 对象返回的单个行。|  
|**adOptionUnspecified**|-1|指示未指定命令。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
|返回的常量|  
|--------------|  
|AdoEnums.ExecuteOption. ASYNCEXECUTE|  
|AdoEnums.ExecuteOption. ASYNCFETCH|  
|AdoEnums.ExecuteOption. ASYNCFETCHNONBLOCKING|  
|AdoEnums.ExecuteOption. NORECORDS|  
|AdoEnums.ExecuteOption。未指定|  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [Execute 方法（ADO 命令）](../../../ado/reference/ado-api/execute-method-ado-command.md)  
        [Execute 方法（ADO 连接）](../../../ado/reference/ado-api/execute-method-ado-connection.md)  
    :::column-end:::
    :::column:::
        [Open 方法（ADO 记录集）](../../../ado/reference/ado-api/open-method-ado-recordset.md)  
        [Requery 方法](../../../ado/reference/ado-api/requery-method.md)  
    :::column-end:::
:::row-end:::
