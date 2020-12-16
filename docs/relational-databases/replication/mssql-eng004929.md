---
description: MSSQL_ENG004929
title: MSSQL_ENG004929 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG004929 error
ms.assetid: 1d9b1d88-1fbf-4089-b392-687d3b0220ca
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 246c01776e8af4ad31666653387eebf20e2bb8b9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475788"
---
# <a name="mssql_eng004929"></a>MSSQL_ENG004929
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|4929|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|无法更改 %S_MSG '%.*ls'，因为正在为复制而发布它。|  
  
## <a name="explanation"></a>说明  
 尝试删除为事务性复制而发布的表的主键约束时，通常会发生此错误。 事务性复制要求每个已发布表都具有主键，因此不能删除约束。  
  
## <a name="user-action"></a>用户操作  
 若要删除约束，请先删除与表关联的项目。 有关详细信息，请参阅[向现有发布添加项目和从中删除项目](../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)。 如果此错误发生在未复制的数据库中，请执行 [sp_removedbreplication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)，以确保此数据库中的对象不会标记为已复制。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
