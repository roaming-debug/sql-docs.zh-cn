---
description: sys.fulltext_document_types (Transact-SQL)
title: sys.fulltext_document_types (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fulltext_document_types_TSQL
- fulltext_document_types
- fulltext_document_types_TSQL
- sys.fulltext_document_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_document_types catalog view
ms.assetid: 156fcfa4-7304-4a5c-b96f-1c3e061e5df0
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 09e43a52ad5eae807c20692dd71421d93eb40697
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466998"
---
# <a name="sysfulltext_document_types-transact-sql"></a>sys.fulltext_document_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为可用于全文索引操作的每个文档类型返回一行。 每行表示在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中注册的 IFilter 接口。  
  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**document_type**|**sysname**|支持的文档类型的文件扩展名。<br /><br /> 此值可用于标识在类型为 **varbinary (max)** 或 **image** 的列进行全文索引时将使用的筛选器。|  
|**class_id**|**uniqueidentifier**|支持文件扩展名的 IFilter 类的 GUID。|  
|**路径**|**nvarchar(260)**|IFilter DLL 的路径。 路径仅对 **serveradmin** 固定服务器角色的成员可见。|  
|**version**|**sysname**|IFilter DLL 的版本。|  
|**提供**|**sysname**|IFilter 制造商的名称。<br /><br /> 注意：仅支持附带制造商的文档 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDS](../../includes/sssds-md.md)] 。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
