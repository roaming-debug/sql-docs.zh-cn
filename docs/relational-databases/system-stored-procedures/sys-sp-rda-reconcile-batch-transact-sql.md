---
title: sys.sp_rda_reconcile_batch (Transact-sql) |Microsoft Docs
description: 了解如何使用 sys.sp_rda_reconcile_batch 将启用 Stretch 的 SQL Server 表中的批 ID 与存储在远程 Azure 表中的批 ID 进行协调。
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_batch
- sys.sp_rda_reconcile_batch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_batch stored procedure
ms.assetid: 6d21eac3-7b6c-4fe0-8bc4-bf503f3948a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 858da83b7529198da29cc5665219e9b83dfe3a41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211787"
---
# <a name="syssp_rda_reconcile_batch-transact-sql"></a>sys.sp_rda_reconcile_batch (Transact-sql) 
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  将已启用 Stretch 的 SQL Server 表中存储的批 ID 与存储在远程 Azure 表中的批 ID 进行协调。  
  
 通常，仅当你已从远程表中手动删除最近迁移的数据时，才必须运行 **sp_rda_reconcile_batch** 。 手动删除包含最新批的远程数据时，批处理 Id 不同步，迁移将停止。  
 
 若要删除已迁移到 Azure 的数据，请参阅本页上的 "备注"。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>语法  
  
```  
  
sp_rda_reconcile_batch @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>参数  
 \@objname = '*\@ objname*'  
 已启用延伸的 SQL Server 表的名称。  
  
## <a name="permissions"></a>权限  
 需要 db_owner 权限。  
  
## <a name="remarks"></a>备注  
 如果要删除已迁移到 Azure 的数据，请执行以下操作。  
  
1.  暂停数据迁移。 有关详细信息，请参阅[暂停和恢复数据迁移 &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)。  
  
2.  通过使用 STAGE_ONLY 提示运行 DELETE 命令，从 SQL Server 临时表中删除数据。 有关详细信息，请参阅 [进行管理更新和删除](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md#adminHints)。
  
3.  通过使用 REMOTE_ONLY 提示运行 DELETE 命令，从远程 Azure 表中删除相同的数据。  
  
4.  运行 **sp_rda_reconcile_batch**。  
  
5.  恢复数据迁移。 有关详细信息，请参阅[暂停和恢复数据迁移 &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)。  
  
## <a name="example"></a>示例  
 若要协调批处理 Id，请运行以下语句。  
  
```sql  
EXEC sp_rda_reconcile_batch @objname = N'StretchEnabledTableName';  
```  
  
  
