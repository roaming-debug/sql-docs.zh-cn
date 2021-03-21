---
title: 日期和时间改进 (OLE DB) | Microsoft Docs
description: 这些文章介绍了 OLE DB Driver for SQL Server 如何支持新的日期和时间数据类型。 请参阅概述和示例。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB]
- OLE DB, date/time improvements
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 490b8875547560a2fa5d2e8792fc80a1c9dabbff
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742337"
---
# <a name="date-and-time-improvements-ole-db"></a>日期和时间改进 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 引入了新的日期和时间数据类型。 本节介绍这些新类型如何在 OLE DB Driver for SQL Server 中公开为扩展。 有关 OLE DB Driver for SQL Server 对新日期和时间数据类型支持的概述，请参阅[日期和时间改进](../../oledb/features/date-and-time-improvements.md)。 例如，请参阅[使用增强的日期和时间功能 (OLE DB)](../../oledb/ole-db-how-to/use-enhanced-date-and-time-features-ole-db.md)。  
  
 有关日期和时间数据类型的更多常规信息，请参阅 [datetime (Transact-SQL)](../../../t-sql/data-types/datetime-transact-sql.md)。  
  
## <a name="in-this-section"></a>本节内容  
 [针对 OLE DB 日期和时间改进的数据类型支持](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)  
 提供有关支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 日期和时间数据类型的 OLE DB ( OLE DB Driver for SQL Server) 类型的信息。  
  
 [元数据 &#40;OLE DB&#41;](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md)  
 包含有关 DBBINDING 结构、ICommandWithParameters::GetParameterInfo、ICommandWithParameters::SetParameterInfo、IColumnsRowset::GetColumnsRowset 和 IColumnsInfo::GetColumnInfo 的信息。 还提供了有关 OLE DB 架构行集更新的信息。  
  
 [绑定和转换 (OLE DB)](../../oledb/ole-db-date-time/conversions-ole-db.md)  
 说明在服务器和客户端之间现有和新数据类型的转换规则。  
  
 [增强的日期和时间类型的大容量复制更改 (OLE DB)](../../oledb/ole-db-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db.md)  
 描述支持大容量复制操作的日期/时间增强功能。  
  
 [OLE DB API 对日期和时间增强功能的支持](../../oledb/ole-db-date-time/ole-db-api-support-for-date-and-time-enhancements.md)  
 说明支持日期/时间增强功能的 OLE DB API。  
  
 [IRowsetFind 的可比性](../../oledb/ole-db-date-time/comparability-for-irowsetfind.md)  
 说明日期/时间类型和 IRowsetFind。  
 
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序编程](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
