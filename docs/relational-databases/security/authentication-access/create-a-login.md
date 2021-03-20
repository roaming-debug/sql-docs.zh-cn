---
title: 创建登录名 | Microsoft Docs
description: 了解如何通过 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 或 Azure SQL 数据库中创建登录名。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.login.status.f1
- sql13.swb.login.effectivepermissions.f1
- sql13.swb.login.general.f1
- sql13.swb.login.databaseaccess.f1
- sql13.swb.login.serverroles.f1
helpviewer_keywords:
- authentication [SQL Server], logins
- logins [SQL Server], creating
- creating logins with Management Studio
- Create login [SQL Server]
- SQL Server logins
ms.assetid: fb163e47-1546-4682-abaa-8c9494e9ddc7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25abb0069aa14105f27c581614917a11a83ea127
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755897"
---
# <a name="create-a-login"></a>创建一个登录名
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建登录名。 登录名是连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例的个人或进程的标识。  
  
##  <a name="background"></a><a name="Background"></a> 背景  
 登录名是一个可由安全系统进行身份验证的安全主体或实体。 用户需要使用登录名连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 您可基于 Windows 主体（例如，域用户或 Windows 域组）创建登录名，或者也可创建一个并非基于 Windows 主体的登录名（例如， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名）。  
  
> **注意：** 若要使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，[!INCLUDE[ssDE](../../../includes/ssde-md.md)]必须使用混合模式身份验证。 有关详细信息，请参阅 [选择身份验证模式](../../../relational-databases/security/choose-an-authentication-mode.md)。  
  
 可以向作为安全主体的登录名授予权限。 登录名的作用域是整个 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]。 若要连接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]实例上的特定数据库，登录名必须映射到数据库用户。 数据库内的权限是向数据库用户而不是登录名授予和拒绝授予的。 可将作用域为整个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的权限（例如 **CREATE ENDPOINT** 权限）授予一个登录名。  
  
> **注意：** 当登录名连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，在 master 数据库验证标识。 使用包含的数据库用户在数据库级别对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 连接进行身份验证。 当使用包含的数据库用户时，登录名不是必需的。 “包含的数据库”是独立于其他数据库以及承载数据库的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]/ [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 实例（和 master 数据库）的一种数据库。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持包含的数据库用户进行 Windows 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。 使用 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]时，将包含的数据库用户与数据库级别防火墙规则相结合。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
##  <a name="security"></a><a name="Security"></a> Security  

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 需要对服务器拥有 **ALTER ANY LOGIN** 或 **ALTER LOGIN** 权限。  
  
 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 要求具有 **loginmanager** 角色的成员身份。  
  
##  <a name="create-a-login-using-ssms"></a><a name="SSMSProcedure"></a> 使用 SSMS 创建登录名  
  
  
1.  在对象资源管理器中，展开要在其中创建新登录名的服务器实例的文件夹。  
  
2.  右键单击“安全性”文件夹，指向“新建”，然后选择“登录名…”  。  
  
3.  在“登录名 - 新建”对话框的“常规”页中，在“登录名”框中输入用户的名称  。 或者，单击“搜索…”以打开“选择用户或组”对话框 。  
  
     如果单击“搜索…”：  
  
    1.  在“选择此对象类型”下，单击“对象类型…”以打开“对象类型”对话框，并选择以下任意或全部选项：  “内置安全主体”、“组”和“用户”  。 默认情况下，将选中“内置安全主体”和“用户”。 完成后，单击 **“确定”** 。  
  
    2.  在“从此位置”下，单击“位置…”以打开“位置”对话框，并选择一个可用的服务器位置  。 完成后，单击 **“确定”** 。  
  
    3.  在“输入要选择的对象名称（示例）”下，输入你想要查找的用户或组名。 有关详细信息，请参阅 [“选择用户、计算机或组”对话框](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771712(v=ws.11))。  
  
    4.  单击“高级…”以显示更多高级搜索选项。 有关详细信息，请参阅 [选择“用户”、“计算机”或“组”对话框 - 高级页面](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc733110(v=ws.11))。  
  
    5.  单击“确定”。  
  
4.  若要基于 Windows 主体创建一个登录名，请选择 **“Windows 身份验证”** 。 这是默认选项。  
  
5.  若要创建一个保存在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中的登录名，请选择 **“SQL Server 身份验证”** 。  
  
    1.  在“密码”框中，输入新用户的密码。 在“确认密码”框中再次输入该密码。  
  
    2.  在更改现有密码时，选择 **“指定旧密码”** ，然后在 **“旧密码”** 框中键入旧密码。  
  
    3.  若要强制实施有关复杂性和强制执行的密码策略选项，请选择 **“强制实施密码策略”** 。 有关详细信息，请参阅 [Password Policy](../../../relational-databases/security/password-policy.md)。 选中 **“SQL Server 身份验证”** 时，这是默认选项。  
  
    4.  若要强制实施有关过期的密码策略选项，请选中 **“强制密码过期”** 。 必须选择 **“强制实施密码策略”** 才能启用此复选框。 选中 **“SQL Server 身份验证”** 时，这是默认选项。  
  
    5.  若要在首次使用登录名后强制用户创建新密码，请选择 **“用户在下次登录时必须更改密码”** 。 必须选择 **“强制密码过期”** 才能启用此复选框。 选中 **“SQL Server 身份验证”** 时，这是默认选项。  
  
