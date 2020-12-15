---
description: bcp_getcolfmt
title: bcp_getcolfmt |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_getcolfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_getcolfmt function
ms.assetid: f8bdada5-7b2d-4475-8c98-f93e9d77b130
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d22956e5331d8ab718d354b981d59d7cfef4eaeb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473778"
---
# <a name="bcp_getcolfmt"></a>bcp_getcolfmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  用于查找列格式属性值。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_getcolfmt (  
        HDBC hdbc,  
        INT field,  
        INT property,  
        void* pValue,  
        INT cbvalue,  
        INT* pcbLen);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *定义域*  
 要检索其属性的列编号。  
  
 *property*  
 属性常量之一。  
  
 *pValue*  
 指向要从中检索属性值的缓冲区的指针。  
  
 *cbValue*  
 以字节表示的属性缓冲区的长度。  
  
 *pcbLen*  
 指向属性缓冲区中所返回数据的长度的指针。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 [Bcp_setcolfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-setcolfmt.md)主题中列出了列格式属性值。 列格式属性值是通过调用 **bcp_setcolfmt** 函数设置的，而 **bcp_getcolfmt** 函数用于查找列格式属性值。  
  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]与早期版本相比，连接到 (或更高版本) 服务器计算机时，可能会观察到行为更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
## <a name="bcp_getcolfmt-support-for-enhanced-date-and-time-features"></a>bcp_getcolfmt 对日期和时间增强功能的支持  
 与日期/时间类型的 **BCP_FMT_TYPE** 属性一起使用的类型是在 [&#40;OLE DB 和 ODBC&#41;的增强日期和时间类型的大容量复制更改](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)中指定的。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
