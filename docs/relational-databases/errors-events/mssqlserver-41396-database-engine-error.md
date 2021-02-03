---
description: MSSQLSERVER_41396
title: MSSQLSERVER_41396 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41396 (Database Engine error)
ms.assetid: 4ff04042-8367-46f3-8a16-c94682d6eedb
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4eb000a3e01f6bffe4fc29455f839c6085efa932
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179491"
---
# <a name="mssqlserver_41396"></a>MSSQLSERVER_41396
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|41396|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|MAX_SORT_ROWS_EXCEEDED|  
|消息正文|该排序操作超出了缓冲区限制。 存储过程执行已中止。 有关详细信息，请查阅 SQL Server 联机丛书。|  
  
## <a name="explanation"></a>说明  
本机编译的存储过程在内存中执行排序操作。 对排序缓冲区的大小存在限制。 此错误意味着该排序缓冲区的大小超过了此限制。 排序操作和存储过程执行已中止。  
  
排序缓冲区中每一行或条目的大小由已排序的行数以及查询中联接的数目和聚合函数的数目和类型确定。 通过简化查询，可以减小每一行的大小，从而在排序缓冲区中容纳更多的行。 基表中行的大小不会影响排序缓冲区中每一行或条目的大小。  
  
## <a name="user-action"></a>用户操作  
选择更少的行，或者通过删除联接或聚合函数降低查询的复杂程度。  
  
## <a name="see-also"></a>另请参阅  
[内存中 OLTP（内存中优化）](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
