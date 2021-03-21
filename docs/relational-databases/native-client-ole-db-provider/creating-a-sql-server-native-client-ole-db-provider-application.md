---
description: 创建 SQL Server Native Client OLE DB 访问接口应用程序
title: 创建 OLE DB 应用
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, application creation
- applications [SQL Server Native Client]
- OLE DB, creating applications
ms.assetid: f3ae6815-f32d-4913-a1a2-2ba2f20cfd88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a15d78c5e1a3f53a812eb13d94eded070f6b3e1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753687"
---
# <a name="creating-a-sql-server-native-client-ole-db-provider-application"></a>创建 SQL Server Native Client OLE DB 访问接口应用程序
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序应用程序涉及以下步骤：  
  
1.  建立与数据源的连接。  
  
2.  执行命令。  
  
3.  结果处理。  

> [!NOTE]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 cryptoAPI](/windows/win32/seccng/cng-portal) 对它们加密。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [建立与数据源的连接](../../relational-databases/native-client-ole-db-provider/establishing-a-connection-to-a-data-source.md)  
  
-   [执行命令](../../relational-databases/native-client-ole-db-provider/executing-a-command.md)  
  
-   [处理结果](../../relational-databases/native-client-ole-db-provider/processing-results.md)  
  
-   [关于 OLE DB 属性](../../relational-databases/native-client-ole-db-provider/about-ole-db-properties.md)  
  
-   [在 SQL Server Native Client 中使用 OUTPUT 子句 (OLE DB)](../../relational-databases/native-client-ole-db-provider/using-the-output-clause-with-ole-db-in-sql-server-native-client.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
