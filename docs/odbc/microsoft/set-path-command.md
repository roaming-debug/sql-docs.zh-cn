---
description: SET PATH 命令
title: 设置路径命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET PATH command [ODBC]
ms.assetid: db488d1e-0963-4f45-8c76-a23b9bde9e9d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a97ac5a074116fcaad0477625ea683226abaf76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203925"
---
# <a name="set-path-command"></a>SET PATH 命令
指定文件搜索的路径。 有关特定于驱动程序的信息，请参阅 "备注"。  
  
## <a name="syntax"></a>语法  
  
```  
  
SET PATH TO [Path]  
```  
  
## <a name="arguments"></a>参数  
 TO [ *Path*]  
 指定要在 Visual FoxPro 中搜索的目录。 使用逗号或分号分隔目录。  
  
## <a name="remarks"></a>备注  
 设置路径允许您为其他可在存储过程中调用的 Visual FoxPro 程序指定搜索路径。 设置路径将不会更改为连接指定的数据源的路径。  
  
 将路径设置为不带 *路径* 的路径，以还原默认目录或文件夹的路径。  
  
## <a name="driver-remarks"></a>驱动程序备注  
 如果在存储过程中发出设置路径，则以下函数和命令将忽略该路径：  
  
-   诸如 [SQLTables](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md) 和 [SQLColumns](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md) 之类的目录函数将忽略新路径，并继续引用 [SQLPrepare](../../odbc/microsoft/sqlprepare-visual-foxpro-odbc-driver.md) 或 [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md)中的数据源指定的路径。  
  
-   诸如 SELECT、INSERT、UPDATE、DELETE 和 CREATE TABLE 等命令将忽略新路径，并继续引用 **SQLPrepare** 或 **SQLExecDirect** 中的数据源指定的路径。  
  
 如果在存储过程中发出集路径，但随后不将路径设置回其原始状态，则数据库的其他连接将使用新路径 (因为设置路径的作用域不是) 的数据会话。  
  
 如果要在数据源所指定的目录以外的目录中创建、选择或更新表，请使用命令指定文件的完整路径。  
  
## <a name="see-also"></a>另请参阅  
 ["ODBC Visual FoxPro 安装程序" 对话框](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)   
 [SQLColumns (Visual FoxPro ODBC 驱动程序) ](../../odbc/microsoft/sqlcolumns-visual-foxpro-odbc-driver.md)   
 [SQLDriverConnect (Visual FoxPro ODBC 驱动程序) ](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md)   
 [SQLTables（Visual FoxPro ODBC 驱动程序）](../../odbc/microsoft/sqltables-visual-foxpro-odbc-driver.md)
