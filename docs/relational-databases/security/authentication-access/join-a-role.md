---
title: 加入角色 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-sql 向 SQL Server 中的登录名和数据库用户分配角色。 使用角色管理权限。
ms.custom: ''
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.DATABASEUSER.MEMBERSHIP.F1
helpviewer_keywords:
- adding a member to a role
- join a role
ms.assetid: 05c8d10d-5823-46c6-8b1a-81722da6a42b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d4dfa6ec2916badfafc0ebe6aa826cefb636f6b1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750417"
---
# <a name="join-a-role"></a>加入角色
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中向登录名和数据库用户分配角色。 可使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的角色高效地管理权限。 将权限分配给角色，然后在角色中添加和删除用户以及登录名。 通过使用角色，不必针对各个用户单独维护权限。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持四种类型的角色。  
  
-   固定服务器角色  
  
-   用户定义的服务器角色  
  
-   固定的数据库角色  
  
-   用户定义的数据库角色  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中会自动提供固定角色。 固定角色具有完成常见任务必需的权限。 有关固定角色的详细信息，请参阅以下链接。 用户定义的角色由您创建，并可使用选择的权限进行自定义。 有关用户定义角色的详细信息，请参阅以下链接。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要向登录名和数据库用户分配角色，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   更改数据库角色的名称不会更改角色的 ID 号、所有者或权限。  
  
-   在 sys.database_role_members 和 sys.database_principals 目录视图中可以查看数据库角色。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求对数据库具有 **ALTER ANY ROLE** 权限、对角色具有 **ALTER** 权限或具有 **db_securityadmin** 中的成员身份。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>向固定服务器角色添加成员  
  
1.  在“对象资源管理器”中，展开要在其中编辑固定服务器角色的服务器。  
  
2.  展开 **“安全性”** 文件夹。  
  
3.  展开 **“服务器角色”** 文件夹。  
  
4.  右键单击要编辑的角色，然后选择“属性”。  
  
5.  在“服务器角色属性 - _server\_role\_name_”对话框的“成员”页中，单击“添加”  。  
  
6.  在“选择服务器登录名或角色”对话框的“输入要选择的对象名称(示例)”下，输入要添加到该服务器角色的登录名或服务器角色。 或者，单击“浏览...”，然后在“浏览对象”对话框中选择任意对象或所有可用对象 。 单击“确定”以返回“服务器角色属性 - _server\_role\_name_”对话框 。  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>向用户定义的数据库角色添加成员  
  
1.  在“对象资源管理器”中，展开要在其中编辑用户定义的数据库角色的服务器。  
  
2.  展开 **“数据库”** 文件夹。  
  
3.  展开要在其中编辑用户定义的数据库角色的数据库。  
  
4.  展开 **“安全性”** 文件夹。  
  
5.  展开 **“角色”** 文件夹。  
  
6.  展开 **“服务器角色”** 文件夹。  
  
7.  右键单击要编辑的角色，然后选择“属性”。  
  
8.  在“数据库角色属性 - _database\_role\_name_”对话框的“常规”页中，单击“添加”  。  
  
9. 在“选择数据库用户或角色”对话框的“输入要选择的对象名称(示例)”下，输入要添加到该数据库角色的登录名或数据库角色。 或者，单击“浏览...”，然后在“浏览对象”对话框中选择任意对象或所有可用对象 。 单击“确定”以返回“数据库角色属性 - _database\_role\_name_”对话框 。  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-add-a-member-to-a-fixed-server-role"></a>向固定服务器角色添加成员  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    ALTER SERVER ROLE diskadmin ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 有关详细信息，请参阅 [ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md)。  
  
#### <a name="to-add-a-member-to-a-user-defined-database-role"></a>向用户定义的数据库角色添加成员  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    ALTER ROLE Marketing ADD MEMBER [Domain\Juan] ;  
    GO  
    ```  
  
 有关详细信息，请参阅 [sp_addrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [服务器级别角色](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [数据库级别的角色](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [应用程序角色](../../../relational-databases/security/authentication-access/application-roles.md)  
  
  
