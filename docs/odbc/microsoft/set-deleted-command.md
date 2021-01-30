---
description: SET DELETED 命令
title: SET DELETED 命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET DELETED command [ODBC]
ms.assetid: 6b5e0086-156d-471d-8e7f-6c5fa9686cd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6447e2d0c77e1a161df58ca070fae8c624c196a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208641"
---
# <a name="set-deleted-command"></a>SET DELETED 命令
指定是否处理标记为删除的记录以及是否可在其他命令中使用这些记录。  
  
## <a name="syntax"></a>语法  
  
```  
  
SET DELETED ON | OFF  
```  
  
## <a name="arguments"></a>参数  
 ON  
  (驱动程序的默认值;Visual FoxPro 的默认值为 OFF。 ) 指定对记录执行的命令 (包含相关表中的记录，) 使用范围将忽略标记为删除的记录。  
  
 OFF  
 指定可以通过对记录执行的命令（ (包括相关表中的记录) 使用作用域）来访问标记为删除的记录。  
  
## <a name="remarks"></a>备注  
 使用已删除 ( ) 测试记录状态的查询可以使用 Visual FoxPro 雕像技术进行优化，前提是该表是根据删除的 ( ) 编制索引的。  
  
> [!IMPORTANT]  
>  如果命令的默认作用域是当前记录或包括单个记录的作用域，则将忽略 SET DELETED。 INDEX 始终忽略 SET DELETED，并为表中的所有记录编制索引。  
  
## <a name="see-also"></a>另请参阅  
 [DELETE - SQL 命令](../../odbc/microsoft/delete-sql-command.md)
