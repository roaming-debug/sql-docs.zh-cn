---
description: sp_table_validation (Transact-SQL)
title: sp_table_validation (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_table_validation_TSQL
- sp_table_validation
helpviewer_keywords:
- sp_table_validation
ms.assetid: 31b25f9b-9b62-496e-a97e-441d5fd6e767
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 255c30ea6b3144e781007bff3ae748c8e5a960ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189253"
---
# <a name="sp_table_validation-transact-sql"></a>sp_table_validation (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  返回有关表或索引视图的行数或校验和信息，或者将提供的行数或校验和信息与指定的表或索引视图进行比较。 此存储过程在发布服务器上的发布数据库中执行，或者在订阅服务器上的订阅数据库中执行。 *对于 Oracle 发布服务器不支持*。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_table_validation [ @table = ] 'table'  
    [ , [ @expected_rowcount = ] type_of_check_requested OUTPUT]  
    [ , [ @expected_checksum = ] expected_checksum OUTPUT]  
    [ , [ @rowcount_only = ] rowcount_only ]  
    [ , [ @owner = ] 'owner' ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @table_name = ] table_name ]  
    [ , [ @column_list = ] 'column_list' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @table = ] 'table'` 表的名称。 *table* 的值为 **sysname**，无默认值。  
  
`[ @expected_rowcount = ] expected_rowcountOUTPUT` 指定是否返回表中所需的行数。 *expected_rowcount* 的值为 **int**，默认值为 NULL。 如果为 NULL，则将实际行数作为输出参数返回。 如果提供了一个值，则针对实际行数检查该值，以标识二者之间的差异。  
  
`[ @expected_checksum = ] expected_checksumOUTPUT` 指定是否返回表的预期校验和。 *expected_checksum* 为 **numeric**，默认值为 NULL。 如果为 NULL，则将实际校验和作为输出参数返回。 如果提供了一个值，则针对实际校验和检查该值，以标识二者之间的差异。  
  
`[ @rowcount_only = ] type_of_check_requested` 指定要执行的校验和或行数的类型。 *type_of_check_requested* 为 **smallint**，默认值为 **1**。  
  
 如果为 **0**，则执行行计数和与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 兼容的校验和。  
  
 如果为 **1**，则仅执行行计数检查。  
  
 如果为 **2**，则执行行计数和二进制校验和。  
  
`[ @owner = ] 'owner'` 表所有者的名称。 *所有者* 为 **sysname**，默认值为 NULL。  
  
`[ @full_or_fast = ] full_or_fast` 用于计算行计数的方法。 *full_or_fast* 为 **tinyint**，默认值为 **2**，可以为以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**0**|使用 COUNT(*) 进行完整计数。|  
|**1**|从 **sysindexes** 中快速计数。 在 **sysindexes** 中对行进行计数比在实际表中计算行的速度要快得多。 但是，因为 **sysindexes** 被延迟更新，所以行计数可能不准确。|  
|**2** （默认值）|首先尝试使用快速方法进行条件性快速计数。 如果快速方法显示出差异，则转而使用完整方法。 如果 *expected_rowcount* 为 NULL，并且存储过程用于获取该值，则始终使用 ( * ) 的完整计数。|  
  
`[ @shutdown_agent = ] shutdown_agent` 如果分发代理 **sp_table_validation** 执行，则指定分发代理是否应在完成验证后立即关闭。 *shutdown_agent* 为 **bit**，默认值为 **0**。 如果为 **0**，则复制代理不会关闭。 如果为 **1**，则会引发错误20578，并向复制代理发出信号以关闭。 当用户直接执行 **sp_table_validation** 时，将忽略此参数。  
  
`[ @table_name = ] table_name` 用于输出消息的视图的表名。 *table_name* 的默认值为 **sysname**，默认值为 **\@ table**。  
  
`[ @column_list = ] 'column_list'` 应该在校验和函数中使用的列的列表。 *column_list* 为 **nvarchar (4000)**，默认值为 NULL。 启用合并项目验证，以指定不包括计算列和时间戳列的列列表。  
  
## <a name="return-code-values"></a>返回代码值  
 如果执行校验和验证时所需的校验和等于表中的校验和， **sp_table_validation** 将返回一条消息，指示表已通过校验和验证。 否则，将返回一条消息，指示表可能不同步，并报告预期的行数和实际行数之间的差异。  
  
 如果执行行计数验证并且预期的行数等于表中的数字， **sp_table_validation** 将返回表通过行计数验证的消息。 否则，将返回一条消息，指示表可能不同步，并报告预期的行数和实际行数之间的差异。  
  
## <a name="remarks"></a>备注  
 **sp_table_validation** 在所有类型的复制中使用。 Oracle 发布服务器不支持 **sp_table_validation** 。  
  
 校验和对页上的整个行图像上计算 32 位循环冗余检查 (CRC)。 它不是有选择地检查列，并且不能对视图或表的垂直分区进行操作。 此外，校验和将跳过设计)  (的 **text** 和 **image** 列的内容。  
  
 执行校验和检查时，两个服务器的表结构必须完全相同；也就是说，表中包含的列必须相同，且列的顺序、数据类型和长度以及 NULL/NOT NULL 条件都必须相同。 例如，如果发布服务器执行 CREATE TABLE，然后执行 ALTER TABLE 以添加列，但是订阅服务器上应用的脚本是一个简单的 CREATE 表，则表结构不相同。 如果您不确定两个表的结构是相同的，请查看 [syscolumns](../../relational-databases/system-compatibility-views/sys-syscolumns-transact-sql.md) 并确认每个表中的偏移是相同的。  
  
 如果使用了字符模式的 **bcp** ，则浮点值可能会生成校验和差异，如果发布具有非订阅服务器，则为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 这是由于在进行字符模式转换时，精度上存在不可避免的微小差异。  
  
## <a name="permissions"></a>权限  
 若要执行 **sp_table_validation**，您必须对要验证的表具有 SELECT 权限。  
  
## <a name="see-also"></a>另请参阅  
 [CHECKSUM &#40;Transact-sql&#41;](../../t-sql/functions/checksum-transact-sql.md)   
 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)   
 [sp_article_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md)   
 [sp_publication_validation &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
