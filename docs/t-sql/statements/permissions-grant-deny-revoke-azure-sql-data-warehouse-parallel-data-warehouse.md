---
description: 权限：GRANT、DENY、REVOKE（Azure Synapse Analytics 和并行数据仓库）
title: GRANT-DENY-REVOKE 权限
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5a3b7424-408e-4cb0-8957-667ebf4596fc
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: fef6faff2e9e091836afbbe9e2b65f837851aad8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734058"
---
# <a name="permissions-grant-deny-revoke-azure-synapse-analytics-parallel-data-warehouse"></a>权限：GRANT、DENY、REVOKE（Azure Synapse Analytics 和并行数据仓库）
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  使用 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] GRANT 和 DENY 语句授予或拒绝安全主体（例如登录名、数据库用户或数据库角色）对安全对象（例如数据库、表、视图等）的某个权限（例如 UPDATE）  。 使用 REVOKE 删除对某个权限的授予或拒绝。  
  
 服务器级权限适用于登录名。 数据库级权限适用于数据库用户和数据库角色。  
  
 若要查看已授予和拒绝授予的权限，请查询 sys.server_permissions 和 sys.database_permissions 视图。 可通过在具有权限的角色中获得成员身份来继承非显式授予或拒绝授予安全主体的权限。 固定数据库角色的权限无法更改，而且不会出现在 sys.server_permissions 和 sys.database_permissions 视图中。  
  
-   GRANT 显式授予一个或多个权限。  
  
-   DENY 显式拒绝授予主体一个或多个权限。  
  
-   REVOKE 删除现有的 GRANT 或 DENY 权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Azure Synapse Analytics and Parallel Data Warehouse  
GRANT   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
[;]  
  
DENY   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    TO principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
REVOKE   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    [ FROM | TO ] principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
## <a name="arguments"></a>参数  
 \<permission>[ ,...n ]  
 要授予、拒绝授予或撤消的一个或多个权限。  
  
 ON [ \<class_type> :: ] securable：ON 子句描述要对其授予、拒绝授予或撤消权限的安全对象参数。  
  
 \<class_type>：安全对象的类类型。 它可以是 LOGIN、DATABASE、OBJECT、SCHEMA、ROLE 或 USER     。 此外，可向 **SERVER**_class\_type_ 授予权限，但没有为这些权限指定 SERVER。 权限包含单词 DATABASE（例如 ALTER ANY DATABASE）时，不指定 DATABASE。 如果未指定 class_type，并且权限类型不限于服务器或数据库类，则该类假定为 OBJECT。  
  
 securable  
 要授予、拒绝授予或撤销权限的登录名、数据库、表、视图、架构、过程、角色或用户。 可使用 [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md) 中所述的三部分命名规则来指定对象名称。  
  
 TO principal [ ,...n ]  
 被授予、拒绝授予或撤销权限的一个或多个主体。 主体是登录名、数据库用户或数据库角色。  
  
 FROM principal [ ,...n ]  
 要从其撤销权限的一个或多个主体。  主体是登录名、数据库用户或数据库角色。 FROM 只能与 REVOKE 语句配合使用 。 TO 可与 GRANT、DENY 或 REVOKE 配合使用   。  
  
 WITH GRANT OPTION  
 指示被授权者在获得指定权限的同时还可以将指定权限授予其他主体。  
  
 CASCADE  
 指示拒绝授予或撤销指定主体该权限，同时，对该主体授予了该权限的所有其他主体，也拒绝授予或撤销该权限。 当主体具有带 GRANT OPTION 的权限时，为必选项。  
  
 GRANT OPTION FOR  
 指示将撤消授予指定权限的能力。 使用 CASCADE 参数时，为必选项。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
## <a name="permissions"></a>权限  
 若要授予某个权限，授权者必须具有带 WITH GRANT OPTION 的权限本身，或必须具有隐含授予该权限的更高权限。  对象所有者可以授予对其所拥有的对象的权限。 对某安全对象具有 CONTROL 权限的主体可以授予对该安全对象的权限。  db_owner 和 db_securityadmin 固定数据库角色的成员可以授予数据库中的任何权限 。  
  
## <a name="general-remarks"></a>一般备注  
 拒绝或撤消授予给某主体的权限不会影响已通过授权和当前运行的请求。 若要立即限制访问，则必须取消活动请求或终止当前会话。  
  
