---
description: MSSQLSERVER_15517
title: MSSQLSERVER_15517 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15517 (Database Engine error)
ms.assetid: f94287f5-129f-4c52-9d34-62b996088001
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 592439f0e90f1b8ba607f518ad5125510b8e8a3e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196894"
---
# <a name="mssqlserver_15517"></a>MSSQLSERVER_15517
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|15517|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SEC_CANNOTEXECUTEASUSER|  
|消息正文|无法作为数据库主体执行，因为主体“*principal*”不存在、无法模拟这种类型的主体，或你没有所需的权限。|  
  
## <a name="user-action"></a>用户操作  
使用现有主体的名称或者获取针对该主体的 IMPERSONATE 权限。  
  
在并非原始数据库所有者的某个其他人执行数据库的附加和还原后，也可能会发生错误 15517。 若要纠正此错误，请通过运行以下命令，将 db_owner 更改为您的服务器上的某个登录名：  
  
```  
ALTER AUTHORIZATION ON DATABASE:: DBName TO [NewLogin]  
```  
  
## <a name="see-also"></a>另请参阅  
[内存中 OLTP（内存中优化）](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
