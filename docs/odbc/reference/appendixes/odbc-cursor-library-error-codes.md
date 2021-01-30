---
description: ODBC 游标库错误代码
title: ODBC 游标库错误代码 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- cursor library [ODBC], error codes
- error codes [ODBC], cursor library
- ODBC cursor library [ODBC], error codes
ms.assetid: 9713480e-8744-4f37-a630-20871590d4a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6c9a4d7a68ddaf9f569a04acd8be81b993a636b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193258"
---
# <a name="odbc-cursor-library-error-codes"></a>ODBC 游标库错误代码
> [!IMPORTANT]  
>  此功能将在 Microsoft 数据访问组件的未来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 请改用驱动程序和服务器游标。  
  
 除 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)中列出的那些 SQLSTATEs 外，odbc 游标库还会返回以下。  
  
> [!NOTE]  
>  游标库不对状态记录进行排序;驱动程序管理器和 ODBC 3。*x* 驱动程序负责对状态记录进行排序。  
  
|SQLSTATE|说明|可以从|  
|--------------|-----------------|--------------------------|  
|01000|游标不可更新。|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|01000|未使用游标库。 加载失败。|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|未使用游标库。 驱动程序支持不足。|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|未使用游标库。 版本与驱动程序管理器不匹配。|**SQLBrowseConnect**<br /><br /> **SQLConnect**<br /><br /> **SQLDriverConnect**|  
|01000|SQL_SUCCESS_WITH_INFO 返回驱动程序。 警告消息已丢失。|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|S1000|常见错误：无法创建文件缓冲区。|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|常规错误：无法从文件缓冲区读取。|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|常规错误：无法写入文件缓冲区。|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|S1000|常规错误：无法关闭或删除文件缓冲区。|**SQLFreeHandle**<br /><br /> **SQLFreeStmt**|  
|SL001|无法执行定位请求，因为未绑定任何可搜索的列。|**SQLExecDirect**<br /><br /> **SQLGetData**<br /><br /> **SQLPrepare**|  
|SL002|无法执行定位请求，因为结果集是通过联接条件创建的。|**SQLExecute**<br /><br /> **SQLExecDirect**<br /><br /> **SQLGetData**|  
|SL003|绑定缓冲区超出最大段大小。|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL004|**SELECT** 语句不生成结果集。|**SQLGetData**|  
|SL005|**SELECT** 语句包含 group by 子句。|**SQLGetData**|  
|SL006|定位请求不支持参数数组。|**SQLPrepare**<br /><br /> **SQLExecDirect**|  
|SL008|不允许在只进 (非缓冲) 游标上使用 **SQLGetData** 。|**SQLGetData**|  
|SL009|调用 **SQLFetch** 或 **SQLFetchScroll** 之前未绑定任何列。|**SQLFetch**<br /><br /> **SQLFetchScroll**|  
|SL010|尝试绑定到内部缓冲区时， **SQLBindCol** 返回 SQL_ERROR。|**SQLFetch**<br /><br /> **SQLFetchScroll**<br /><br /> **SQLGetData**|  
|SL011|语句选项仅在调用 **SQLFetch** 或 **SQLFetchScroll** 后有效。|**SQLGetStmtAttr**|  
|SL012|当游标打开时，语句绑定可能不会更改。|**SQLBindCol**<br /><br /> **SQLFreeHandle**<br /><br /> **SQLFreeStmt**<br /><br /> **SQLSetStmtAttr**|  
|SL014|发出了定位请求，但并未缓冲所有列计数字段。|**SQLExecDirect**<br /><br /> **SQLExecute**<br /><br /> **SQLPrepare**|  
|SL015|**SQLFetch** 和 **SQLFetchScroll** 不能混合。|**SQLExtendedFetch**<br /><br /> **SQLFetch**<br /><br /> **SQLFetchScroll**|
