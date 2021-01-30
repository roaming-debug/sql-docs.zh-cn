---
description: 'sys.sys配置 (Transact-sql) '
title: sys.sys配置 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sysconfigures
- sysconfigures
- sys.sysconfigures_TSQL
- sysconfigures_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconfigures compatibility view
- sysconfigures system table
ms.assetid: 146bf10a-c898-4676-a2a1-673fb1cee7a2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 109c17040f52392df2518ddccb47491d2b0a9527
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180699"
---
# <a name="syssysconfigures-transact-sql"></a>sys.sys配置 (Transact-sql) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对每个由用户设置的配置选项都包含一行。 **sysconfigures** 包含在最新启动之前定义的配置选项 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，以及自那时起设置的任何动态配置选项。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**value**|**int**|用户可修改的变量值。 仅在执行 RECONFIGURE 后，由[!INCLUDE[ssDE](../../includes/ssde-md.md)]使用。|  
|**config**|**int**|配置变量号。|  
|**comment**|**nvarchar(255)**|对配置选项的解释。|  
|**status**|**smallint**|指示选项状态的位图。 可能的值如下所示：<br /><br /> 0 = 静态。 重新启动服务器后，设置才会生效。<br /><br /> 1 = 动态。 执行 RECONFIGURE 语句后，变量才会生效。<br /><br /> 2 = 高级。 仅当设置了 " **显示高级选项** " 时，才会显示变量。 重新启动服务器后，设置才会生效。<br /><br /> 3 = 动态和高级。|  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
