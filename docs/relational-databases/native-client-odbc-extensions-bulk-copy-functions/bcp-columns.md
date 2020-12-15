---
description: bcp_columns
title: bcp_columns |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_columns
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_columns function
ms.assetid: 5376f6fe-9508-439a-8c66-778d77f19ac3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 867ab836c1ab31d1deac348bc63b74e76d1970fe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473608"
---
# <a name="bcp_columns"></a>bcp_columns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  用于设置在以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 作为源或目标执行大容量复制时所用的用户文件中找到的列的总数。 可以使用[bcp_setbulkmode](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setbulkmode.md) ，而不是 bcp_columns 和[bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_columns (  
        HDBC hdbc,  
        INT nColumns);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *nColumns*  
 用户文件中的列的总数。 即使您准备将数据从用户文件大容量复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，并且不想复制用户文件中的所有列，仍必须将 *nColumns* 设置为用户文件列总数。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 仅当使用有效的文件名调用 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) 之后，才能调用此函数。  
  
 仅当您要使用不同于默认设置的用户文件格式时，才应当调用该函数。 有关默认用户文件格式的说明的详细信息，请参阅 **bcp_init**。  
  
 调用 **bcp_columns** 之后，必须对用户文件中的每个列调用 [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) 以完全定义自定义文件格式。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
