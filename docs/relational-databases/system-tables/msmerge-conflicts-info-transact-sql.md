---
description: MSmerge_conflicts_info (Transact-SQL)
title: MSmerge_conflicts_info (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
author: cawrites
ms.author: chadam
ms.openlocfilehash: da7de7b1c8c18a74fc5a255027507853d195813b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091462"
---
# <a name="msmerge_conflicts_info-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  **MSmerge_conflicts_info** 表跟踪将订阅与合并发布进行同步时发生的冲突。 冲突的丢失行数据存储在发生冲突的项目的 [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) 表中。 此表存储在发布服务器上的发布数据库中，并存储在订阅服务器上的订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|已发布表的别名。|  
|**rowguid**|**uniqueidentifier**|冲突行的标识符。|  
|**origin_datasource**|**nvarchar(255)**|发起冲突更改的数据库名。|  
|**conflict_type**|**int**|发生的冲突类型，可以为下列类型之一：<br /><br /> **1** = 更新冲突：在行级别上检测到冲突。<br /><br /> **2** = 列更新冲突：在列级别上检测到冲突。<br /><br /> **3** = 更新删除 wins 冲突：删除入选冲突。<br /><br /> **4** = 更新 Wins 删除冲突：此表中记录了丢失冲突的已删除 rowguid。<br /><br /> **5** = 上载插入失败：无法在发布服务器中应用来自订阅服务器的插入。<br /><br /> **6** = 下载插入失败：无法在订阅服务器上应用从发布服务器进行的插入。<br /><br /> **7** = 上载删除失败：无法将订阅服务器上的删除内容上载到发布服务器。<br /><br /> **8** = 下载删除失败：无法将在发布服务器上删除操作下载到订阅服务器。<br /><br /> **9** = 上载更新失败：订阅服务器上的更新无法在发布服务器上应用。<br /><br /> **10** = 下载更新失败：发布服务器上的更新无法应用于订阅服务器。<br /><br /> **11** = 分辨率<br /><br /> **12** = 逻辑记录更新 Wins 删除：此表中记录了丢失冲突的已删除逻辑记录。<br /><br /> **13** = 逻辑记录冲突插入更新：插入到逻辑记录与更新冲突。<br /><br /> **14** = 逻辑记录删除 Wins 更新冲突：此表中记录了丢失冲突的更新逻辑记录。|  
|**reason_code**|**int**|可能与上下文相关的错误代码。 对于更新-更新和更新-删除冲突，用于此列的值与 **conflict_type** 相同。 但是，对于失败的更改冲突，原因代码是使合并代理无法应用更改的错误。 例如，如果合并代理在订阅服务器上由于主键冲突而无法在订阅服务器上应用插入，则它将记录一个 **conflict_type** 为 6 ( "下载插入失败" ) ，并记录 **reason_code** 为2627，这是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] primary key 冲突的内部错误消息： "违反了% ls 约束 '%. * ls"。 无法在对象 '%. 中插入重复键。 \*ls "."|  
|**reason_text**|**nvarchar (720)**|可能与上下文相关的错误说明。|  
|**pubid**|**uniqueidentifier**|发布的标识符。|  
|**MSrepl_create_time**|**datetime**|冲突发生的时间。|  
|**origin_datasource_id**|**uniqueidentifier**|发起冲突更改的数据库的标识符。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
