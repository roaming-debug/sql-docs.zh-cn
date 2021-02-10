---
title: 准备访问数据库以迁移 (AccessToSQL) |Microsoft Docs
description: 了解如何确定要迁移到 SQL Server 或 Azure SQL 数据库的访问数据库，并确保这些数据库已准备好进行迁移。
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7c52b13a2a71afc005f19d1733e8dff435d2a582
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066582"
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>准备用于迁移的 Access 数据库 (AccessToSQL) 
在将 Access 数据库迁移到之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，必须确定要迁移的数据库，并确保这些数据库已准备好进行迁移。  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>确定何时迁移到 SQL Server  
Jet 数据库引擎是一种灵活、易于使用的数据管理解决方案，它是用于访问的数据库引擎。 然而，随着数据库变得越来越重要，许多用户发现它们需要更高的性能、安全性或可用性。 对于需要更可靠的数据平台的应用程序，请考虑将这些应用程序的基础数据库移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关确定何时迁移的详细信息，请参阅网站上的 [迁移信息页](https://go.microsoft.com/fwlink/?LinkId=68571) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
将数据库迁移到之后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可以继续使用链接表进行访问，也可以手动将应用程序迁移到与 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 直接交互的基于 .NET Framework 的代码 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="determining-which-databases-to-migrate"></a>确定要迁移的数据库  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移助手 (SSMA) 的访问权限可以找到访问数据库。 然后，你可以将有关这些数据库的元数据导出至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关如何导出和查询元数据的详细信息，请参阅 [导出访问清单](exporting-an-access-inventory-accesstosql.md)。  

   > [!NOTE]
   > 并非所有访问功能和设置都受支持，或者可以轻松地转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 在开始迁移数据库之前，请参阅 [不兼容的访问功能](incompatible-access-features-accesstosql.md)。
  
## <a name="preparing-for-migration"></a>准备迁移  
使用以下准则来帮助准备要迁移到的 Access 数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
### <a name="upgrading-older-access-databases"></a>升级旧的 Access 数据库  
SSMA for Access 支持 Access 97 及更高版本。 如果你有 Access 的早期版本中的数据库，请打开 Access 97 或更高版本中的数据库并将其保存。  
  
### <a name="removing-workgroup-protection"></a>删除工作组保护  
SSMA 无法迁移使用工作组保护的数据库。 若要从 Access 数据库中删除工作组保护，请执行以下步骤：  
  
1.  将 Access 数据库文件复制到另一个位置。  
  
2.  打开复制的数据库。  
  
3.  在 " **工具** " 菜单上，指向 " **安全性**"，然后选择 " **用户和组权限**"。  
  
4.  选择 " **用户** " 选项，选择 **管理员** 用户，然后确保选择 " **管理** " 权限。  
  
5.  选择 " **组** " 选项，选择 " **用户** " 组，然后确保选择 " **管理** " 权限。  
  
6.  单击 **"确定"**，然后在 " **文件** " 菜单上，单击 " **退出**"。  
  
你现在可以使用 SSMA 来迁移复制的数据库。 将架构加载到中后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，你可以手动保护上的数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
### <a name="backing-up-databases"></a>备份数据库  
在将 Access 数据库迁移到之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，您应该备份要迁移的 access 数据库以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 要将访问对象和数据迁移到的数据库。  
  
若要备份 Access 数据库，请在 " **工具** " 菜单上指向 " **数据库实用工具**"，然后选择 " **备份数据库**"。  
  
有关如何备份数据库的信息 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 联机丛书中的 "在中备份和还原数据库" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
### <a name="documenting-databases"></a>记录数据库  
你可能还需要记录 Access 数据库的属性，例如数据库对象的列表、文件大小和权限。 若要在 Access 中生成此文档，请在 " **工具** " 菜单上，指向 " **分析**"，然后单击 "已 **记录**"。  
  
## <a name="see-also"></a>另请参阅  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[将访问应用程序链接到 SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
