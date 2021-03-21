---
description: SQLTablePrivileges
title: SQLTablePrivileges |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLTablePrivileges function
ms.assetid: 8cce22d5-28b1-4b50-a5bc-1de03e0ffd6b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4efebad2fd9b6e08d87b22ce9b056a7baf276c76
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754777"
---
# <a name="sqltableprivileges"></a>SQLTablePrivileges
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  可以对静态游标执行 **SQLTablePrivileges** 。 尝试在可更新 (键集驱动或动态) 上执行 **SQLTablePrivileges** 会返回 SQL_SUCCESS_WITH_INFO，指示游标类型已更改。  
  
 通过接受由两部分组成的 *CatalogName* 参数的名称： *Linked_Server_Name*，> Native Client ODBC 驱动程序支持链接服务器上的表的报告信息。  
  
## <a name="see-also"></a>另请参阅  
 [SQLTablePrivileges 函数] (https://go.microsoft.com/fwlink/?LinkId=59373\)   
 [ODBC API 实现细节](~/relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
