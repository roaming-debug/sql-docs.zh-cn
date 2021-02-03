---
description: MSSQLSERVER_1406
title: MSSQLSERVER_1406 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1406 (Database Engine error)
ms.assetid: 915f97de-9b74-41f8-8bd5-b2d061416718
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c5d7fe3f66ea581305f985e25cc1a2c30c1809e4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197016"
---
# <a name="mssqlserver_1406"></a>MSSQLSERVER_1406
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|1406|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|DBM_BADSTATEFORFORCESERVICE|  
|消息正文|无法安全地强制执行服务。 请删除数据库镜像并恢复数据库 "%.*ls" 以获得访问权。|  
  
## <a name="explanation"></a>说明  
由于镜像状态不能保证强制服务过程正确进行，所以 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 不能强制服务。  
  
## <a name="user-action"></a>用户操作  
删除数据库镜像。 然后可以恢复该镜像数据库以获得它的访问权。  
  
## <a name="see-also"></a>另请参阅  
[在数据库镜像会话中强制服务 (Transact-SQL)](~/database-engine/database-mirroring/force-service-in-a-database-mirroring-session-transact-sql.md)  
[删除数据库镜像 (SQL Server)](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
  
