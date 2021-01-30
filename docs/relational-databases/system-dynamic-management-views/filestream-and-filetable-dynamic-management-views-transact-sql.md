---
description: Filestream 和 FileTable 动态管理视图 (Transact-SQL)
title: Filestream 和 FileTable 动态管理视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- FileTables [SQL Server], dynamic management views
ms.assetid: e50a135d-6644-42a4-a0df-1c7a2b722051
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3291b20888ba330496c6cf3d5784520ce02bc906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99146103"
---
# <a name="filestream-and-filetable-dynamic-management-views-transact-sql"></a>Filestream 和 FileTable 动态管理视图 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节介绍与 FILESTREAM 和 FileTable 功能相关的动态管理视图。  
  
## <a name="filestream-dynamic-management-views-and-functions"></a>Filestream 动态管理视图和函数  
 [sys.dm_filestream_file_io_handles (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-handles-transact-sql.md)  
 显示当前打开的事务性文件句柄。  
  
 [sys.dm_filestream_file_io_requests (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-file-io-requests-transact-sql.md)  
 显示当前文件输入和文件输出请求。  
  
## <a name="filetable-dynamic-management-views-and-functions"></a>FileTable 动态管理视图和函数  
 [sys.dm_filestream_non_transacted_handles (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)  
 显示 FileTable 数据的当前打开的非事务性文件句柄。  

## <a name="see-also"></a>另请参阅
[文件流](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Filestream 和 FileTable 目录视图 (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
<br>[Filestream 和 FileTable 系统存储过程 (Transact-SQL)](../system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)
  
