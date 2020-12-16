---
title: 创建数据库用户 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 创建最常见类型的数据库用户。
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.user.securables.f1
- SQL13.SWB.DATABASEUSER.GENERAL.F1
helpviewer_keywords:
- database users, creating
- creating users with Management Studio
- mapping users
- users [SQL Server], creating
- database user additions [SQL Server]
- database users, mapping
- CREATE USER [Management Studio]
- users [SQL Server], adding
- mapping database users
ms.assetid: 782798d3-9552-4514-9f58-e87be4b264e4
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c06db04136d3e38ad62fe71313490c0a54670c46
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479428"
---
# <a name="create-a-database-user"></a>创建数据库用户
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题介绍如何创建最常见类型的数据库用户。 有十一种类型的用户。 主题 [CREATE USER (Transact-SQL)](../../../t-sql/statements/create-user-transact-sql.md) 中提供了完整列表。 所有类型的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 都支持数据库用户，但不一定支持所有类型的用户。  
  
 可以通过使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)]来创建数据库用户。  
  
##  <a name="understanding-the-types-of-users"></a><a name="Understanding"></a> 了解用户类型  
 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 提供了创建数据库用户时的 6 个选项。 下图在绿框中显示了这 6 个选项，并展示了选项所代表的含义。  
  
 ![说明不同类型用户的关系图。](../../../relational-databases/security/authentication-access/media/typesofusers.png "TypesOfUsers")  
  
### <a name="selecting-the-type-of-user"></a>选择用户类型  
 **登录名或没有映射到登录名的用户**  
  
 如果对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]不熟悉，可能很难决定要创建哪种类型的用户。 首先问问自己，需要访问数据库的用户或组是否有登录名？ 管理 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的用户和需要访问 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例上的多个或者全部数据库的用户通常拥有主数据库中的登录名。 在这种情况下，你需要创建一个“带登录名的 SQL 用户” 。 数据库用户是连接到数据库时的登录名的标识。 数据库用户可以使用与登录名相同的名称，但这不是必需的。 本主题假设 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中已存在登录名。 有关如何创建登录名的信息，请参阅 [创建登录名](../../../relational-databases/security/authentication-access/create-a-login.md)  
  
 如果需要访问数据库的用户和组没有登录名，并且他们只需要访问一个或少数几个数据库，则创建 **Windows 用户** 或者 **带密码的 SQL 用户**。 也称为包含的数据库用户，它与主数据库的登录名不相关。 当你想在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的实例间轻松地移动数据库时，这是一个理想的选择。 若要对 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)]使用此选项，管理员必须首先对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]启用包含的数据库，然后对包含启用数据库。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
> **重要说明！** 作为包含的数据库用户进行连接时，必须在连接字符串中提供数据库的名称。 若要在 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]中指定数据库，在“连接到”  对话框中单击“选项” ，然后单击“连接属性”  选项卡。  
  
 当用户连接无法使用 Windows 进行身份验证时，选择“带密码的 SQL 用户”  或者“带用户名的 SQL 用户”  ，具体取决于 **SQL Server 身份验证登录名**。 组织外的用户（例如客户）连接到你的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]时这种情况很常见。  
  
> **提示！** 对于组织内的用户，最好选择使用 Windows 身份验证。因为组织内的用户不需要记住其他密码，而且 Windows 身份验证可以提供其他安全功能，例如 Kerberos。  
  
##  <a name="background"></a><a name="Restrictions"></a> 背景  
 用户是数据库级别安全主体。 登录名必须映射到数据库用户才能连接到数据库。 一个登录名可以作为不同用户映射到不同的数据库，但在每个数据库中只能作为一个用户进行映射。 在部分包含数据库中，可以创建不具有登录名的用户。 有关包含的数据库用户的详细信息，请参阅 [CREATE USER (Transact-SQL)](../../../t-sql/statements/create-user-transact-sql.md)。 如果在数据库中启用了 guest 用户，未映射到数据库用户的登录名可作为 guest 用户进入该数据库。  
  
> **重要说明！** guest 用户通常处于禁用状态。 除非有必要，否则不要启用 guest 用户。  
  
 可以向作为安全主体的用户授予权限。 用户的作用域是数据库。 若要连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例上的特定数据库，登录名必须映射到数据库用户。 数据库内的权限是向数据库用户而不是登录名授予和拒绝授予的。  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库拥有 **ALTER ANY USER** 权限。  
  
##  <a name="create-a-user-with-ssms"></a><a name="SSMSProcedure"></a> 使用 SSMS 创建用户  
  
 
1.  在对象资源管理器中，展开 **“数据库”** 文件夹。  
  
2.  展开要在其中创建新数据库用户的数据库。  
  
