---
title: " (ODBC) 中添加数据源 |Microsoft Docs"
description: 了解 SQL Server ODBC 驱动程序如何使用远程存储过程调用机制，将存储过程作为 SQL Server 中的远程存储过程调用。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data sources [ODBC]
ms.assetid: b4ac6f0e-8e6a-4b1a-9a7e-60e0a69b2180
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: af6b417f8742fa20f49954f8b5ae2bcb5ddfc6f3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752547"
---
# <a name="configuring-the-sql-server-odbc-driver---add-a-data-source"></a>配置 SQL Server ODBC 驱动程序 - 添加数据源
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将 ODBC 应用程序用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本之前，必须了解如何升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本上的目录存储过程的版本，以及如何添加、删除和测试数据源。  
  
  您可以使用 ODBC 管理器添加数据源，通过使用 [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)) 或创建文件以编程方式 (。  
  
### <a name="to-add-a-data-source-by-using-odbc-administrator"></a>使用 ODBC 管理器添加数据源  
  
1.  在 " **控制面板**" 中，访问 " **管理工具** "，然后 **(64 位)** 或 **odbc 数据源 (32 位)** 中的 odbc 数据源。 或者，可以调用 odbcad32.exe。  
  
2.  单击 " **用户 dsn**"、" **系统 dsn**" 或 " **文件 dsn** " 选项卡，然后单击 " **添加**"。  
  
3.  单击 " **SQL Server**"，然后单击 " **完成**"。  
  
4.  完成 " **创建新数据源到 SQL Server** " 向导中的步骤。  
  
### <a name="to-add-a-data-source-programmatically"></a>以编程方式添加数据源  
  
1.  调用 [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md) ，并将第二个参数设置为 ODBC_ADD_DSN 或 ODBC_ADD_SYS_DSN。  
  
### <a name="to-add-a-file-data-source"></a>添加文件数据源  
  
1.  使用连接字符串中的 SAVEFILE = file_name 参数调用 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) 。 如果连接成功，则 ODBC 驱动程序将使用 SAVEFILE 参数指向的位置中的连接参数创建一个文件数据源。  
  
## <a name="see-also"></a>另请参阅  
[&#40;ODBC&#41;删除数据源 ](../../relational-databases/native-client-odbc-how-to/configuring-the-sql-server-odbc-driver-delete-a-data-source.md)    
  
  
