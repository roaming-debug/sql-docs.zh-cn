---
description: '用 IOpenRowset (Native Client OLE DB 提供程序创建行集) '
title: 用 IOpenRowset (Native Client OLE DB provider) 创建行集 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- IOpenRowset interface
- rowsets [OLE DB], creating
- SQL Server Native Client OLE DB provider, rowsets
- OLE DB rowsets, creating
ms.assetid: e8bc3de7-4b97-4de9-8df8-e11947d24045
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d39ea5abe537938e20f2870fb293f592cadeb5ab
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753607"
---
# <a name="creating-a-rowset-with-iopenrowset-in-sql-server-native-client"></a>使用 IOpenRowset 在 SQL Server Native Client 中创建行集
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序支持 **IOpenRowset：： OpenRowset** 方法，但有以下限制：  
  
-   必须在 pTableID 参数指向的数据库 ID (DBID) 结构中指定基表或视图  。  
  
-   DBID eKind 成员必须指示 DBKIND_NAME  。  
  
-   DBID uName 成员必须将现有基表或视图的名称指定为 Unicode 字符串  。  
  
-   OpenRowset 的 pIndexID 参数必须为 NULL   。  
  
 IOpenRowset::OpenRowset 的结果集包含单个行集  。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标可以支持包含单个行集的结果集。 游标支持允许开发人员使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并发控制机制。  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
  
