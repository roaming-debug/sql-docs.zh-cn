---
description: sys.bandwidth_usage (Azure SQL Database)
title: Azure SQL Database (sys.bandwidth_usage) |Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- bandwidth_usage
- sys.bandwidth_usage
- bandwidth_usage_TSQL
- sys.bandwidth_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.bandwidth_usage
- bandwidth_usage
ms.assetid: 43ed8435-f059-4907-b5c0-193a258b394a
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 9af11212533337e575976261995161b222e73efa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198564"
---
# <a name="sysbandwidth_usage-azure-sql-database"></a>sys.bandwidth_usage (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

> [!NOTE]
> 这仅适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V11。 * *  
  
 返回有关 **[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V11 数据库服务器** 中每个数据库使用的网络带宽的信息。 为给定数据库返回的每行总结在一小时内使用的单个方向和类别。  
  
 **此已在中弃用 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。**  
  
 **Sys.bandwidth_usage** 视图包含以下列。  
  
|列名|说明|  
|-----------------|-----------------|  
|**time**|占用带宽的时间（小时）。 此视图中的各行以小时为单位。 例如，2009-09-19 02:00:00.000 表示占用带宽的时间是 2009 年 9 月 19 日的凌晨 2:00  到凌晨 3:00。|  
|**database_name**|占用带宽的数据库的名称。|  
|**direction**|占用带宽的类型，为以下选项之一：<br /><br /> 入口：移动到中的数据 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。<br /><br /> 出口：移出的数据 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。|  
|class |占用带宽的类别，为以下选项之一：<br />内部：正在 Azure 平台中移动的数据。<br />External：移出 Azure 平台的数据。<br /><br /> 仅在数据库与区域处于连续复制关系 ([!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]) 时返回此类别。 如果给定数据库未参与任何连续复制关系，则不会返回 "互连" 行。 有关详细信息，请参阅本主题后面的“备注”部分。|  
|**time_period**|使用带宽的时段为 Peak 或 OffPeak。 The Peak time is based on the region in which the server was created. 例如，如果在“US_Northwest”地区创建了服务器，则高峰期时间定义为 PST 时间上午 10:00 点 到下午 6:00 之间。|  
|**quantity**|占用的带宽量 (KB)。|  
  
## <a name="permissions"></a>权限

 此视图仅在 **master** 数据库中适用于服务器级主体登录名。  
  
## <a name="remarks"></a>备注  
  
### <a name="external-and-internal-classes"></a>External 和 Internal 类别

 对于在给定时间使用的每个数据库，" **sys.bandwidth_usage** " 视图返回显示类和带宽使用方向的行。 下例列举给定数据库可能公开的数据。 在此示例中，时间为 2012-04-21 17:00: 00，即发生在高峰时段。 数据库名称为 Db1。 在此示例中， **sys.bandwidth_usage** 已为入口和出口方向以及外部和内部类的四个组合返回了一行，如下所示：  
  
|time|database_name|direction|class|time_period|quantity|  
|----------|--------------------|---------------|-----------|------------------|--------------|  
|2012-04-21 17:00:00|Db1|流入量|外部|Peak|66|  
|2012-04-21 17:00:00|Db1|流出量|外部|Peak|741|  
|2012-04-21 17:00:00|Db1|流入量|内部|Peak|1052|  
|2012-04-21 17:00:00|Db1|流出量|内部|Peak|3525|  
  
### <a name="interpreting-data-direction-for-ssgeodr"></a>解释 [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] 的数据方向

 对于 [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]，带宽使用情况数据在连续复制关系两侧的逻辑主数据库中可见。 因此，您必须从要查询的数据库的角度解释入口和出口方向指示器。 例如，请考虑将1MB 数据从源服务器传输到目标服务器的复制流。 在这种情况下，在源服务器上，1MB 会计入发送的总数据，在目标服务器上，1MB 记录为接收的数据。  
  
> [!NOTE]  
> 这些数据按用户数据流的方向从源服务器传输到目标服务器。 但是，在另一方向也需要传输一些数据。  
