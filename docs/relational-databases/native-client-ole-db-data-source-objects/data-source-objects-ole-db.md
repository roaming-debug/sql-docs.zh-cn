---
description: 'SQL Server Native Client 数据源对象 (OLE DB) '
title: 数据源对象 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data access [SQL Server Native Client], data source objects
- SQL Server Native Client, data source objects
- SQLNCLI, data source objects
- SQL Server Native Client OLE DB provider, data source objects
- OLE DB data source objects [SQL Server Native Client]
- data source objects [OLE DB]
- CLSID
ms.assetid: c1d4ed20-ad3b-4e33-a26b-38d7517237b7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c989baa31b16f4f47e55b8cbf7c416fa88028b43
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750087"
---
#  <a name="sql-server-native-client-data-source-objects-ole-db"></a>SQL Server Native Client 数据源对象 (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 对用于建立到数据存储的链接的一组 OLE DB 接口使用术语 "数据源"，如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 创建提供程序的数据源对象的实例是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端使用者的第一个任务。  
  
 每个 OLE DB 访问接口都为自身声明一个类标识符 (CLSID)。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序的 CLSID 是 C/c + + GUID CLSID_SQLNCLI10 (符号 SQLNCLI_CLSID 会解析为引用) 的 sqlncli.msi 文件中的正确 progid。 通过 CLSID，使用者使用 OLE CoCreateInstance 函数生成数据源对象的实例  。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 是进程内服务器。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用 CLSCTX_INPROC_SERVER 宏创建 Native Client OLE DB 提供程序对象的实例以指示可执行上下文。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序数据源对象公开 OLE DB 初始化接口，该接口允许使用者连接到现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序建立的每个连接都会自动设置以下选项：  
  
-   SET ANSI_WARNINGS ON  
  
-   SET ANSI_NULLS ON  
  
-   SET ANSI_PADDING ON  
  
-   SET ANSI_NULL_DFLT_ON ON  
  
-   SET QUOTED_IDENTIFIER ON  
  
-   SET CONCAT_OF_NULL_YIELDS_NULL ON  
  
 此示例使用类标识符宏来创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序数据源对象并获取对其 **IDBInitialize** 接口的引用。  
  
```  
IDBInitialize*   pIDBInitialize;  
HRESULT          hr;  
  
hr = CoCreateInstance(CLSID_SQLNCLI10, NULL, CLSCTX_INPROC_SERVER,  
    IID_IDBInitialize, (void**) &pIDBInitialize);  
  
if (SUCCEEDED(hr))  
{  
    //  Perform necessary processing with the interface.  
    pIDBInitialize->Uninitialize();  
    pIDBInitialize->Release();  
}  
else  
{  
    // Display error from CoCreateInstance.  
}  
```  
  
 成功创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端的实例 OLE DB 提供程序数据源对象后，使用者应用程序可以通过初始化数据源并创建会话来继续。 OLE DB 会话提供允许数据访问和操作的接口。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 成功完成数据源初始化的过程中，使其第一次连接到指定的实例。 只要保持对数据源初始化接口的引用，或者在调用 IDBInitialize::Uninitialize 方法前，这一连接会一直保持  。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [数据源属性 (OLE DB)](../../relational-databases/native-client-ole-db-data-source-objects/data-source-properties-ole-db.md)  
  
-   [数据源信息属性](../../relational-databases/native-client-ole-db-data-source-objects/data-source-information-properties.md)  
  
-   [初始化和授权属性](../../relational-databases/native-client-ole-db-data-source-objects/initialization-and-authorization-properties.md)  
  
-   [会话](../../relational-databases/native-client-ole-db-data-source-objects/sessions.md)  
  
-   [会话属性 - SQL Server Native Client OLE DB 提供程序](../../relational-databases/native-client-ole-db-data-source-objects/session-properties-sql-server-native-client-ole-db-provider.md)  
  
-   [持久化数据源对象](../../relational-databases/native-client-ole-db-data-source-objects/persisted-data-source-objects.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  
  
