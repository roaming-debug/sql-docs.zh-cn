---
description: MSSQLSERVER_10507
title: MSSQLSERVER_10507 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10507 (Database Engine error)
ms.assetid: cd83fa81-ac37-4eda-a3c3-17610b051de2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 804a09bd518b5dd15d9abbd1944cf4cbb1aa00d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197486"
---
# <a name="mssqlserver_10507"></a>MSSQLSERVER_10507
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|10507|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|PG_STMT_DOES_NOT_MATCH|  
|消息正文|无法创建计划指南“%.\*ls”，因为由 **\@stmt** 和 **\@module_or_batch** 或由 **\@plan_handle** 和 **\@statement_start_offset** 指定的语句与指定模块或批中的语句均不匹配。 请修改这些值以匹配模块或批中的语句。|  
  
## <a name="explanation"></a>说明  
指定模块或批中的语句无法与指定的语句或语句偏移量值相匹配。  
  
## <a name="user-action"></a>用户操作  
请修改指定的参数值，使其与模块或批中的语句相匹配。  
  
## <a name="see-also"></a>另请参阅  
[计划指南](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[sp_create_plan_guide_from_handle (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
