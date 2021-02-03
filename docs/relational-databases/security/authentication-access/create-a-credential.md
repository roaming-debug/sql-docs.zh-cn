---
title: 创建凭据 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中创建凭据。 了解如何在具有限制和局限的情况下工作。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- credentials [SQL Server], creating
- authentication [SQL Server], credentials
- logins [SQL Server], credentials
ms.assetid: c1e77e91-2a69-40d9-b8b3-97cffc710586
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 98fddc3b614cd53ad88d761365243bb81a3eca10
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250307"
---
# <a name="create-a-credential"></a>创建凭据
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  本主题说明如何使用 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中创建凭据。  
  
 凭据是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证用户在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]外部的身份标识。 主要用于执行具有 EXTERNAL_ACCESS 权限集的程序集中的代码。 当 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证用户需要访问域资源（例如存储备份的文件位置）时，也可以使用凭据。  
  
 可以将一个凭据同时映射到多个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名。 一个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名一次只能映射到一个凭据。 在创建凭据之后，可以使用“登录属性”（“常规”页）将登录名映射到凭据。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [限制和局限](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要创建凭据，可使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制和局限  
  
-   如果该提供程序没有任何登录名映射的凭据，则使用映射到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务帐户的凭据。  
  
-   一个登录名可以有多个映射的凭据，只要它们用于不同的提供程序即可。 每个登录名的每个提供程序只能有一个映射的凭据。 相同的凭据可以映射到其他登录名。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 必须具有 ALTER ANY CREDENTIAL 权限才能创建或修改凭据，并且必须具有 ALTER ANY LOGIN 权限才能将登录名映射到凭据。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-credential"></a>创建凭据  
  
1.  在对象资源管理器中，展开“安全性”  文件夹。  
  
2.  右键单击“凭据”文件夹，然后选择“新建凭据…” 。  
  
3.  在 **“新建凭据”** 对话框中的 **“凭据名称”** 框中，键入凭据的名称。  
  
4.  在“标识”框中，键入用于传出连接的帐户名称（在离开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的上下文时）。 通常为 Windows 用户帐户，但标识可以为其他类型的帐户。  
  
     或者，单击省略号“(…)”打开“选择用户或组”对话框 。  
  
5.  在 **“密码”** 和 **“确认密码”** 框中，键入 **“标识”** 框中指定的帐户的密码。 如果 **“标识”** 为 Windows 用户帐户，则密码为 Windows 密码。 如果不需要密码， **“密码”** 可为空。  
  
6.  选择“使用加密提供程序”将凭据设置为由可扩展的密钥管理 (EKM) 提供程序验证。 有关详细信息，请参阅[可扩展的密钥管理 (EKM)](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
7.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-credential"></a>创建凭据  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的实例。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例复制并粘贴到查询窗口中，然后单击“执行” 。  
  
    ```  
    -- Creates the credential called "AlterEgo.".   
    -- The credential contains the Windows user "Mary5" and a password.  
    CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',   
        SECRET = '<EnterStrongPasswordHere>';  
    GO  
    ```  
  
 有关详细信息，请参阅 [CREATE CREDENTIAL (Transact-SQL)](../../../t-sql/statements/create-credential-transact-sql.md)。  
  
  
