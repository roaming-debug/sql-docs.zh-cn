---
description: MSSQLSERVER_41302
title: MSSQLSERVER_41302 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41302 (Database Engine error)
ms.assetid: 01e75618-afec-4232-ba68-93ab7bc31003
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1d01df7afff73d181cd389c417aa2283b8048f35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185153"
---
# <a name="mssqlserver_41302"></a>MSSQLSERVER_41302
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|41302|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|WRITE_WRITE_CONFLICT|  
|消息正文|当前事务尝试更新自该事务启动后已更新的记录。 该事务已中止。|  
  
## <a name="explanation"></a>说明  
事务遇到写/写冲突，语句已终止。  
  
## <a name="user-action"></a>用户操作  
稍后在其他事务中重试该操作。 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
## <a name="see-also"></a>另请参阅  
[内存中 OLTP（内存中优化）](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
