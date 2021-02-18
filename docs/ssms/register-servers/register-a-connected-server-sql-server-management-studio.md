---
description: 注册连接的服务器 (SQL Server Management Studio)
title: 注册连接的服务器
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], register connected servers
- connected server registrations [SQL Server]
ms.assetid: 77deb5f5-0f80-484f-8b8b-29afa67ec18f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/28/2016
ms.openlocfilehash: 4a5780c0d945f3ac44c6936bdc2a2d9a4e7f9fe5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350485"
---
# <a name="register-a-connected-server-sql-server-management-studio"></a>注册连接的服务器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题说明如何使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] (SSMS) 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中注册已连接的服务器。 通过注册服务器，您可以保存经常访问的服务器的连接信息。 可以在连接前注册服务器，也可以在通过对象资源管理器进行连接时注册服务器。  可以通过从菜单导航到“视图”\\“已注册的服务器，在 SSMS 中查看已注册的服务器。
  
 **本主题内容**  
  
-   **若要注册服务器，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-register-a-connected-server"></a>注册连接的服务器  
  
在对象资源管理器中，右键单击已经连接的服务器，然后单击“注册”。
  
服务器名称  
此字段默认为你连接到的服务器名称。  可以选择输入服务器名称或从下拉列表中选择一个。

**身份验证**  
在连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例时，可以使用两种身份验证模式。 

-    **Windows 身份验证**  
Windows 身份验证模式允许用户通过 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户帐户进行连接。 

-    **SQL Server 身份验证**   
当用户使用指定的登录名和密码通过不可信连接进行连接时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将自行进行身份验证，即检查是否已设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户以及指定的密码是否与以前记录的密码相匹配。 如果未设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户，则身份验证失败，并且用户会收到一条错误消息。

     > [!IMPORTANT]  
     > [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] 有关详细信息，请参阅 [选择身份验证模式](../../relational-databases/security/choose-an-authentication-mode.md)。  

     -    **用户名**  
显示当前连接所使用的用户名。 只有在已选择使用 Windows 身份验证进行连接的情况下，此只读选项才可用。 若要更改 **“用户名”** ，请以其他用户身份登录计算机。 

     -    **登录**  
输入连接所用的登录名。 只有选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接时，此选项才可用。  

     -    **密码**  
输入登录名的密码。 只有在已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接的情况下，此选项才是可编辑的。 

     -    **记住密码**  
选择此选项，可以让 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对输入的密码进行加密并存储。 只有在已选择使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接的情况下，才会显示此选项。  

          > [!NOTE]  
          > 如果已经存储了此密码，而现在要放弃存储，请清除此复选框，再单击“保存”  。  

**已注册的服务器名称**  
希望在“已注册的服务器”中显示的名称。 此名称不必与 **“服务器名称”** 框中的内容匹配。  
  
**已注册的服务器说明**  
输入服务器的说明（可选）。  
  
**Test**  
单击此项可测试与“服务器名称”  中所选服务器的连接。  
  
**保存**  
单击此项可保存已注册服务器的设置。 

## <a name="see-also"></a>另请参阅

[创建新的已注册的服务器 (SQL Server Management Studio)](./create-a-new-registered-server-sql-server-management-studio.md)