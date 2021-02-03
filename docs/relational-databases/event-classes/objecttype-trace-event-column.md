---
title: ObjectType 跟踪事件列 | Microsoft Docs
description: 了解对象类型跟踪事件列的可能值，它们用在 SQL Server 中的各种跟踪事件中。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Object Type column values
- events [SQL Server], Object Type column values
- event classes [SQL Server], Object Type column values
- Object Type column values [SQL Server]
ms.assetid: 42f85c50-34c9-49ca-955f-af9595e2707f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 63b888ac84ff567be7b72eb09cd6271853ab69f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183239"
---
# <a name="objecttype-trace-event-column"></a>ObjectType 跟踪事件列
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  ObjectType 跟踪事件列用在各种跟踪事件中。 本主题说明此列的可能值以及其相关定义。  
  
## <a name="object-type-column-values"></a>ObjectType 列值  
  
|值|定义|  
|-----------|----------------|  
|8259|检查约束|  
|8260|默认值（约束或独立）|  
|8262|外键约束|  
|8272|存储过程|  
|8274|规则|  
|8275|系统表|  
|8276|服务器上的触发器|  
|8277|（用户定义的）表|  
|8278|查看|  
|8280|扩展存储过程|  
|16724|CLR 触发器|  
|16964|数据库|  
|16975|对象|  
|17222|全文目录|  
|17232|CLR 存储过程|  
|17235|架构|  
|17475|凭据|  
|17491|DDL 事件|  
|17741|管理事件|  
|17747|安全性事件|  
|17749|用户事件|  
|17985|CLR 聚合函数|  
|17993|内联表值 SQL 函数|  
|18000|分区函数|  
|18002|复制筛选过程|  
|18004|表值 SQL 函数|  
|18259|服务器角色|  
|18263|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 组|  
|19265|非对称密钥|  
|19277|主密钥|  
|19280|主密钥|  
|19283|ObfusKey|  
|19521|非对称密钥登录名|  
|19523|证书登录|  
|19538|角色|  
|19539|SQL 登录名|  
|19543|Windows 登录名|  
|20034|远程服务绑定|  
|20036|数据库上的事件通知|  
|20037|事件通知|  
|20038|标量 SQL 函数|  
|20047|对象上的事件通知|  
|20051|同义词|  
|20307|序列|  
|20549|端点|  
|20801|可能缓存的即席查询|  
|20816|可能缓存的已准备查询|  
|20819|Service Broker 服务队列|  
|20821|唯一约束|  
|21057|应用程序角色|  
|21059|证书|  
|21075|服务器|  
|21076|Transact-SQL 触发器|  
|21313|Assembly|  
|21318|CLR 标量函数|  
|21321|内联标量 SQL 函数|  
|21328|分区方案|  
|21333|用户|  
|21571|Service Broker 服务约定|  
|21572|数据库上的触发器|  
|21574|CLR 表值函数|  
|21577|内部表（例如，XML 节点表、队列表。）|  
|21581|Service Broker 消息类型|  
|21586|Service Broker 路由|  
|21587|统计信息|  
|21825<br /><br /> 21827<br /><br /> 21831<br /><br /> 21843<br /><br /> 21847|用户|  
|22099|Service Broker 服务|  
|22601|索引|  
|22604|证书登录|  
|22611|XMLSchema|  
|22868|类型|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
