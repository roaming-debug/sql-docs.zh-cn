---
description: 创建新的已注册的服务器 (SQL Server Management Studio)
title: 新建已注册的服务器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.general.sqlce.f1
- sql13.swb.registerserver.general.sqlserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], creating new registered servers
ms.assetid: 716ea070-a3b5-4514-9de2-82ce8a96514b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4c158ff93e5f5e62f8cb3d3d68333c5f48fd6338
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350544"
---
# <a name="create-a-new-registered-server-sql-server-management-studio"></a>创建新的已注册的服务器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题介绍如何通过在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]的已注册服务器组件中注册服务器，以保存经常访问的服务器的连接信息。 您可以在连接之前注册服务器，也可以在从对象资源管理器中进行连接时注册服务器。 对象资源管理器中有注册本地计算机上的服务器实例的专用菜单选项。  
  
 共有两种类型的已注册服务器：  
  
-   本地服务器组  
  
     可以使用本地服务器组方便地连接到经常管理的服务器。 本地服务器和非本地服务器都是在本地服务器组中注册的。 对于每个用户来说，本地服务器组是唯一的。 有关如何共享已注册的服务器信息，请参阅 [导出已注册服务器信息 (SQL Server Management Studio)](./export-registered-server-information-sql-server-management-studio.md) 和 [导入已注册的服务器信息 (SQL Server Management Studio)](./import-registered-server-information-sql-server-management-studio.md)的已注册服务器组件中注册服务器，以保存经常访问的服务器的连接信息。  
  
    > [!NOTE]  
    >  建议您尽量使用 Windows 身份验证。  
  
-   中央管理服务器  
  
     中央管理服务器将服务器注册信息存储在中央管理服务器中，而不是存储在文件系统中。 只能使用 Windows 身份验证来注册中央管理服务器和已注册的从属服务器。 中央管理服务器注册完毕后，与其关联的已注册服务器将自动显示出来。 有关详细信息，请参阅 [使用中央管理服务器管理多台服务器](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)。 不能将早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 指定为中央管理服务器。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-automatically-register-the-local-server-instances"></a>自动注册本地服务器实例  
  
-   在“已注册的服务器”中，右键单击已注册的服务器树中的任意节点，再单击“更新本地服务器注册”  。  
  
#### <a name="to-create-a-new-registered-server"></a>创建新的已注册的服务器  
  
1.  如果已注册的服务器在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中没有出现，请在 **“视图”** 菜单上，单击 **“已注册的服务器”** 。  
  
     **服务器类型**  
     从“已注册的服务器”中注册某服务器时，“服务器类型”  框是只读的，它与“已注册的服务器”窗格中显示的服务器类型相匹配。 若要注册其他类型的服务器，请在开始注册新服务器之前，在 **“已注册的服务器”** 工具栏上依次单击 **“数据库引擎”** 、 **“分析服务器”** 、 **Reporting Services** 或 **Integration Services** 。  
  
     **服务器名称**  
     按以下格式选择要注册的服务器实例： *\<servername>* [\\ *\<instancename>* ]。  
  
     **身份验证**  
     在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时，可以使用两种身份验证模式。  
  
     **Windows 身份验证**  
     Windows 身份验证模式允许用户通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户帐户进行连接。  
  
     **SQL Server 身份验证**  
     当用户使用指定的登录名和密码通过不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将自行进行身份验证，即检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码相匹配。 如果未设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户，则身份验证失败，并且用户会收到一条错误消息。  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] 有关详细信息，请参阅 [选择身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)。  
  
     **用户名**  
     显示当前连接所使用的用户名。 只有在已选择使用 Windows 身份验证进行连接的情况下，此只读选项才可用。 若要更改 **“用户名”** ，请以其他用户身份登录计算机。  
  
     **登录**  
     输入连接所用的登录名。 只有选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接时，此选项才可用。  
  
     **密码**  
     输入登录名的密码。 只有在已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接的情况下，此选项才是可编辑的。  
  
     **记住密码**  
     选择此选项，可以让 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对输入的密码进行加密并存储。 只有在已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接的情况下，才会显示此选项。  
  
    > [!NOTE]  
    >  如果已经存储了此密码，而现在要放弃存储，请清除此复选框，再单击“保存”  。  
  
     **已注册的服务器名称**  
     希望在“已注册的服务器”中显示的名称。 此名称不必与 **“服务器名称”** 框中的内容匹配。  
  
     **已注册的服务器说明**  
     输入服务器的说明（可选）。  
  
     **Test**  
     单击此项可测试与“服务器名称”  中所选服务器的连接。  
  
     **保存**  
     单击此项可保存已注册服务器的设置。  
  
## <a name="multiserver-queries"></a>多服务器查询  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的查询编辑器窗口可以同时连接到多个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例并对其进行查询。 可以将查询返回的结果合并到单个结果窗格中，也可以在单独结果窗格中返回这些结果。 查询编辑器可以选择包含一些列（提供生成每个行的服务器名称）以及登录名（用于连接到提供每个行的服务器）。 有关如何执行多服务器查询的详细信息，请参阅[同时对多个服务器执行语句 (SQL Server Management Studio)](./execute-statements-against-multiple-servers-simultaneously.md)。  
  
 若要对本地服务器组中的所有服务器执行查询，请右键单击该服务器组，指向并单击“连接”  ，然后单击“新建查询”  。 在新的查询编辑器窗口中执行查询时，将利用存储的连接信息（包括用户身份验证上下文）对该组中的全部服务器进行查询。 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证注册服务器但未保存密码，服务器将无法进行连接。  
  
 若要对注册到中央管理服务器的所有服务器执行查询，请展开此中央管理服务器，右键单击服务器组，指向并单击“连接”  ，然后单击“新建查询”  。 在新的查询编辑器窗口中执行查询时，将使用存储的连接信息以及用户的 Windows 身份验证上下文对服务器组中的所有服务器执行查询。  
  
## <a name="see-also"></a>另请参阅  
 [在对象资源管理器中隐藏系统对象](../object/hide-system-objects-in-object-explorer.md)   
 [导出已注册服务器信息 (SQL Server Management Studio)](./export-registered-server-information-sql-server-management-studio.md)   
 [导入已注册的服务器信息 (SQL Server Management Studio)](./import-registered-server-information-sql-server-management-studio.md)  
  
