---
description: SQLSetStmtOption 函数
title: SQLSetStmtOption 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetStmtOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetStmtOption
helpviewer_keywords:
- SQLSetStmtOption function [ODBC]
ms.assetid: 9cbe2b62-4cf7-43ab-8fb4-9a53df2c6b3f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bef7c6e2036e7ec0dc9512a152cfc1c5108786e5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191775"
---
# <a name="sqlsetstmtoption-function"></a>SQLSetStmtOption 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性：已弃用  
  
 **摘要**  
 在 ODBC 3.x *中，odbc* 2.0 函数 **SQLSetStmtOption** 已被 **SQLSetStmtAttr** 取代。 有关详细信息，请参阅 [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)。  
  
> [!NOTE]
>  有关 ODBC *2.x 应用程序* 使用 odbc 2.x 驱动程序时，驱动程序管理器将此函数映射到的内容的详细信息，请参阅附录 G：*驱动程序准则* 中的 [映射弃用的函数](../../../odbc/reference/appendixes/mapping-deprecated-functions.md) 以实现向后兼容性。  
  
## <a name="remarks"></a>备注  
 如果你的应用程序将在64位操作系统上运行，请参阅 [ODBC 64 位信息](../../../odbc/reference/odbc-64-bit-information.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