3.  右键单击“安全性”文件夹，指向“新建”，然后选择“用户…”  。  
  
4.  在“常规”页上的“数据库用户 - 新建”对话框中，从“用户类型”列表中选择以下一个用户类型  ：  
  
    -   **带登录名的 SQL 用户**  
  
    -   **带密码的 SQL 用户**  
  
    -   **不带登录名的 SQL 用户**  
  
    -   **映射到证书的用户**  
  
    -   **映射到非对称密钥的用户**  
  
    -   **Windows 用户**  
  
5.  选择选项时，对话框中的其他选项可能改变。 某些选项仅适用于特定类型的数据库用户。 某些选项可以为空，并且将使用默认值。  
  
     **用户名**  
     输入新用户的名称。 如果你从“用户类型”列表中选择了“Windows 用户”，则还可以单击省略号 (…) 打开“选择用户或组”对话框   。  
  
     **登录名**  
     输入用户的登录名。 或者，单击省略号 (…) 以打开“选择登录名”对话框 。 如果您从 **“用户类型”** 列表中选择了 **“带登录名的 SQL 用户”** 或 **“Windows 用户”** ，则 **“登录名”** 可用。  
  
     **“密码”** 和 **“确认密码”**  
     输入在数据库中进行身份验证的用户的密码。  
  
     **默认语言**  
     输入默认的用户语言。  
  
     **默认架构**  
     输入此用户所创建的对象所属的架构。 或者，单击省略号 (…) 以打开“选择架构”对话框 。 如果您从 **“用户类型”** 列表中选择了 **“带登录名的 SQL 用户”** , **“不带登录名的 SQL 用户”** 或 **“Windows 用户”** ，则 **“默认架构”** 可用。  
  
     **证书名称**  
     输入将用于数据库用户的证书。 或者，单击省略号 (…) 以打开“选择证书”对话框 。 如果从 **“用户类型”** 列表中选择了 **“映射到证书的用户”** ，则 **“证书名称”** 可用。  
  
     **非对称密钥名称**  
     输入将用于数据库用户的密钥。 或者，单击省略号 (…) 以打开“选择非对称密钥”对话框 。 如果从 **“用户类型”** 列表中选择了 **“映射到非对称密钥的用户”** ，则 **“非对称密钥名称”** 可用。  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="additional-options"></a>其他选项  
 “数据库用户 - 新建”对话框还提供了四个其他页上的选项：“拥有的架构”、“成员身份”、“安全对象”和“扩展属性”   。  
  
-   **“拥有的架构”** 页列出了可由新的数据库用户拥有的所有可能的架构。 若要向数据库用户添加架构或者从数据库用户中删除架构，请在 **“此用户拥有的架构”** 下选中或取消选中架构旁边的复选框。  
  
-   **“成员身份”** 页列出了可由新的数据库用户拥有的所有可能的数据库成员身份角色。 若要向数据库用户添加角色或者从数据库用户中删除角色，请在 **“数据库角色成员身份”** 下选中或取消选中角色旁边的复选框。  
  
-   **“安全对象”** 页将列出所有可能的安全对象以及可授予登录名的针对这些安全对象的权限。  
  
-   **“扩展属性”** 页允许您向数据库用户添加自定义属性。 此页还提供以下选项：  
  
     **Database**  
     显示所选数据库的名称。 此字段为只读。  
  
     **排序规则**  
     显示用于所选数据库的排序规则。 此字段为只读。  
  
     **属性**  
     查看或指定对象的扩展属性。 每个扩展属性都由与该对象关联的元数据的名称/值对组成。  
  
     **省略号 (...)**  
     单击“值”后面的省略号 (…) 按钮可打开“已扩展属性的值”对话框  。 在这一较大的范围中键入或查看扩展属性的值。 有关详细信息，请参阅 [“扩展属性的值”对话框](../../databases/value-for-extended-property-dialog-box.md)。  
  
     **删除**  
     删除所选扩展属性。  
  
##  <a name="create-a-user-using-t-sql"></a><a name="TsqlProcedure"></a> 使用 T-SQL 创建用户  
    
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在“标准”  菜单栏上，单击“新建查询” 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates the login AbolrousHazem with password '340$Uuxwp7Mcxo7Khy'.  
    CREATE LOGIN AbolrousHazem   
        WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
    GO  
  
    -- Creates a database user for the login created above.  
    CREATE USER AbolrousHazem FOR LOGIN AbolrousHazem;  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE USER (Transact-SQL&)](../../../t-sql/statements/create-user-transact-sql.md)，其中包含更多的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 示例。  
  
## <a name="see-also"></a>另请参阅  
 [主体（数据库引擎）](../../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [创建登录名](../../../relational-databases/security/authentication-access/create-a-login.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
  
