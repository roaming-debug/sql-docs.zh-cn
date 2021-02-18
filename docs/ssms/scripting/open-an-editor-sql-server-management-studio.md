---
title: 打开编辑器 (SQL Server Management Studio)
description: 了解如何在 SQL Server Management Studio 中打开数据库引擎查询、MDX、DMX 和 XML/A 编辑器。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 5d654a60-d205-49d2-a831-b3d986d60024
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce81ec69f717f10c705b0d4e7fc1f5b1c6d6ea70
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354590"
---
# <a name="open-an-editor-sql-server-management-studio"></a>打开编辑器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本主题介绍如何在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]查询、MDX、DMX 或 XML/A 编辑器。 打开后，每个编辑器窗口都在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]的中心窗格中显示为一个选项卡。  
  
## <a name="before-you-begin"></a>开始之前  
 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 支持四种编辑器：用于编辑 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 脚本的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询编辑器、用于编辑使用相应语言的脚本的 DMX 和 MDX 编辑器、以及用于编辑 XML/A 脚本或 XML 文件的 XML/A 编辑器。 任何编辑器都还可以用于编辑文本文件。  
  
### <a name="limitations-and-restrictions"></a>限制和局限  
 如果与其他站点上使用非重复代码页的用户共享文件，则应使用相应的 Unicode 代码页保存文件，以避免读取该文件时出错。 此外，在保存用于 UNIX 或 Macintosh 的文件时，请确保使用相应的文档格式来保存文件。 在 **“文件”** 菜单上，单击 **“另存为”**，再单击 **“保存”** 按钮旁边向下箭头中的 **“编码保存”** ，然后在 **“行尾”** 下选择 **Unix** 或 **Macintosh**。  
  
### <a name="permissions"></a>权限  
 在代码编辑器中执行的操作会受到为您用于登录的身份验证帐户所授予的权限的约束。 例如，如果您使用 Windows 身份验证打开一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口，则无法执行引用您的 Windows 登录帐户无权访问的对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
## <a name="how-to-open-editors"></a>如何：打开编辑器  
 本节介绍如何在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开各种编辑器。  
  
### <a name="using-the-filenew-menu"></a>使用“文件”/“新建”菜单  
 在 **“文件”** 菜单上，单击 **“新建”** ，然后选择一个查询编辑器选项：  
  
-   **使用当前连接查询** - 在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中打开一个类型与当前连接相关联的新的编辑器窗口。 该编辑器窗口使用与当前连接相同的身份验证信息。 例如，如果您在对象资源管理器中选择一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，然后使用 **“使用当前连接查询”** ， [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 将使用相同的身份验证信息打开一个连接到同一实例的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器。  
  
-   **数据库引擎查询** - 打开一个新的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例所需的信息。  
  
-   **Analysis Services MDX 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] MDX 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
-   **Analysis Services DMX 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] DMX 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
-   **Analysis Services XML/A 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] XML/A 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
### <a name="using-the-fileopen-menu"></a>使用“文件”/“打开”菜单  
 在 **“文件”** 菜单上，单击 **“打开”** ，然后浏览到一个文件并打开它。 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 根据文件扩展名打开相应类型的编辑器，将该文件的内容复制到编辑器窗口中，如果需要，还会打开一个连接对话框。 例如，如果打开了一个扩展名为 .sql 的文件， [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 将打开一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口，并将该 .sql 文件的内容复制到其中，然后打开一个连接对话框。 如果您打开的文件的扩展名不与特定编辑器相关联， [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 将打开一个文本编辑器窗口并将该文件的内容复制到其中。  
  
 有关详细信息，请参阅 [将文件扩展名与代码编辑器关联](./associate-file-extensions-to-a-code-editor.md)。  
  
### <a name="using-the-toolbar"></a>使用工具栏  
 在 **“标准”** 工具栏上，单击下列按钮之一：  
  
-   **新建查询** - 在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中打开一个类型与当前连接相关联的新编辑器窗口。 该编辑器窗口使用与当前连接相同的身份验证信息。 例如，如果您在对象资源管理器中选择一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，然后单击 **“新建查询”** 按钮， [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 将使用相同的身份验证信息打开一个连接到同一实例的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器。  
  
-   **数据库引擎查询** - 打开一个新的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例所需的信息。  
  
-   **Analysis Services MDX 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] MDX 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
-   **Analysis Services DMX 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] DMX 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
-   **Analysis Services XML/A 查询** - 打开一个新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] XML/A 查询编辑器和一个对话框，以获取连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例所需的信息。  
  
### <a name="using-object-explorer"></a>使用对象资源管理器  
 在 **“对象资源管理器”** 中：  
  
-   右键单击连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的服务器节点，然后选择“新建查询”。 这将打开一个连接到同一 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口，并将该窗口的数据库上下设置为登录帐户的默认数据库。  
  
-   右键单击一个数据库节点，然后选择“新建查询”。 这将打开一个连接到同一 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口，并将该窗口的数据库上下文设置为同一数据库。  
  
### <a name="using-solution-explorer"></a>使用解决方案资源管理器  
 在“解决方案资源管理器”中，展开一个文件夹，右键单击该文件夹中的某一项，再单击“打开”，也可双击该项或文件。  
  
### <a name="using-template-browser-to-open-the-database-engine-query-editor"></a>使用模板浏览器打开数据库引擎查询编辑器  
  
-   在 **“视图”** 菜单上，单击 **“模板资源管理器”** 。  
  
-   右窗格中将出现 **“模板浏览器”** 窗口。  
  
-   双击一个模板以打开“数据库引擎查询”窗口，窗口中显示该模板的文本。 例如，若要打开 CREATE DATABASE 模板，请打开“SQL Server 模板”文件夹，再打开“数据库”文件夹，然后双击“创建数据库”。