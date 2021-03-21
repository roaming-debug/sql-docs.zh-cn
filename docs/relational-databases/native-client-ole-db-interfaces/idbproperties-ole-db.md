---
description: 'IDBProperties (Native Client OLE DB 提供程序) '
title: IDBProperties (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 2e5a4fd8-5164-495a-9986-3477aef8d8a5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 59e98bfcdec717aaed8312d6c5c066873e219d3e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756067"
---
# <a name="idbproperties-native-client-ole-db-provider"></a>IDBProperties (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  OLE DB 标准规范允许提供程序为 DBPROPINFO::vValues 指定 VT_EMPTY  。 但是，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 调用 **IDBProperties：： GetPropertyInfo** 和 **DBPROPSET_ROWSETALL** 检索行集属性时，Native Client OLE DB 始终返回 VT_EMPTY。  
  
## <a name="see-also"></a>另请参阅  
 [接口 (OLE DB)](./sql-server-native-client-ole-db-interfaces.md)  
  
