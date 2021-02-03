---
description: MSSQLSERVER_8621
title: MSSQLSERVER_8621 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8621 (Database Engine error)
ms.assetid: 67f59865-becd-4999-8bb0-90aedd7effbf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 61b164c84b39dc4f1e6e2a9a243d86243e798041
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180912"
---
# <a name="mssqlserver_8621"></a>MSSQLSERVER_8621
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|8621|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|OPTIMIZER_STACK_OVERFLOW_ERR|  
|消息正文|查询处理器在优化查询时堆栈空间不足。 请简化查询。|  
  
## <a name="explanation"></a>说明  
出错的最可能原因是扩展查询的大小。 在每个视图的定义、计算列、[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数和它引用的公用表表达式以及级联操作（如更新辅助索引、视图和触发器）方面，扩展查询将替换原始查询。  
  
很可能查询在某个维度中很大；例如，视图定义所引用的表数，或者很大的标量表达式。  
  
## <a name="user-action"></a>用户操作  
通过沿最大维度将该查询分解为多个查询，对其进行简化。 首先删除实际上不需要的任何查询元素，然后尝试添加临时表并将该查询拆分为两个查询。  仅将查询的一部分移动到子查询、函数或公用表表达式是不够的，因为 [!INCLUDE[tsql](../../includes/tsql-md.md)] 编译器会将它们重新组合在一起。  
  
