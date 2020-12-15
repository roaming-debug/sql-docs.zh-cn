---
description: bcp_sendrow
title: bcp_sendrow |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_sendrow
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_sendrow function
ms.assetid: ddbdb4bd-ad4e-4bf1-9a75-656aa26ce10a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c470244b1a739b989b5bcff36e8d0804b464bb4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483288"
---
# <a name="bcp_sendrow"></a>bcp_sendrow
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将一行数据从程序变量发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_sendrow (  
    HDBC hdbc);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 **Bcp_sendrow** 函数从程序变量生成行并将其发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 在调用 **bcp_sendrow** 之前，必须调用 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) 以指定包含行数据的程序变量。  
  
 如果 **bcp_bind** 指定长度可变的长整型数据类型（例如，SQLTEXT 的 *eDataType* 参数和非 NULL *pData* 参数），则 **bcp_sendrow** 发送整个数据值，就像对任何其他数据类型一样。 但是，如果 **bcp_bind** 具有 NULL *pData* 参数，则 **bcp_sendrow** 会在将具有指定数据的所有列发送到之后立即将控制权返回给应用程序 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 然后，应用程序可以反复调用 [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md) 将长的可变长度数据发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，一次发送一个块区。 有关详细信息，请参阅 [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)。  
  
 当使用 **bcp_sendrow** 将行从程序变量大容量复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中时，仅当用户调用 [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) 或 [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md)时才提交行。 用户可以选择每 *n* 行调用一次 **bcp_batch** ，或在传入数据的时间段之间进行趋缓。 如果永远不会调用 **bcp_batch** ，则在调用 **bcp_done** 时将提交行。  
  
 有关从开始大容量复制的重大更改的信息 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，请参阅 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
