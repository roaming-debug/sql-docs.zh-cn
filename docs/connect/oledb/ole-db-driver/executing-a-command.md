---
title: 执行命令（OLE DB 驱动程序）| Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中的使用者如何通过首先创建会话、获取行集以及使用 Execute 来执行命令。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- commands [OLE DB Driver for SQL Server]
- OLE DB, executing commands
- sessions [OLE DB Driver for SQL Server]
- OLE DB extensions for XML
- OLE DB Driver for SQL Server, command execution
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 91c5377e84a47255bd079332c58835ec7413ce6d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749717"
---
# <a name="executing-a-command"></a>执行命令
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  与数据源建立连接后，使用者调用 IDBCreateSession::CreateSession  方法来创建会话。 该会话充当命令、行集或事务工厂。  
  
 为了直接使用单独的表或索引，使用者请求 IOpenRowset 接口  。 IOpenRowset::OpenRowset 方法打开并返回一个行集，该行集包括来自单个基表或索引的所有行  。  
  
 为了执行某一命令（例如 SELECT \* FROM Authors），使用者请求 IDBCreateCommand 接口  。 使用者可以执行 IDBCreateCommand::CreateCommand  方法，以便为 ICommandText  接口创建命令对象和请求。 ICommandText::SetCommandText  方法用于指定要执行的命令。  
  
 Execute 命令用于执行该命令  。 该命令可以是任何 SQL 语句或过程名称。 不是所有命令都将生成结果集（行集）对象。 SELECT * FROM Authors 之类的命令将生成结果集。  
  
## <a name="see-also"></a>另请参阅  
 [创建适用于 SQL Server 的 OLE DB 驱动程序应用程序](../../oledb/ole-db-driver/creating-a-oledb-driver-for-sql-server-application.md)  
  
  