6.  若要将登录名与独立的安全性证书相关联，请选择“映射到证书”，然后再从列表中选择现有证书的名称。  
  
7.  若要将登录名与独立的非对称密钥相关联，请选择“映射到非对称密钥”，然后再从列表中选择现有密钥的名称。  
  
8.  若要将登录名与安全凭据相关联，请选中 **“映射到凭据”** 复选框，然后再从列表中选择现有凭据或单击 **“添加”** 以创建新的凭据。 若要从登录名删除与某个安全凭据的映射，请从 **“映射的凭据”** 中选择该凭据，然后单击 **“删除”** 。 有关常规凭据的详细信息，请参阅[凭据（数据库引擎）](../../../relational-databases/security/authentication-access/credentials-database-engine.md)。  
  
9. 从 **“默认数据库”** 列表中，选择登录名的默认数据库。 **“Master”** 是此选项的默认值。  
  
10. 从 **“默认语言”** 列表中，选择登录名的默认语言。  
  
11. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="additional-options"></a>其他选项  
 “登录名 - 新建”对话框中还提供了其他四个页面上选项：“服务器角色”、“用户映射”、“安全对象”和“状态”   。  
  
### <a name="server-roles"></a>“服务器角色”  
 **“服务器角色”** 页将列出可分配给新登录名的所有可能的角色。 提供了以下选项：  
  
 “bulkadmin”复选框  
 **bulkadmin** 固定服务器角色的成员可以运行 BULK INSERT 语句。  
  
 “dbcreator”复选框  
 **dbcreator** 固定服务器角色的成员可以创建、更改、删除和还原任何数据库。  
  
 “diskadmin”复选框  
 **diskadmin** 固定服务器角色的成员可以管理磁盘文件。  
  
 “processadmin”复选框  
 **processadmin** 固定服务器角色的成员可以终止在 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]实例中运行的进程。  
  
 “public”复选框  
 默认情况下，所有 SQL Server 用户、组和角色都属于 **public** 固定服务器角色。  
  
 “securityadmin”复选框  
 **securityadmin** 固定服务器角色的成员可以管理登录名及其属性。 他们可以 GRANT、DENY 和 REVOKE 服务器级别的权限。 他们还可以 GRANT、DENY 和 REVOKE 数据库级别的权限。 此外，他们还可以重置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名的密码。  
  
 “serveradmin”复选框  
 **serveradmin** 固定服务器角色的成员可以更改服务器范围的配置选项和关闭服务器。  
  
 “setupadmin”复选框  
 **setupadmin** 固定服务器角色成员可以添加和删除链接服务器，并可以执行某些系统存储过程。  
  
 “sysadmin”复选框  
 **sysadmin** 固定服务器角色的成员可以在 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]中执行任何活动。  
  
### <a name="user-mapping"></a>用户映射  
 **“用户映射”** 页将列出可应用于登录名的所有可能的数据库以及这些数据库上的数据库角色成员身份。 选定的数据库将确定对登录名可用的角色成员身份。 此页还将提供以下选项：  
  
 **映射到此登录名的用户**  
 选择此登录名可以访问的数据库。 选择某个数据库时，其有效的数据库角色将会显示在“数据库角色成员身份: _database_name_”窗格中。  
  
 **Map**  
 允许登录名访问下面列出的数据库。  
  
 **Database**  
 列出服务器上可用的数据库。  
  
 **用户**  
 指定要映射到登录名的数据库用户。 默认情况下，数据库用户名与登录名相同。  
  
 **默认架构**  
 指定用户的默认架构。 首次创建用户时，其默认架构是 **dbo**。 可以指定并不存在的默认架构。 对于已映射到 Windows 组、证书或非对称密钥的用户，无法为其指定默认架构。  
  
 已启用 Guest 帐户：_database_name_  
 只读属性，指示所选数据库是否已启用 Guest 帐户。 可使用 Guest 帐户的 **“登录属性”** 对话框的 **“状态”** 页来启用或禁用 Guest 帐户。  
  
 数据库角色成员身份：_database_name_  
 选择用户在指定数据库中的角色。 在每个数据库中，所有用户都是 **public** 角色的成员，并且不能被删除。 有关数据库角色的详细信息，请参阅 [数据库级别的角色](../../../relational-databases/security/authentication-access/database-level-roles.md)。  
  
### <a name="securables"></a>安全对象  
 **“安全对象”** 页将列出所有可能的安全对象以及可授予登录名的针对这些安全对象的权限。 此页还将提供以下选项：  
  
 **上部网格**  
 包含一个或多个可以为其设置权限的项目。 上部网格中显示的列会根据主体或安全对象的不同而变化。  
  
 向上部网格中添加项目：  
  
1.  单击 **“搜索”** 。  
  
