---
description: 第 2 课：配置数据库对象的权限
title: 教程：配置对数据库对象的权限
ms.custom: seo-lt-2019
ms.date: 07/31/2018
ms.prod: sql
ms.technology: t-sql
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- database permissions
ms.assetid: f964b66a-ec32-44c2-a185-6a0f173bfa22
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 15986ee3b8407a62fc4ed40a49c043921fe34588
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92035846"
---
# <a name="lesson-2-configure-permissions-on-database-objects"></a>第 2 课：配置数据库对象的权限
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
授予用户访问数据库的权限涉及三个步骤。 首先，创建登录名。 使用登录名，用户可以连接到 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]。 然后将登录名配置为指定数据库中的用户。 最后，授予该用户访问数据库对象的权限。 本课介绍了这三个步骤，并介绍了如何将视图和存储过程创建为对象。  

  >[!NOTE]
  > 本课程依赖[第 1 课 - 创建数据库对象](lesson-1-creating-database-objects.md)中创建的对象。 请在学习第 2 课之前，先完成第 1 课。 

## <a name="prerequisites"></a>先决条件
若要完成本教程，需要 SQL Server Management Studio 以及针对 SQL Server 实例的访问权限。 

- 安装 [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。

如果不能访问 SQL Server 实例，请从以下链接选择平台。 如果选择 SQL 身份验证，请使用 SQL Server 登录凭据。
- **Windows**：[下载 SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
- **macOS**：[在 Docker 上下载 SQL Server 2017](../linux/quickstart-install-connect-docker.md)。

[!INCLUDE[Freshness](../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="create-a-login"></a>创建登录名
若要访问 [!INCLUDE[ssDE](../includes/ssde-md.md)]，用户需要有登录名。 登录名可以将用户身份表示为 Windows 帐户或 Windows 组成员，登录名也可以是仅存在于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]登录名。 应该尽可能使用 Windows 身份验证。  
  
默认情况下，计算机上的管理员具有对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的完全访问权限。 在本课中，我们需要一个具有更少特权的用户；因此，您将在计算机上创建一个新的本地 Windows 身份验证帐户。 为此，您必须是计算机上的管理员。 然后您将授予该新用户访问 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的权限。  
  
### <a name="create-a-new-windows-account"></a>创建新的 Windows 帐户  
  
1.  单击“开始”后，单击“运行”，在“打开”框中，键入“%SystemRoot%\system32\compmgmt.msc /s”，再单击“确定”打开“计算机管理”程序。 
2.  在“系统工具”下，展开“本地用户和组”，右键单击“用户”，再单击“新建用户”。    
3.  在“用户名”框中，键入“Mary”。    
4.  在“密码”和“确认密码”框中，键入强密码，再单击“创建”创建新的本地 Windows 用户。  
  
### <a name="create-a-sql-login"></a>创建 SQL 登录名  

在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]的查询编辑器窗口中，键入并执行以下代码（将 `computer_name` 替换为您计算机的名称）。 `FROM WINDOWS` 表示 Windows 对用户进行身份验证。 除非连接字符串指示其他数据库，否则可选的 `DEFAULT_DATABASE` 参数将 `Mary` 连接到 `TestData` 数据库。 此语句引入了分号作为 [!INCLUDE[tsql](../includes/tsql-md.md)] 语句的可选结束符。
  
  ```sql  
  CREATE LOGIN [computer_name\Mary]  
      FROM WINDOWS  
      WITH DEFAULT_DATABASE = [TestData];  
  GO  
  ```  
  
  这将授权通过计算机的身份验证的用户名 `Mary`访问此 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例。 如果计算机上存在多个 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 实例，则必须在 `Mary` 必须访问的每个实例上创建登录名。    
  > [!NOTE]  
  > 因为 `Mary` 不是域帐户，所以此用户名只能在此计算机上进行身份验证。 


## <a name="grant-access-to-a-database"></a>授予对数据库的访问权限
现在 Mary 具有访问此 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]实例的权限，但是不具有访问数据库的权限。 在授权她作为数据库用户之前，她甚至无权访问其默认数据库 **TestData** 。  
  
若要授予 Mary 访问权限，请切换到 **TestData** 数据库，再使用 CREATE USER 语句将她的登录名映射到名为 Mary 的用户。  
  
### <a name="to-create-a-user-in-a-database"></a>在数据库中创建用户  
  
键入并执行下列语句（将 `computer_name` 替换为您计算机的名称），以授予 `Mary` 访问 `TestData` 数据库的权限。
  
 ```sql  
 USE [TestData];  
 GO  
 
 CREATE USER [Mary] FOR LOGIN [computer_name\Mary];  
 GO    
 ```  
  
 现在，对于 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 和 `TestData` 数据库，Mary 都具有访问权限。  


## <a name="create-views-and-stored-procedures"></a>创建视图和存储过程
作为管理员，可以从 **Products** 表和 **vw_Names** 视图执行 Select，以及执行 **pr_Names** 过程；但是 Mary 不能。 若要授予 Mary 必要的权限，请使用 GRANT 语句。  

### <a name="grant-permission-to-stored-procedure"></a>授予对存储过程的权限  
执行以下语句将 `Mary` 存储过程的 `EXECUTE` 权限授予 `pr_Names` 。
  
  ```sql  
  GRANT EXECUTE ON pr_Names TO Mary;  
  GO  
  ```  
  
在这种情况下，Mary 只能通过使用存储过程访问 **Products** 表。 如果您希望 Mary 能够对视图执行 SELECT 语句，则您还必须执行 `GRANT SELECT ON vw_Names TO Mary`。 若要删除对数据库对象的访问权限，请使用 REVOKE 语句。  
  
> [!NOTE]  
> 如果表、视图和存储过程不是由同一架构拥有，则授予权限将变得更复杂。  
  
### <a name="about-grant"></a>关于 GRANT  
必须具有 EXECUTE 权限才能执行存储过程。 必须具有 SELECT、INSERT、UPDATE 和 DELETE 权限才能访问和更改数据。 GRANT 语句还用于其他权限，如创建表的权限。  
  
## <a name="next-steps"></a>后续步骤
下一篇文章将介绍如何删除在其他课程中创建的数据库对象。 

转到下一篇文章，了解详细信息：
> [!div class="nextstepaction"]
>[后续步骤](lesson-3-deleting-database-objects.md)
