---
description: SET NULL 命令
title: 设置 NULL 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set nullSET NULL
ms.assetid: 410c5a6e-e957-4ecc-9e2d-e591cbc0bc4f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a190c913b2f8cc9d7a111a0c95d0408de2c1c6c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208603"
---
# <a name="set-null-command"></a>SET NULL 命令
确定 ALTER TABLE-SQL、CREATE TABLE SQL 和 INSERT SQL 命令如何支持 null 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
SET NULL ON | OFF  
```  
  
## <a name="arguments"></a>参数  
 ON  
  (驱动程序的默认值;Visual FoxPro 的默认值为 OFF。 ) 指定使用 ALTER TABLE 和 CREATE TABLE 创建的表中的所有列都将允许空值。 您可以通过在列的定义中包括 NOT NULL 子句来覆盖表中列的 null 值支持。  
  
 还指定 INSERT-SQL 会将 null 值插入到 INSERT SQL 值子句中未包含的任何列中。 INSERT-SQL 只会将 null 值插入允许空值的列。  
  
 OFF  
 指定使用 ALTER TABLE 和 CREATE TABLE 创建的表中的所有列都不允许空值。 可以通过在列定义中包含 NULL 子句来指定 ALTER TABLE 和 CREATE TABLE 中列的 null 值支持。  
  
 还指定 INSERT-SQL 会将空值插入到 INSERT SQL 值子句中未包含的任何列中。  
  
## <a name="remarks"></a>备注  
 SET NULL 仅影响 ALTER TABLE、CREATE TABLE 和 INSERT SQL 所支持的 NULL 值。 如果设置为 NULL，其他命令不受影响。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TABLE-SQL 命令](../../odbc/microsoft/alter-table-sql-command.md)   
 [CREATE TABLE-SQL 命令](../../odbc/microsoft/create-table-sql-command.md)   
 [INSERT - SQL 命令](../../odbc/microsoft/insert-sql-command.md)