> [!NOTE]  
>  大部分固定服务器角色在此版本中不可用。 请改用用户定义数据库角色。 无法将登录名添加到 sysadmin 固定服务器角色。 授予 CONTROL SERVER 权限近似拥有 sysadmin 固定服务器角色的成员身份 。  
  
 某些语句需要多个权限。 例如，若要创建表，则需要数据库中的 CREATE TABLE 权限以及将包含该表的表的 ALTER SCHEMA 权限 。  
  
 PDW 有时会执行存储过程，以将用户操作分发到计算节点。 因此，不能拒绝对整个数据库的 execute 权限。 （例如，`DENY EXECUTE ON DATABASE::<name> TO <user>;` 将失败。）作为替代方案，可拒绝对用户架构或特定对象（过程）的 execute 权限。  
  
### <a name="implicit-and-explicit-permissions"></a>隐式和显式权限  
 显式权限是 GRANT 或 DENY 语句给授予某主体的 GRANT 或 DENY 权限   。  
  
 隐式权限是主体（登录名、用户或数据库角色）从另一数据库角色继承的 GRANT 或 DENY 权限 。  
  
 隐式权限还可继承自覆盖的权限或父级权限。 例如，通过对包含该表的架构具有 UPDATE 权限，或对该表具有 CONTROL 权限，可以继承对某个表的 UPDATE 权限  。  
  
