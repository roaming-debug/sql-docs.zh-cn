---
description: '迁移向导 (AccessToSQL) '
title: 迁移向导 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration Wizard dialog box
- Migration Wizard, adding Access databases
- Migration Wizard, Connect to SQL Azure
- Migration Wizard, Connect to SQL Server
- Migration Wizard, Link Tables
- Migration Wizard, Migration status
- Migration Wizard, New Project
- Migration Wizard, Selecting objects to migrate
ms.assetid: 5bab5914-b2ae-4795-8cf5-83e42d64bef2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f5f97b55b4d357c2caa2314c993806c61f51e07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066662"
---
# <a name="migration-wizard-accesstosql"></a>迁移向导 (AccessToSQL) 
迁移向导将指导你完成将一个或多个数据库从访问权限迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 通过使用此向导，你将创建一个项目，将数据库添加到项目，选择要迁移的对象，然后连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。 你还将转换、加载和迁移访问架构和数据。 您也可以将 Access 表链接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 表。  
  
大多数迁移向导页面包含与现有 "SSMA" 对话框相同的选项。 因此，此处描述了向导页，然后提供了链接，以便您可以了解有关各个选项的详细信息。 如果页面包含唯一的选项，请参阅此处。  
  
## <a name="starting-the-migration-wizard"></a>启动迁移向导  
默认情况下，在启动 SSMA 时，将显示迁移向导。 还可以通过选择 "**迁移向导**" 在 "**文件**" 菜单中启动向导。  
  
## <a name="welcome-page"></a>欢迎页  
"欢迎" 页面介绍了迁移向导，并提供了以下用于启动向导的选项。  
  
**启动时启动此向导。**  
默认情况下，启动 SSMA 时，SSMA 将启动迁移向导。 若要阻止自动启动向导，请清除此复选框。  
  
## <a name="create-new-project-page"></a>"创建新项目" 页  
在 "创建新项目" 页上，你可以在其中输入项目文件名、位置和迁移项目类型 (用于迁移) 的目标 SQL Server 版本。 有关详细信息，请参阅 [New Project (SSMA) ](./new-project-ssma-accesstosql.md)  
  
## <a name="add-access-databases-page"></a>"添加 Access 数据库" 页  
在 "添加访问数据库" 页中，可以将一个或多个 Access 数据库添加到项目。 您可以通过单击 " **添加数据库**"，然后从 **打开** 的窗口中选择数据库来添加单个数据库。 或者，您可以使用 " **查找数据库** " 按钮查找数据库。 有关详细信息，请参阅下列主题：  
  
-   [添加和删除 Access 数据库文件](adding-and-removing-access-database-files-accesstosql.md)  
  
-   [查找数据库向导（选择位置）](./find-databases-wizard-select-locations-accesstosql.md)  
  
-   [查找数据库向导（选择文件）](./find-databases-wizard-select-files-accesstosql.md)  
  
-   [查找数据库向导（验证所选内容）](./find-databases-wizard-verify-selection-accesstosql.md)  
  
## <a name="select-objects-to-migrate-page"></a>选择要迁移的对象页  
在 "选择要迁移的对象" 页上，选择要转换的对象。 您可以选择所有对象、对象组或单个对象。  
  
**选择对象**  
  
1.  展开 " **Access-元数据库**"，然后展开 " **数据库**"。  
  
2.  执行以下一项或多项操作：  
  
    -   若要转换所有数据库，请选中 " **数据库**" 旁边的复选框。  
  
    -   若要转换或省略单个数据库，请选中或清除数据库名称旁边的复选框。  
  
    -   若要转换或省略查询，请展开数据库，然后选中或清除 " **查询** " 复选框。  
  
    -   若要转换或省略单个表，请展开数据库，展开 " **表**"，然后选中或清除表旁边的复选框。  
  
如果有多个对象，则可能要使用右窗格中的 **高级对象选择** 选项来筛选 Access 数据库对象。 例如，如果在左窗格中选择 " **表** "，则可以通过在 " **筛选器** " 框中输入字符串来筛选表的列表。 然后，可以使用窗格顶部的按钮来选择或清除要迁移的筛选的表。  
  
有关筛选的详细信息，请参阅 [高级对象选择 (SSMA Common) ](../sybase/advanced-object-selection-sybasetosql.md)中的 "选项" 部分。  
  
## <a name="connect-to-sql-server-page"></a>连接到 SQL Server 页  
在 "连接到" [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 页上，指定连接属性，然后连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [连接到 SQL Server](connect-to-sql-server-accesstosql.md)。
  
> [!IMPORTANT]  
> 一旦连接成功，您将会遇到 " **链接表** " 页，您可以在其中设置表的链接。 单击 " **下一步** " 开始迁移。  
  
## <a name="connect-to-sql-azure-page"></a>连接到 SQL Azure 页  
在 "连接到 SQL Azure" 页上，指定连接属性，然后连接到 SQL Azure。 若要创建新的 Azure 数据库，可以使用单击 "**浏览**" 按钮上显示的 "**创建 Azure 数据库**" 选项来执行此操作。 有关详细信息，请参阅 [连接到 SQL Azure](connect-to-azure-sql-db-accesstosql.md)  
  
> [!IMPORTANT]  
> 一旦连接成功，您将会遇到 " **链接表** " 页，您可以在其中设置表的链接。 单击 "链接" 页上的 " **下一步** " 按钮，开始迁移。  
  
## <a name="link-tables-page"></a>链接表页  
"链接表" 页允许您将原始访问表链接到已迁移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的表。 链接表将修改 Access 数据库，以便你的查询、窗体、报表和数据访问页使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库中的数据，而非 access 数据库中的数据。  
  
**链接表**  
选中 " **链接表** " 复选框可将 Access 表链接到已迁移 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 的表。 若要开始迁移，请单击 " **下一步** " 按钮。  
  
## <a name="migration-status-page"></a>迁移状态页  
"迁移状态" 页显示将访问架构转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 架构、将转换后的架构加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure 然后迁移数据的进度。  
  
有关此页面的详细信息，请参阅 [转换、加载和迁移](./convert-load-and-migrate-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[使用 SQL Server 迁移助手入门访问 &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[将 Access 数据库迁移到 SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[ (访问) 的用户界面参考 ](./user-interface-reference-accesstosql.md)  
