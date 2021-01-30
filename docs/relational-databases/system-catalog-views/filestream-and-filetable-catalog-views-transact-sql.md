---
description: Filestream 和 FileTable 目录视图 (Transact-SQL)
title: Filestream 和 FileTable 目录视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], catalog views
ms.assetid: 2c83a4a7-720b-4435-a3b5-788c29f56949
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8713dba06964ce44ccaa81b5f03ec44e1b30beaa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194626"
---
# <a name="filestream-and-filetable-catalog-views-transact-sql"></a>Filestream 和 FileTable 目录视图 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节介绍与 FileTable 功能相关的目录视图。  
  
## <a name="filestream-and-filetable-catalog-views-transact-sql"></a>Filestream 和 filetable 目录视图 (Transact-sql) 
 [sys.database_filestream_options (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-filestream-options-transact-sql.md)  
 显示已启用的针对 FileTable 中的 FILESTREAM 数据的非事务性访问级别的相关信息。 为 SQL Server 实例中的每个数据库包含一行。  
  
 [sys.filetable_system_defined_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filetable-system-defined-objects-transact-sql.md)  
 显示与 FileTable 相关的系统定义对象的列表。 为每个系统定义对象包含一行。  
  
 [sys.filetables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-filetables-transact-sql.md)  
 为每个 FileTable 返回一行。 继承自 **sys.databases**。  

## <a name="see-also"></a>另请参阅
[文件流](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Filestream 和 FileTable 动态管理视图 (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Filestream 和 FileTable 系统存储过程 (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
  
