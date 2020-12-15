---
description: sys.time_zone_info (Transact-SQL)
title: sys.time_zone_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.time_zone_info
- sys.time_zone_info_TSQL
- time_zone_info
- time_zone_info_TSQL
helpviewer_keywords:
- sys.time_zone_info system table
ms.assetid: 3f51a9a4-75f8-4a11-9552-8bf6118b68da
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d88603b0f7da691633f14211a14204bf14b32aac
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466948"
---
# <a name="systime_zone_info-transact-sql"></a>sys.time_zone_info (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  返回有关支持的时区的信息。 计算机上安装的所有时区都存储在以下注册表配置单元中：  
`KEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones`.  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|采用 Windows 标准格式的时区的名称。 例如， **中部澳大利亚标准时间** 或 **中欧标准时间**。|  
|**current_utc_offset**|**nvarchar (12)**|当前与 UTC 的偏移量。 例如， **+ 01:00** 或 **-07:00**。|  
|**is_currently_dst**|**bit**|如果当前观察到夏令时，则为 True。|  
  
## <a name="see-also"></a>另请参阅  
 [GETUTCDATE &#40;Transact-sql&#41;](../../t-sql/functions/getutcdate-transact-sql.md)   
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [日期和时间数据类型及函数 (Transact-SQL)](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md)   
 [服务器范围内的配置目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)  
