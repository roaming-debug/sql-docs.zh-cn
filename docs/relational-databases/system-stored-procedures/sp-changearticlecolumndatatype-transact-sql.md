---
description: sp_changearticlecolumndatatype (Transact-SQL)
title: sp_changearticlecolumndatatype (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticlecolumndatatype
- sp_changearticlecolumndatatype_TSQL
helpviewer_keywords:
- sp_changearticlecolumndatatype
ms.assetid: 0db80e08-fb77-4d0c-aa41-455b13ffa9b4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 34f3f1878fcae59601a6ac94ff8536f3d22d6ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203704"
---
# <a name="sp_changearticlecolumndatatype-transact-sql"></a>sp_changearticlecolumndatatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改 Oracle 发布的项目列数据类型映射。 此存储过程在分发服务器上的任何数据库中执行。  
  
> [!NOTE]  
>  受支持的发布服务器类型之间的数据类型映射是默认提供的。 仅当覆盖这些默认设置时，才使用 **sp_changearticlecolumndatatype** 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changearticlecolumndatatype [ @publication= ] 'publication'  
    [ @article = ] 'article'   
    [ @column = ] 'column'  
    [ , [ @type = ] 'type' ]  
    [ , [ @length = ] length ]  
    [ , [ @precision = ] precision ]  
    [ , [ @scale = ] scale ]  
    [ , [ @publisher = ] 'publisher'  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` Oracle 发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @article = ] 'article'` 项目的名称。 *项目* 是 **sysname**，无默认值。  
  
`[ @column = ] 'column'` 要更改其数据类型映射的列的名称。 *列* 的值为 **sysname**，无默认值。  
  
`[ @type = ] 'type'`[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 目标列中的数据类型的名称。 *type 的类型* 为 **sysname**，默认值为 NULL。  
  
`[ @length = ] length`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标列中数据类型的长度。 *长度* 为 **bigint**，默认值为 NULL。  
  
`[ @precision = ] precision`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标列中数据类型的精度。 *精度* 为 **bigint**，默认值为 NULL。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **Sp_changearticlecolumndatatype** 用于重写受支持的发布服务器类型 (Oracle 和) 之间的默认数据类型映射 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 若要查看这些默认的数据类型映射，请执行 [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)。  
  
 仅 Oracle 发布服务器支持 **sp_changearticlecolumndatatype** 。 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布执行此存储过程将导致错误。  
  
 必须为必须更改的每个项目列映射执行 **sp_changearticlecolumndatatype** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_changearticlecolumndatatype**。  
  
## <a name="see-also"></a>另请参阅  
 [更改发布和项目属性](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
