---
description: SQLGetConnectOption 函数
title: SQLGetConnectOption 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectOption
helpviewer_keywords:
- SQLGetConnectOption function [ODBC]
ms.assetid: 59cde899-7957-4b5e-8677-f34d3b859bfd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f518915bb9813fd6d0c17e1b37f436ae4379e12d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203411"
---
# <a name="sqlgetconnectoption-function"></a>SQLGetConnectOption 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性：已弃用  
  
 **摘要**  
 在 ODBC 3.x *中，odbc* *2.x 函数* **SQLGetConnectOption** 已被 **SQLGetConnectAttr** 取代。 有关详细信息，请参阅 [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)。  
  
> [!NOTE]
>  有关 ODBC *2.x 应用程序**使用 odbc 2.x* 驱动程序时，驱动程序管理器将此函数映射到的内容的详细信息，请参阅附录 G：驱动程序准则中的 [映射弃用的函数](../../../odbc/reference/appendixes/mapping-deprecated-functions.md)以实现向后兼容性。  
> 
> [!NOTE]
>  **SQLGetConnectOption** 不支持 ODBC 3.8 中引入的属性 SQL_ASYNC_DBC_FUNCTION_ENABLE。 使用连接句柄上的异步操作的应用程序必须使用 **SQLGetConnectAttr**。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
