---
description: MSSQLSERVER_41301
title: MSSQLSERVER_41301 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41301 (Database Engine error)
ms.assetid: c6127e1e-2846-4ee9-bc42-2d896ea9730e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5d7fef050bc8287fb2665343e0fda148d38bf523
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185119"
---
# <a name="mssqlserver_41301"></a>MSSQLSERVER_41301
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|41301|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|COMMIT_DEPENDENCY_FAILURE|  
|消息正文|当前事务所依赖的前一事务已终止，当前事务无法再提交。|  
  
## <a name="explanation"></a>说明  
事务遇到依赖关系错误，已失败。  
  
依赖事务太多也可导致此错误。 任何写事务的依赖事务都有数量限制。 例如，如果太多的读事务尝试依赖更新事务，就可能会发生此错误。  
  
## <a name="user-action"></a>用户操作  
不必对事务执行任何操作。 调用 ROLLBACK TRAN 回滚事务。 有关详细信息，请参阅[内存中 OLTP&#40;内存中优化&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)。  
  
## <a name="see-also"></a>另请参阅  
[启用和禁用 AlwaysOn 可用性组 (SQL Server)](~/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
