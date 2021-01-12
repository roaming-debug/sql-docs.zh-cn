---
description: DROP XML SCHEMA COLLECTION (Transact-SQL)
title: DROP XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP XML SCHEMA COLLECTION
- DROP_XML_SCHEMA_COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting XML schema collections
- XML schema collections [SQL Server], removing
- schema collections [SQL Server], removing
- removing XML schema collections
- dropping XML schema collections
- DROP XML SCHEMA COLLECTION statement
ms.assetid: d686f2f5-e03a-4ffe-a566-6036628f46f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 87ce71446016c59ee888648a6557b4cbc0575519
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102280"
---
# <a name="drop-xml-schema-collection-transact-sql"></a>DROP XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

删除整个 XML 架构集合及其所有组件。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
relational_schema  
标识关系架构的名称。 如果未指定，则假定为默认的关系架构。  
  
sql_identifier  
要删除的 XML 架构集合的名称。  
  
## <a name="remarks"></a>注解  
删除 XML 架构集合属于事务性操作。 如果删除事务内的 XML 架构集合，然后回滚此事务，则 XML 架构集合不会被删除。  
  
当 XML 架构集合在使用时，不能将其删除。 因此，要删除的集合不能存在下列任何情况：  
  
-   与任何 **xml** 类型参数或列关联。  
  
-   在任何表约束中指定。  
  
-   被绑定到架构的函数或存储过程中引用。 例如，以下函数将锁定 XML 架构集合 `MyCollection`，因为此函数指定了 `WITH SCHEMABINDING`。 如果将其删除，则 XML SCHEMA COLLECTION 中将不存在锁。  
  
    ```sql  
    CREATE FUNCTION dbo.MyFunction()  
    RETURNS int  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
       ...  
       DECLARE @x XML(MyCollection)  
       ...  
    END;  
    ```  
  
## <a name="permissions"></a>权限  
删除 XML SCHEMA COLLECTION 需要对集合具有 DROP 权限。  
  
## <a name="examples"></a>示例  
以下示例显示如何删除 XML 架构集合。  
  
```sql  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [ALTER XML SCHEMA COLLECTION (Transact SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [在服务器上使用 XML 架构集合的要求和限制](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
