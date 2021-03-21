---
description: bcp_batch
title: bcp_batch |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_batch
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_batch function
ms.assetid: 0bda489e-86bc-4a7e-80f6-96047e03f281
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c780882b4c1cca9c709ea8ca5f8da4aad4063dcb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751907"
---
# <a name="bcp_batch"></a>bcp_batch
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将以前从程序变量大容量复制的所有行提交 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DBINT bcp_batch (HDBC  
        hdbc);  
```  
  
## <a name="arguments"></a>参数  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
## <a name="returns"></a>返回  
 在最后一次调用 **bcp_batch** 之后保存的行数; 如果出现错误，则为-1。  
  
## <a name="remarks"></a>备注  
 大容量复制批处理定义事务。 当应用程序使用 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) 和 **bcp_sendrow** 将行从程序变量大容量复制到 SQL Server 表时，只在程序调用 **bcp_batch** 或 [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md)时才提交行。  
  
 可以在每 *n* 行调用一次 bcp_batch，也可以在传入数据 (时调用 ，就像在遥测应用程序) 中一样。 如果应用程序不调用 **bcp_batch** 仅当调用 **bcp_done** 时才提交大容量复制的行。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