### <a name="ownership-chaining"></a>所有权链接  
 当多个数据库对象按顺序互相访问时，该序列便称为“链”。 尽管这样的链不会单独存在，但是当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 遍历链中的链接时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 评估对构成对象的权限时的方式与单独访问对象时不同。 所有权链对管理安全性具有重要的影响。 有关所有权链的详细信息，请参阅[所有权链](https://msdn.microsoft.com/library/ms188676\(v=sql11\).aspx)和[教程：所有权链和上下文切换](../../relational-databases/tutorial-ownership-chains-and-context-switching.md)  
  
## <a name="permission-list"></a>权限列表  
  
### <a name="server-level-permissions"></a>服务器级权限  
 可从登录名授予、拒绝授予或撤销服务器级权限。  
  
 **适用于服务器的权限**  
  
-   CONTROL SERVER  
  
-   ADMINISTER BULK OPERATIONS  
  
-   ALTER ANY CONNECTION  
  
-   ALTER ANY DATABASE  
  
-   CREATE ANY DATABASE  
  
-   ALTER ANY EXTERNAL DATA SOURCE  
  
-   ALTER ANY EXTERNAL FILE FORMAT  
  
-   ALTER ANY LOGIN  
  
-   ALTER SERVER STATE  
  
-   CONNECT SQL  
  
-   VIEW ANY DEFINITION  
  
-   VIEW ANY DATABASE  
  
-   VIEW SERVER STATE  
  
 **适用于登录名的权限**  
  
-   CONTROL ON LOGIN  
  
-   ALTER ON LOGIN  
  
-   IMPERSONATE ON LOGIN  
  
-   VIEW DEFINITION  
  
### <a name="database-level-permissions"></a>数据库级权限  
 可从数据库用户和用户定义数据库角色授予、拒绝和撤销数据库级权限。  
  
 **适用于所有数据库类的权限**  
  
-   CONTROL  
  
-   ALTER  
  
-   VIEW DEFINITION  
  
 **适用于除用户外的所有数据库类的权限**  
  
-   TAKE OWNERSHIP  
  
 **仅适用于数据库的权限**  
  
-   ALTER ANY DATABASE  
  
-   ALTER ON DATABASE  
  
-   ALTER ANY DATASPACE  
  
-   ALTER ANY ROLE  
  
-   ALTER ANY SCHEMA  
  
-   ALTER ANY USER  
  
-   BACKUP DATABASE  
  
-   CONNECT ON DATABASE  
  
-   CREATE PROCEDURE  
  
-   CREATE ROLE  
  
-   CREATE SCHEMA  
  
-   CREATE TABLE  
  
-   CREATE VIEW  
  
-   SHOWPLAN  
  
 **仅适用于用户的权限**  
  
-   IMPERSONATE  
  
 **适用于数据库、架构和对象的权限**  
  
-   ALTER  
  
-   DELETE  
  
-   EXECUTE  
  
-   INSERT  
  
-   SELECT  
  
-   UPDATE  
  
-   REFERENCES  
  
 有关每种类型的权限定义，请参阅[权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)。  
  
### <a name="chart-of-permissions"></a>权限的图表  
 所有权限均以图形方式呈现在此发布程序上。 这是查看权限的嵌套层次结构最简单的方法。 例如，ALTER ON LOGIN 权限可由其本身授予，但如果授予某一登录名对该登录名的 CONTROL 权限，或者授予某一登录名 ALTER ANY LOGIN 权限，它也包含在内  。  
  
 ![APS 安全性权限发布者](../../t-sql/statements/media/aps-security-perms-poster.png "APS 安全性权限发布者")  
  
  
## <a name="default-permissions"></a>默认权限  
 以下列表对默认权限进行了说明：  
  
-   使用 CREATE LOGIN 语句创建登录名时，新登录名会收到 CONNECT SQL 权限 。  
  
-   所有登录名都是公共服务器角色的成员，无法从其删除 。  
  
-   使用 CREATE USER 权限创建数据库用户时，数据库用户会收到数据库中的 CONNECT 权限 。  
  
-   包括公共角色在内的所有主体，默认情况下都无任何显式或隐式权限。  
  
-   登录名或用户成为数据库或对象的所有者时，登录名或用户始终拥有对数据库或对象的所有权限。 所有者权限无法更改，也不能显示为显式权限。 GRANT、DENY 和 REVOKE 语句不会对所有者产生任何影响  。  
  
-   sa 登录名具有设备上的所有权限。 类似于所有者权限，sa 权限无法更改，也不能显示为显式权限。 GRANT、DENY 和 REVOKE 语句不会对 sa 登录名产生任何影响   。 无法重命名 sa 登录名。  
  
-   USE 语句不需要权限。 所有主体都可在任何数据库上运行 USE 语句。  
  
##  <a name="examples-sssdw-and-sspdw"></a><a name="Examples"></a> 示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-granting-a-server-level-permission-to-a-login"></a>A. 为登录名授予服务器级权限  
 以下两个语句为登录名授予服务器级权限。  
  
```sql  
GRANT CONTROL SERVER TO [Ted];  
```  
  
```sql  
GRANT ALTER ANY DATABASE TO Mary;  
```  
  
### <a name="b-granting-a-server-level-permission-to-a-login"></a>B. 为登录名授予服务器级权限  
 以下示例为服务器主体（另一登录名）授予对某一登录名的服务器级权限。  
  
```sql  
GRANT  VIEW DEFINITION ON LOGIN::Ted TO Mary;  
```  
  
### <a name="c-granting-a-database-level-permission-to-a-user"></a>C. 为用户授予数据库级权限  
 以下示例为数据库主体（另一用户）授予对某一用户的数据库级权限。  
  
```sql  
GRANT VIEW DEFINITION ON USER::[Ted] TO Mary;  
```  
  
### <a name="d-granting-denying-and-revoking-a-schema-permission"></a>D. 授予、拒绝授予和撤消架构权限  
 以下 GRANT 语句授予 Yuen 从 dbo 架构的任何表或视图中选择数据的能力。  
  
```sql  
GRANT SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 以下 DENY 语句阻止 Yuen 从 dbo 架构的任何表或视图中选择数据。 即使 Yuen 以某种其他方式（例如，通过角色成员身份）获得权限，他也无法读取数据。  
  
```sql  
DENY SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
 以下 REVOKE 语句会删除 DENY 权限 。 现在，Yuen 的显式权限为中性。 Yuen 可以通过其他隐式权限（如角色成员身份）从任何表中选择数据。  
  
```sql  
REVOKE SELECT ON SCHEMA::dbo TO [Yuen];  
```  
  
### <a name="e-demonstrating-the-optional-object-clause"></a>E. 演示可选的 OBJECT:: 子句  
 OBJECT 是权限语句的默认类，因此以下两个语句相同。 OBJECT:: 子句为可选项。  
  
```sql  
GRANT UPDATE ON OBJECT::dbo.StatusTable TO [Ted];  
```  
  
```sql  
GRANT UPDATE ON dbo.StatusTable TO [Ted];  
```  
  
