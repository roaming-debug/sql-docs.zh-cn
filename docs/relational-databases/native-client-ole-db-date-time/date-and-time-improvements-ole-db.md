---
description: 'SQL Server Native Client 日期和时间改进 (OLE DB) '
title: 日期和时间改进 (OLE DB)
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB]
- OLE DB, date/time improvements
ms.assetid: 71614aaf-0fa4-4fe0-b522-68e2e0b66f43
author: markingmyname
ms.author: maghan
ms.custom: seo-dt-2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d6afbedc35ca1da8f3d9325fc4721d92062f4d2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97434915"
---
# <a name="sql-server-native-client-date-and-time-improvements-ole-db"></a>SQL Server Native Client 日期和时间改进 (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入了新的日期和时间数据类型。 本部分介绍如何将这些新类型作为本机客户端中的扩展公开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 新日期和时间数据类型的 Native Client 支持的概述，请参阅 [日期和时间改进](../../relational-databases/native-client/features/date-and-time-improvements.md)。 例如，请参阅[使用增强的日期和时间功能 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/use-enhanced-date-and-time-features-ole-db.md)。  
  
 有关日期和时间数据类型的更多常规信息，请参阅 [datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)。  
  
## <a name="in-this-section"></a>本节内容  
 [针对 OLE DB 日期和时间改进的数据类型支持](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)  
 提供有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期和时间数据类型 OLE DB ( Native Client) 类型的信息。  
  
 [元数据 &#40;OLE DB&#41;](./data-type-support-for-ole-db-date-and-time-improvements.md)  
 包含有关 DBBINDING 结构、ICommandWithParameters::GetParameterInfo、ICommandWithParameters::SetParameterInfo、IColumnsRowset::GetColumnsRowset 和 IColumnsInfo::GetColumnInfo 的信息。 还提供了有关 OLE DB 架构行集更新的信息。  
  
 [绑定和转换 (OLE DB)](../../relational-databases/native-client-ole-db-date-time/conversions-ole-db.md)  
 说明在服务器和客户端之间现有和新数据类型的转换规则。  
  
 [&#40;OLE DB 和 ODBC 的增强日期和时间类型的大容量复制&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)  
 描述支持大容量复制操作的日期/时间增强功能。  
  
 [OLE DB API 对日期和时间增强功能的支持](../../relational-databases/native-client-ole-db-date-time/ole-db-api-support-for-date-and-time-enhancements.md)  
 说明支持日期/时间增强功能的 OLE DB API。  
  
 [IRowsetFind 的可比性](../../relational-databases/native-client-ole-db-date-time/comparability-for-irowsetfind.md)  
 说明日期/时间类型和 IRowsetFind。  
  
 [旧 SQL Server 版本 &#40;OLE DB 的新日期和时间功能&#41;](../../relational-databases/native-client-ole-db-date-time/new-date-and-time-features-with-previous-sql-server-versions-ole-db.md)  
 说明使用日期和时间增强功能的客户端应用程序与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本通信时的预期行为，以及使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 早期版本编译的客户端向支持日期和时间增强功能的服务器发送命令的预期行为。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
