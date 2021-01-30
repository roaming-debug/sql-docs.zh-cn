---
description: sys.sp_cdc_enable_db (Transact-SQL)
title: sys.sp_cdc_enable_db (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_enable_db_TSQL
- sp_cdc_enable_db
- sys.sp_cdc_enable_db
- sys.sp_cdc_enable_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_enable_db
- change data capture [SQL Server], enabling databases
- sp_cdc_enable_db
ms.assetid: 176d83b3-493d-43cd-800e-aa123c3bdf17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a0756c34e9fc914abdb99cddef865d0968767ab3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206001"
---
# <a name="syssp_cdc_enable_db-transact-sql"></a>sys.sp_cdc_enable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  对当前数据库启用变更数据捕获。 必须先对数据库执行此过程，然后才能对该数据库中的任何表启用变更数据捕获。 变更数据捕获可记录应用到所启用的表中的插入、更新和删除活动，同时采用易于使用的关系格式提供变更详细信息。 此操作将为已修改的行捕获反映了所跟踪源表列结构的列信息，同时还捕获将更改应用到目标环境所需的元数据。  
  
> [!IMPORTANT]
>  并非在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的每个版本中均提供变更数据捕获功能。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]各版本支持的功能列表，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_cdc_enable_db  
```  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 无法在 [系统数据库](../../relational-databases/databases/system-databases.md) 或分发数据库上启用变更数据捕获。  
  
 sys.sp_cdc_enable_db 将创建以全数据库为作用域的变更数据捕获对象，包括元数据表和 DDL 触发器。 它还创建 cdc schema 和 cdc 数据库用户，并将 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目录视图中的数据库条目的 is_cdc_enabled 列设置为1。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例启用了变更数据捕获。  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_db;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.sp_cdc_disable_db &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)  
  
  
