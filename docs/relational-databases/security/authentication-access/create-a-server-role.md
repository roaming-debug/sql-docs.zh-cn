---
title: 创建服务器角色 | Microsoft Docs
description: 使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中创建服务器角色。 查看限制、局限性和必要权限。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.SERVERROLE.GENERAL.F1
- sql13.swb.serverrole.memberships.f1
- sql13.swb.serverrole.members.f1
helpviewer_keywords:
- SERVER ROLE, creating
ms.assetid: 74f19992-8082-4ed7-92a1-04fe676ee82d
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 687d5259c3181d7f82cb16dd35bf019ab1631b1a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250763"
---
# <a name="create-a-server-role"></a>创建服务器角色
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建新的服务器角色。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要创建新的服务器角色，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
 不能向服务器角色授予对数据库级安全对象的权限。 若要创建数据库角色，请参阅 [CREATE ROLE (Transact-SQL)](../../../t-sql/statements/create-role-transact-sql.md)。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
  
-   要求具有 CREATE SERVER ROLE 权限，或者 sysadmin 固定服务器角色中的成员身份。  
  
-   还需要针对登录名的 *server_principal* 的 IMPERSONATE 权限、针对用作 *server_principal* 的服务器角色的 ALTER 权限或用作 server_principal 的 Windows 组的成员身份。  
  
-   使用 AUTHORIZATION 选项分配服务器角色所有权时，还需要具有下列权限：  
  
    -   若要将服务器角色的所有权分配给另一个登录名，则需要对该登录名具有 IMPERSONATE 权限。  
  
    -   若要将服务器角色的所有权分配给另一个服务器角色，则需要具有被分配服务器角色的成员身份或对该服务器角色具有 ALTER 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-new-server-role"></a>创建新的服务器角色  
  
1.  在“对象资源管理器”中，展开要创建新的服务器角色的服务器。  
  
2.  展开 **“安全性”** 文件夹。  
  
3.  右键单击“服务器角色”文件夹，然后选择“新建服务器角色…” 。  
  
4.  在“新建服务器角色 - server\_role\_name”对话框的“常规”页上，在“服务器角色名称”框中输入新的服务器角色的名称 。  
  
5.  在 **“所有者”** 框中，输入拥有新角色的服务器主体的名称。 或者，单击省略号 (…) 打开“选择服务器登录名或角色”对话框 。  
  
6.  在“安全对象”下，选择一个或多个服务器级别的安全对象。 当选择安全对象时，可以向此服务器角色授予或拒绝针对该安全对象的权限。  
  
7.  在“权限: 显式”框中，选中相应的复选框以针对选定的安全对象授予、授予再授予或拒绝此服务器角色的权限。 如果某个权限无法针对所有选定的安全对象进行授予或拒绝，则该权限将表示为部分选择。  
  
8.  在 **“成员”** 页上，使用 **“添加”** 按钮将代表个人或组的登录名添加到新的服务器角色。  
  
9. 用户定义的服务器角色可以是另一个服务器角色的成员。 在“成员身份”页上，选中一个复选框以使当前用户定义的服务器角色成为所选服务器角色的成员。  
  
10. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-new-server-role"></a>创建新的服务器角色  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    --Creates the server role auditors that is owned the securityadmin fixed server role.  
    USE master;  
    CREATE SERVER ROLE auditors AUTHORIZATION securityadmin;  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE SERVER ROLE (Transact-SQL)](../../../t-sql/statements/create-server-role-transact-sql.md)。  
  
  
