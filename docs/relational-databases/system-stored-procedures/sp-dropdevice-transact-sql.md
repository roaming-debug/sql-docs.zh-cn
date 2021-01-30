---
description: sp_dropdevice (Transact-SQL)
title: sp_dropdevice (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropdevice_TSQL
- sp_dropdevice
dev_langs:
- TSQL
helpviewer_keywords:
- backup devices [SQL Server], deleting
- sp_dropdevice
ms.assetid: c8b07189-7c35-414b-acc1-45bd6e7e17c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5df2e24fa3ca61cb235153e0157dbb953a828629
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200667"
---
# <a name="sp_dropdevice-transact-sql"></a>sp_dropdevice (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从的实例中删除数据库设备或备份设备，并 [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] 从 **master.dbo.sys设备** 中删除该条目。  
   
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_dropdevice [ @logicalname = ] 'device'   
    [ , [ @delfile = ] 'delfile' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @logicalname = ] 'device'`**master.dbo.sysdevices.name** 中列出的数据库设备或备份设备的逻辑名称。 *设备* 为 **sysname**，无默认值。  
  
`[ @delfile = ] 'delfile'` 指定是否应删除物理备份设备文件。 *delfile* 是 **varchar (7)**。 如果指定为 **DELFILE**，则删除物理备份设备磁盘文件。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 不能在事务内使用 **sp_dropdevice** 。  
  
## <a name="permissions"></a>权限  
 要求具有 **diskadmin** 固定服务器角色中的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例从[!INCLUDE[ssDE](../../includes/ssde-md.md)]中除去 `tapedump1` 磁带转储设备。  
  
```  
EXEC sp_dropdevice 'tapedump1';  
```  
  
## <a name="see-also"></a>请参阅  
 [备份设备 (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [删除备份设备 &#40;SQL Server&#41;](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)   
 [sp_addumpdevice (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_helpdb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpdevice &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdevice-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
