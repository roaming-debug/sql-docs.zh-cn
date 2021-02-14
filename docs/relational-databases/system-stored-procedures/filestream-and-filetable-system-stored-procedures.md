---
description: Filestream 和 FileTable 系统存储过程 (Transact-SQL)
title: Filestream 和 FileTable 系统存储过程 (Transact-sql) |Microsoft Docs
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
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ee0eced0370c3ea16091827555e90f4951d6cf7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346702"
---
# <a name="filestream-and-filetable-system-stored-procedures-transact-sql"></a>Filestream 和 FileTable 系统存储过程 (Transact-sql) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本部分介绍 FileTable 和 Filestream 功能的系统存储过程。  

## <a name="filestream-and-filetable-system-stored-procedures"></a>Filestream 和 Filetable 系统存储过程
  [sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)

   强制运行 FILESTREAM 垃圾回收器，从而删除任何不需要的 FILESTREAM 文件。

  [sp_kill_filestream_non_transacted_handles (Transact-SQL)](filestream-and-filetable-sp-kill-filestream-non-transacted-handles.md)

  关闭 FileTable 数据的非事务性文件句柄。


## <a name="see-also"></a>请参阅
[文件流](../../relational-databases/blob/filestream-sql-server.md)
<br>[Filetable](../../relational-databases/blob/filetables-sql-server.md)
<br>[Filestream 和 FileTable 动态管理视图 (Transact-SQL)](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
<br>[Filestream 和 FileTable 目录视图 (Transact-SQL)](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
  
  
