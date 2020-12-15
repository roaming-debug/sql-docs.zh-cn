---
description: sys.identity_columns (Transact-SQL)
title: sys.identity_columns (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- identity_columns
- sys.identity_columns
- sys.identity_columns_TSQL
- identity_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.identity_columns catalog view
ms.assetid: 97ee01e6-9c9e-4fd9-884b-68b4084669d5
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 195768c830e13f2cb61f04bff9fe67f6eefe6dc4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484759"
---
# <a name="sysidentity_columns-transact-sql"></a>sys.identity_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  作为标识列的每个列各占一行。  
  
 **Sys.identity_columns** 视图继承 **sys.databases** 视图中的行。 **Sys.identity_columns** 视图返回 **sys.databases** 视图中的列，以及 **seed_value**、 **increment_value**、 **last_value** 和 **is_not_for_replication** 列。 有关详细信息，请参阅[目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**\<columns inherited from sys.columns>**||**Sys.identity_columns** 视图返回 **sys.databases** 视图中的所有列。 它还返回如下所述的其他列。 有关 **sys.identity_columns** 视图继承自 **sys.databases** 的列的说明，请参阅 [&#40;transact-sql&#41;的 sys.databases。](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)|  
|**seed_value**|**sql_variant**|该标识列的种子值。 种子值的数据类型与列本身的数据类型相同。|  
|**increment_value**|**sql_variant**|该标识列的增量值。 种子值的数据类型与列本身的数据类型相同。|  
|**last_value**|**sql_variant**|为该标识列生成的最后一个值。 种子值的数据类型与列本身的数据类型相同。|  
|**is_not_for_replication**|**bit**|标识列声明为 NOT FOR REPLICATION。 **注意：** 此列不适用于 Azure Synapse 分析。|  
  
> [!NOTE]  
>  要创建一个可在多个表中使用的自动递增数字或者可以从应用程序中调用而不引用任何表的自动递增数字，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)  
  
  
