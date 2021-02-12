---
title: 导入到数据库项目
description: 了解如何从实时数据库、数据层应用程序和脚本将对象导入到数据库项目中。 了解如何导入加密的对象。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLPROJECTIMPORTSNAPSHOTSUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.SQLPROJECTIMPORTDATABASESUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.IMPORTSCRIPTWIZARD.SUMMARY
ms.assetid: d0a0a394-6cb6-416a-a25f-9babf8ba294a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0820f680382d7a126d2837c653ab7870defeb0ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017927"
---
# <a name="import-into-a-database-project"></a>导入到数据库项目

可以使用“导入”功能，从活动的数据库或 .dacpac 把新对象填充到项目，或使用脚本中的新定义更新您项目中的现有对象。 请注意这三种途径存在一些行为差异，下面将进行说明。  
  
**“导入”菜单**  
  
![SSDT“导入”菜单](../ssdt/media/ssdt-import.gif "SSDT“导入”菜单")  
  
**本主题的内容**  
  
[导入源：数据库或数据层应用程序 (*.dacpac)](#bkmk_import_source_db)  
  
[导入源：脚本 (*.sql)](#bkmk_import_source_script)  
  
[导入加密的对象](#bkmk_import_encrypted)  
  
## <a name="import-source-database-or-data-tier-application-dacpac"></a><a name="bkmk_import_source_db"></a>导入源：数据库或数据层应用程序 (*.dacpac)  
仅当项目中没有定义任何架构对象时，才能从数据库或 .dacpac 文件导入架构。 这不包括 RefactorLog 或预先部署/后期部署脚本。  
  
导入时，将使用 SSDT 用于新对象的组织默认值通过脚本将对象定义写入项目文件，这些默认值是：顶层对象的新文件、与父级相同的文件中定义的层次结构子级、适用的内联对象中定义的表/列约束。 如果获得每个对象的更具针对性的可见性和控制，请使用“架构比较”而非“导入”。  
  
如果导入源包含预先部署和后期部署脚本、RefactorLog 或 SQLCMD 变量定义，它们将导入项目。 如果该项目已包含这些项目中的任何一个，则导入的文件将添加到项目中的“导入时忽略”文件夹中。  
  
**“导入时忽略”文件夹**  
  
![SSDT“导入时忽略”文件夹](../ssdt/media/ssdt-ignoredonimport.gif "SSDT“导入时忽略”文件夹")  
  
## <a name="import-source-script-sql"></a><a name="bkmk_import_source_script"></a>导入源：脚本 (*.sql)  
将添加项目中不存在的导入源的所有对象，而对于导入源中项目已有的所有对象将覆盖项目中的对象定义。  
  
> [!NOTE]  
> 此途径中有两个已知 bug 将在将来的版本中修复：  
>   
> -   如果表/列约束是在项目的表定义中 CREATE TABLE 语句之外定义，则导入操作将覆盖表定义以便约束内联。 但是，它将离开行约束之外，从而导致项目中的约束重复。  
> -   项目中已有的源脚本中的任意主密钥或数据库加密密钥将在导入时复制。 删除重复项以便生成项目。  
  
“从脚本导入”过程将不包含预先部署/后期部署脚本、SQLCMD 变量或 RefactorLog 文件。 这些项以及在导入时检测到的任何其他不支持的构造将放入你项目中“脚本”文件夹的 ScriptsIgnoredOnImport.sql 文件。  
  
 
## <a name="import-encrypted-objects"></a><a name="bkmk_import_encrypted"></a>导入加密的对象  
将加密的对象导入数据库项目时，无法始终从服务器检索对象定义的完整主体。 这样在处理此类对象时，导入行为可能不同。  
  
无法检索完整主体定义时，将对对象页眉/页脚以及虚主体编写脚本。 如果源为活动的数据库或从数据库提取的 .dacpac，您导入或使用“架构比较”功能时可能遇到此行为。  
  
**具有虚主体的脚本**  
  
![具有虚主体的脚本](../ssdt/media/ssdt-procwithencryption.gif "具有虚主体的脚本")  
  
如果提供完整对象定义且可以检索，则“导入”和“架构比较”会将其作为一个整体引入项目。 更新脚本、从数据库项目生成的 .dacpac 或另一数据库项目中的项目时，会出现这种情况。  
  
