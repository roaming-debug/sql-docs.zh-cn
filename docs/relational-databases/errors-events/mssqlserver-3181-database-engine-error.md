---
description: MSSQLSERVER_3181
title: MSSQLSERVER_3181 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3181 (Database Engine error)
ms.assetid: e6d2e716-5263-477c-ad0e-7bcbfef4c1f3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9d64c35a396e6a011ec96463d14cff1eb74e8184
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194161"
---
# <a name="mssqlserver_3181"></a>MSSQLSERVER_3181
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|事件 ID|3181|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|LDDB_STORAGE_VERIFY|  
|消息正文|还原此备份的尝试可能会遇到存储空间问题。 后续消息将提供详细信息。|  
  
## <a name="explanation"></a>说明  
RESTORE VERIFYONLY 语句会检查数据库将还原到的磁盘上的可用存储空间。  
  
### <a name="possible-causes"></a>可能的原因  
可用磁盘空间不足，因此无法还原要验证的备份。  
  
## <a name="user-action"></a>用户操作  
将备份还原到拥有足够磁盘空间的位置，或提供更多磁盘空间。  
  
## <a name="see-also"></a>另请参阅  
[将数据库还原到新位置 (SQL Server)](~/relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
[将文件还原到新位置 (SQL Server)](~/relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
[RESTORE VERIFYONLY (Transact-SQL)](~/t-sql/statements/restore-statements-verifyonly-transact-sql.md)  
  
