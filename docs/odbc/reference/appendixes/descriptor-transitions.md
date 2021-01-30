---
description: 描述符转换
title: 描述符转换 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- state transitions [ODBC], descriptor
- transitioning states [ODBC], descriptor
- descriptor transitions [ODBC]
ms.assetid: 0cf24fe6-5e3c-45fa-81b8-4f52ddf8501d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d312b372a0817ae79ad07bdbf149f90ba0c71939
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194884"
---
# <a name="descriptor-transitions"></a>描述符转换
ODBC 描述符具有以下三种状态。  
  
|状态|说明|  
|-----------|-----------------|  
|D0|未分配的描述符|  
|D1i|隐式分配的描述符|  
|D1e|显式分配的描述符|  
  
 下表显示了每个 ODBC 函数如何影响描述符状态。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
|D1i [1]|--|--|  
|D1e [2]|--|--|  
  
 [1] 此行显示 SQL_HANDLE_STMT *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DESC *HandleType* 时，该行显示转换。  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
| (IH) |--|--|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
|--[1]|D0|--|  
| (IH) [2]| (HY017) |D0|  
  
 [1] 此行显示 SQL_HANDLE_STMT *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DESC *HandleType* 时，该行显示转换。  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField 和 SQLGetDescRec  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
| (IH) |--|--|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField 和 SQLSetDescRec  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
| (IH) [1]|--|--|  
  
 [1] 此行显示当 *DescriptorHandle* 是 ARD、APD 或 IPD 的句柄， (或者当 *DescriptorHandle* 是 IRD 和 *FieldIdentifier* 的句柄 SQL_DESC_ARRAY_STATUS_PTR 或 SQL_DESC_ROWS_PROCESSED_PTR 时 **SQLSetDescField**) 的转换。  
  
## <a name="all-other-odbc-functions"></a>所有其他 ODBC 函数  
  
|D0<br /><br /> 未分配|D1i<br /><br /> 隐式|D1e<br /><br /> 显式|  
|------------------------|----------------------|----------------------|  
|--|--|--|
