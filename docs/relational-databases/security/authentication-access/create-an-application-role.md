---
title: 创建应用程序角色 | Microsoft Docs
description: 使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中创建应用程序角色，以限制对数据库的访问（通过应用程序除外）。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.approle.general.f1
helpviewer_keywords:
- application roles [SQL Server], creating
ms.assetid: 6b8da1f5-3d8e-4f88-b111-b915788b06f1
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: df42c1651d460b0be7b0e2900338982168ebd597
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468558"
---
# <a name="create-an-application-role"></a>创建应用程序角色
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建应用程序角色。 应用程序角色可限制用户通过除特定应用程序之外的其他方式访问数据库。 应用程序角色不包含任何用户，因此，在选择 **“应用程序角色”** 时不会显示 **“角色成员”** 列表。  
  
> [!IMPORTANT]  
>  当设置应用程序角色密码时，将检查密码复杂性。 调用应用程序角色的应用程序必须存储其密码。 而且应当始终以加密的形式存储应用程序角色密码。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建应用程序角色，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要对数据库具有 ALTER ANY APPLICATION ROLE 权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
##### <a name="to-create-an-application-role"></a>创建应用程序角色  
  
1.  在“对象资源管理器”中，展开要创建应用程序角色的数据库。  
  
2.  展开 **“安全性”** 文件夹。  
  
3.  展开 **“角色”** 文件夹。  
  
4.  右键单击“应用程序角色”文件夹，然后选择“新建应用程序角色…” 。  
  
5.  在“常规”页的“应用程序角色 - 新建”对话框中，在“角色名称”框中输入新的应用程序角色的新名称  。  
  
6.  在 **“默认架构”** 框中，通过输入对象名称指定将拥有此角色创建的对象的架构。 或者，单击省略号“(…)”以打开“定位架构”对话框 。  
  
7.  在 **“密码”** 框中，输入新角色的密码。 在“确认密码”框中再次输入该密码。  
  
8.  在 **“此角色拥有的架构”** ，选择或查看此角色将拥有的架构。 架构只能由一个架构或角色拥有。  
  
9. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

### <a name="additional-options"></a>其他选项  
 “应用程序角色 - 新建”对话框还在两个其他页上提供了选项：“安全对象”和“扩展属性” 。  
  
-   **“安全对象”** 页将列出所有可能的安全对象以及可授予登录名的针对这些安全对象的权限。  
  
-   **“扩展属性”** 页允许您向数据库用户添加自定义属性。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-an-application-role"></a>创建应用程序角色  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates an application role called "weekly_receipts" that has the password "987Gbv876sPYY5m23" and "Sales" as its default schema.  
  
    CREATE APPLICATION ROLE weekly_receipts   
        WITH PASSWORD = '987G^bv876sPY)Y5m23'   
        , DEFAULT_SCHEMA = Sales;  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE APPLICATION ROLE (Transact-SQL) ](../../../t-sql/statements/create-application-role-transact-sql.md)。  
  
  
