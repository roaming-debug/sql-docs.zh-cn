---
description: 创建中央管理服务器和服务器组
title: 创建中央管理服务器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- configuration server
ms.assetid: da265482-3953-440a-ac23-0ab7e42a55eb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 843411069a27d658cf9e2b157170d8ca0f14c91c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350550"
---
# <a name="create-a-central-management-server-and-server-group"></a>创建中央管理服务器和服务器组

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 指定一个 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]实例作为中央管理服务器。 中央管理服务器存储组织到一个或多个中央管理服务器组中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例列表。 使用中央管理服务器组执行的操作将作用于服务器组中的所有服务器。 这包括使用对象资源管理器连接到服务器以及在多个服务器上同时执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句和基于策略的管理策略。  
  
> [!NOTE]  
>  不能将早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 指定为中央管理服务器。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要创建中央管理服务器和服务器组，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 msdb 数据库中有两个数据库角色可授予对中央管理服务器的访问权限。 只有 ServerGroupAdministratorRole 角色的成员能够管理中央管理服务器。 若要连接到中央管理服务器，需要具有 ServerGroupReaderRole 角色的成员身份。  
  
 由于中央管理服务器维护的连接是在用户的上下文中使用 Windows 身份验证执行的，因此对注册的服务器的有效权限可能有所不同。 例如，用户可能是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] A 实例上 sysadmin 固定服务器角色的成员，但仅具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] B 实例的有限权限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 下面的过程说明如何执行以下步骤。  
  
1.  创建中央管理服务器。  
  
2.  向中央管理服务器添加一个或多个服务器组，和向服务器组添加一个或多个已注册的服务器。  
  
#### <a name="create-a-central-management-server"></a>创建中央管理服务器  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的 **“视图”** 菜单上，单击 **“已注册的服务器”** 。  
  
2.  在“已注册的服务器”中，展开“数据库引擎”，右键单击“中央管理服务器”，然后单击“注册中央管理服务器”。  
  
3.  在“新建服务器注册”对话框中，从服务器下拉列表中选择要作为中央管理服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。 必须为此中央管理服务器使用 Windows 身份验证。  
  
4.  在 **“已注册的服务器”** 中，输入服务器名称和可选说明。  
  
5.  在“连接属性”选项卡中，查看或修改网络和连接属性。 有关详细信息，请参阅[连接到服务器（“连接属性”页）数据库引擎](../f1-help/connect-to-server-connection-properties-page-database-engine.md)。  
  
6.  单击 **“测试”**，对连接进行测试。  
  
7.  单击“ **保存**”。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将出现在 **“中央管理服务器”** 文件夹下。  
  
#### <a name="create-a-new-server-group-and-add-servers-to-the-group"></a>创建新服务器组并向该组添加服务器  
  
1.  在 **“已注册的服务器”** 中，展开 **“中央管理服务器”**。 右键单击在上述步骤中添加的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例，然后选择“新建服务器组”。  
  
2.  在 **“新建服务器组属性”** 中，输入组名和可选说明。  
  
3.  在“已注册的服务器”中，右键单击服务器组，然后单击“新建服务器注册”。  
  
4.  在新服务器注册中，选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅[创建新的已注册的服务器 (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md). 根据需要添加更多服务器。  
  
#### <a name="to-execute-queries-against-several-configuration-targets-at-the-same-time"></a>同时对多个配置目标执行查询  
  
-   在创建中央管理服务器、一个或多个服务器组以及一个或多个已注册的服务器后，您可以同时对整个组执行查询。 有关如何同时对服务器组中的服务器执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的详细信息，请参阅[同时对多个服务器执行语句 (SQL Server Management Studio)](./execute-statements-against-multiple-servers-simultaneously.md)。  
  
## <a name="see-also"></a>另请参阅  
 [使用中央管理服务器管理多台服务器](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