2.  在“添加对象”对话框中，选择以下选项之一：“特定对象...”、“所有类型的对象...”或“服务器 server\_name”  。 [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    > **注意：** 如果选择“服务器 server\_name”，将使用该服务器的所有安全对象自动填充上部网格。  
  
3.  如果选择“特定对象…”：  
  
    1.  在“选择对象”对话框中的“选择这些对象类型”下，单击“对象类型…”  。  
  
    2.  在“选择对象类型”对话框中，选择以下任意或全部对象类型：“端点”、“登录名”、“服务器”、“可用性组”和“服务器角色”    。 [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
    3.  在“输入要选择的对象名称(示例)”下，单击“浏览…” 。  
  
    4.  在 **“查找对象”** 对话框中，选择您在 **“选择对象类型”** 对话框中选择的类型的任何可用对象，然后单击 **“确定”** 。  
  
    5.  在 **“选择对象”** 对话框中，单击 **“确定”** 。  
  
4.  如果选择“所有类型的对象…”，请在“选择对象类型”对话框中，选择以下任意或全部对象类型： “端点”、“登录名”、“服务器”、“可用性组”和“服务器角色”    。 [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
 **名称**  
 添加到网格中的每个主体或安全对象的名称。  
  
 类型  
 描述每个项目的类型。  
  
 **“显式”选项卡**  
 列出了上部网格中选定的安全对象的可能权限。 并非所有选项均用于任何显式权限。  
  
 **权限**  
 权限的名称。  
  
 **授权者**  
 授予该权限的主体。  
  
 **授予**  
 选中该选项可以将此权限授予该登录名。 清除该选项将撤消此权限。  
  
 **具有授予权限**  
 反映所列权限的 WITH GRANT 选项的状态。 此框是只读的。 若要应用此权限，请使用 [GRANT](../../../t-sql/statements/grant-transact-sql.md) 语句。  
  
 **拒绝**  
 选中该选项可以拒绝该登录名具有该权限。 清除该选项将撤消此权限。  
  
### <a name="status"></a>状态  
 **“状态”** 页将列出可对选定的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名配置的一些身份验证和授权选项。  
  
 此页还将提供以下选项：  
  
 **连接到数据库引擎的权限**  
 当使用此设置时，应将所选登录名视为可授予或拒绝授予其对安全对象的访问权限的主体。  
  
 如果选择 **“授予”** ，将向登录名授予 CONNECT SQL 权限。 如果选择 **“拒绝”** ，将拒绝向登录名授予 CONNECT SQL 权限。  
  
 **登录**  
 当使用此设置时，应将所选登录名视为表中的记录。 对此处列出的值的更改将应用于该记录。  
  
 已禁用的登录名继续作为记录存在。 但是如果它尝试连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，则登录名将不能通过身份验证。  
  
 选择此选项以启用或禁用此登录名。 此选项将 ALTER LOGIN 语句与 ENABLE 或者 DISABLE 选项配合使用。  
  
 **SQL Server 身份验证**  
 仅当所选的登录名使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证进行连接并且登录名已锁定时，复选框“登录名已锁定”才可用。该设置是只读的。 若要解除对已锁定登录名的锁定，请执行带 UNLOCK 选项的 ALTER LOGIN。  
  
##  <a name="create-a-login-using-windows-authentication-using-t-sql"></a><a name="TsqlProcedure"></a> 使用 T-SQL 创建使用 Windows 身份验证的登录名  
  
 
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Create a login for SQL Server by specifying a server name and a Windows domain account name.  
  
    CREATE LOGIN [<domainName>\<loginName>] FROM WINDOWS;  
    GO  
  
    ```  
  
## <a name="create-a-login-using-sql-server-authentication-using-t-sql"></a>使用 T-SQL 创建使用 SQL Server 身份验证的登录名
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates the user "shcooper" for SQL Server using the security credential "RestrictedFaculty"   
    -- The user login starts with the password "Baz1nga," but that password must be changed after the first login.  
  
    CREATE LOGIN shcooper   
       WITH PASSWORD = 'Baz1nga' MUST_CHANGE,  
       CREDENTIAL = RestrictedFaculty;  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE LOGIN (Transact-SQL)](../../../t-sql/statements/create-login-transact-sql.md)。  
  
##  <a name="follow-up-steps-to-take-after-you-create-a-login"></a><a name="FollowUp"></a> 跟进：在创建登录名后采取的步骤  
 创建登录名后，该登录名可连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]，但不一定有执行任何有用工作的充分权限。 下面的列表提供了指向常见登录操作的链接。  
  
-   若要将登录名加入某一角色，请参阅 [加入角色](../../../relational-databases/security/authentication-access/join-a-role.md)。  
  
-   若要授权登录名使用数据库，请参阅 [创建数据库用户](../../../relational-databases/security/authentication-access/create-a-database-user.md)。  
  
-   若要向登录名授予权限，请参阅 [向主体授予权限](../../../relational-databases/security/authentication-access/grant-a-permission-to-a-principal.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库引擎和 Azure SQL Database 的安全中心](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
