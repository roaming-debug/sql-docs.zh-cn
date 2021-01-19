---
description: DROP SEQUENCE (Transact-SQL)
title: DROP SEQUENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SEQUENCE
- DROP_SEQUENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SEQUENCE statement
- sequence number object, dropping
ms.assetid: c25772d3-61af-4aa7-b58b-a6f67a793e3d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0513019d95994607cf3db97c0fe9f72f8c3e44e0
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171619"
---
# <a name="drop-sequence-transact-sql"></a>DROP SEQUENCE (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  从当前数据库中删除序列对象。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP SEQUENCE [ IF EXISTS ] { database_name.schema_name.sequence_name | schema_name.sequence_name | sequence_name } [ ,...n ]  
 [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 到[当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)）。  
  
 只有在序列已存在时才对其进行有条件地删除。  
  
 *database_name*  
 要在其中创建序列对象的数据库的名称。  
  
 *schema_name*  
 序列对象所属架构的名称。  
  
 sequence_name  
 要删除的序列的名称。 类型为 sysname。  
  
## <a name="remarks"></a>注解  
 在生成编号后，序列对象与其生成的编号之间没有延续关系，因此可以删除序列对象，即使生成的编号仍在使用。  
  
 当序列对象由存储过程或触发器引用时，可以删除序列对象，因为序列对象未绑定到架构上。 如果序列对象是作为表中的默认值引用的，则无法删除序列对象。 错误消息将列出引用序列的对象。  
  
 若要列出数据库中的所有序列对象，请执行以下语句。  
  
```sql  
SELECT sch.name + '.' + seq.name AS [Sequence schema and name]   
    FROM sys.sequences AS seq  
    JOIN sys.schemas AS sch  
        ON seq.schema_id = sch.schema_id ;  
GO  
```  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求具有架构的 ALTER 或 CONTROL 权限。  
  
### <a name="audit"></a>审核  
 若要审核 **DROP SEQUENCE**，请监视 **SCHEMA_OBJECT_CHANGE_GROUP**。  
  
## <a name="examples"></a>示例  
 以下示例从当前数据库中删除一个名为 `CountBy1` 的序列对象。  
  
```sql  
DROP SEQUENCE CountBy1 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [NEXT VALUE FOR (Transact-SQL)](../../t-sql/functions/next-value-for-transact-sql.md)   
 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
