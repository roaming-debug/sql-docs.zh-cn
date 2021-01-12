---
title: REVOKE XML 架构集合权限
description: 使用 Transact-SQL 来 REVOKE XML 架构集合权限。
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, XML schema collections
- XML schema collections [SQL Server], permissions
- schema collections [SQL Server], permissions
ms.assetid: 8ca0973c-30b2-4633-a165-c09b13cc81ae
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a7fab5fc48b055fb1a5cf253bfa343e517656cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091863"
---
# <a name="revoke-xml-schema-collection-permissions-transact-sql"></a>REVOKE XML 架构集合权限 (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  撤消授予或拒绝的 XML 架构集合权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] ON   
    XML SCHEMA COLLECTION :: [ schema_name . ]  
    XML_schema_collection_name  
    { TO | FROM } <database_principal> [ ,...n ]  
        [ CASCADE ]  
    [ AS <database_principal> ]   
  
<database_principal> ::=   
        Database_user   
    | Database_role   
    | Application_role   
    | Database_user_mapped_to_Windows_User   
    | Database_user_mapped_to_Windows_Group   
    | Database_user_mapped_to_certificate   
    | Database_user_mapped_to_asymmetric_key   
    | Database_user_with_no_login   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可撤消的对 XML 架构集合的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON XML SCHEMA COLLECTION :: [ schema_name. ] XML_schema_collection_name  
 指定要对其撤消权限的 XML 架构集合。 需要作用域限定符 ::。 如果未指定 schema_name，则使用默认架构。 如果指定了 schema_name，则需要使用架构作用域限定符 (.)。  
  
 GRANT OPTION  
 指示要撤消向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 { TO | FROM } \<*database_principal*>  
 指定要从中撤消权限的主体。  
  
 AS \<database_principal>：指定一个主体，执行该查询的主体从中获得撤消权限的权利。  
  
 Database_user  
 指定数据库用户。  
  
 Database_role  
 指定数据库角色。  
  
 Application_role  
 指定应用程序角色。  
  
 Database_user_mapped_to_Windows_User  
 指定映射到 Windows 用户的数据库用户。  
  
 Database_user_mapped_to_Windows_Group  
 指定映射到 Windows 组的数据库用户。  
  
 Database_user_mapped_to_certificate  
 指定映射到证书的数据库用户。  
  
 Database_user_mapped_to_asymmetric_key  
 指定映射到非对称密钥的数据库用户。  
  
 Database_user_with_no_login  
 指定无相应服务器级主体的数据库用户。  
  
## <a name="remarks"></a>备注  
 可以在 [sys.xml_schema_collections](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md) 目录视图中查看 XML 架构集合的有关信息。  
  
 如果从通过指定 GRANT OPTION 被授权的主体中撤消权限，但是未指定 CASCADE，则语句将失败。  
  
 XML 架构集合是架构级的安全对象，包含于权限层次结构中作为其父级的架构中。 下表列出了可撤消的对 XML 架构集合最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|XML 架构集合权限|XML 架构集合权限隐含的权限|架构权限隐含的权限|  
|--------------------------------------|-------------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER|  
|CONTROL|CONTROL|CONTROL|  
|EXECUTE|CONTROL|EXECUTE|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对 XML 架构集合具有 CONTROL 权限。 若要使用 AS 选项，则指定的主体必须拥有 XML 架构集合。  
  
## <a name="examples"></a>示例  
 以下示例从用户 `EXECUTE` 中撤消对 XML 架构集合 `Invoices4` 的 `Wanida` 权限。 XML 架构集合 `Invoices4` 位于 `Sales` 数据库的 `AdventureWorks2012` 架构中。  
  
 ```sql
 USE AdventureWorks2012;  
 REVOKE EXECUTE ON XML SCHEMA COLLECTION::Sales.Invoices4 FROM Wanida;  
 GO
 ```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT XML 架构集合权限 (Transact-SQL)](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)   
 [DENY XML 架构集合权限 (Transact-SQL)](../../t-sql/statements/deny-xml-schema-collection-permissions-transact-sql.md)   
 [sys.xml_schema_collections (Transact-SQL)](../../relational-databases/system-catalog-views/sys-xml-schema-collections-transact-sql.md)   
 [CREATE XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

