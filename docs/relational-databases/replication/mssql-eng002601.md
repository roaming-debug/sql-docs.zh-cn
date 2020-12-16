---
description: MSSQL_ENG002601
title: MSSQL_ENG002601 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG002601 error
ms.assetid: 657c3ae6-9e4b-4c60-becc-4caf7435c1dc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 50892f2cdefcbab8fe29aea732cc839c128dc11a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475848"
---
# <a name="mssql_eng002601"></a>MSSQL_ENG002601
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|2601|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称|空值|  
|消息正文|不能在具有唯一索引 '%.\*ls' 的对象 '%.*ls' 中插入重复键的行。|  
  
## <a name="explanation"></a>说明  
 这是可能引发的一般错误，不管是否复制数据库，都会引发该错误。 在已复制的数据库中，通常会引发该错误，因为尚未跨拓扑对主键进行适当的管理。 在分布式环境中，确保没有将同一值插入到多个节点上的主键列或任何其他唯一列，这一点很重要。 可能的原因包括：  
  
-   在多个节点上对行进行了插入和更新。 合并复制和事务性复制的可更新订阅都提供了冲突检测和解决，但是最好还是只在一个节点上插入或更新给定行。 对等事务性复制没有提供冲突检测和解决，它要求对插入和更新进行分区。  
  
-   在应该为只读的订阅服务器上插入了行。 应该将快照发布的订阅服务器视为只读，就像事务发布的订阅服务器一样，除非使用了可更新的订阅或对等事务性复制。  
  
-   使用了具有标识列的表，但是没有对该列进行适当的管理。  
  
-   在合并复制过程中，插入系统表 **MSmerge_contents** 时也可发生该错误；所引起的错误类似于：不能在具有唯一索引“ucl1SycContents”的对象“MSmerge_contents”中插入重复键的行。  
  
## <a name="user-action"></a>用户操作  
 所需操作取决于引发错误的原因：  
  
-   在多个节点上对行进行了插入和更新。  
  
     不管使用哪种复制类型，我们都建议您尽可能地对插入和更新进行分区，因为这样可以减少检测和解决冲突所需的处理操作。 对于对等事务性复制，需要对插入和更新进行分区。 有关详细信息，请参阅 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)。  
  
-   在应该为只读的订阅服务器上插入了行。  
  
     除非使用合并复制、具有可更新订阅的事务性复制或对等事务性复制，否则不要在订阅服务器上插入或更新行。  
  
-   使用了具有标识列的表，但是没有对该列进行适当的管理。  
  
     对于合并复制和具有可更新订阅的事务性复制，标识列应该由复制自动管理。 对于对等事务性复制，必须手动进行管理。 有关详细信息，请参阅[复制标识列](../../relational-databases/replication/publish/replicate-identity-columns.md)。  
  
-   此错误在系统表 **MSmerge_contents** 中进行插入时发生。  
  
     发生此错误的原因是联接筛选器属性 **join_unique_key** 的值不正确。 仅当父表中的联接列唯一时，才应将此属性设置为 TRUE。 如果此属性设置为 TRUE，而该列不唯一，则引发此错误。 有关设置此属性的详细信息，请参阅 [定义和修改合并项目间的联接筛选器](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
