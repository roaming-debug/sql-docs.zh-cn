---
title: 数据库属性（“文件”页）| Microsoft Docs
description: 了解如何查看或修改数据库属性以及如何在 SQL Server Management Studio 中创建新数据库。
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.files.f1
ms.assetid: 3c030e51-db82-4b43-b1e5-8547ddd3de87
author: stevestein
ms.author: sstein
ms.openlocfilehash: 3a8068945e91b56537a16ddf87cebfc9aa27fb3c
ms.sourcegitcommit: c09ef164007879a904a376eb508004985ba06cf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104890639"
---
# <a name="database-properties-files-page"></a>数据库属性（“文件”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此页可以创建新的数据库，也可以查看或修改所选数据库的属性。 对于现有数据库，本主题适用于 **数据库属性（“文件”页）** ；另外，本主题还适用于 **新建数据库（“常规”页）** 。  
  
## <a name="ui-element-list"></a>UI 元素列表  
 **数据库名称**  
 添加或显示数据库的名称。  
  
 **所有者**  
 通过从列表中进行选择来指定数据库的所有者。  
  
 **使用全文索引**  
 由于全文检索在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中始终处于启用状态，因此该复选框处于选中状态并被禁用。 有关详细信息，请参阅 [全文搜索](../../relational-databases/search/full-text-search.md)。  
  
 **数据库文件**  
 添加、查看、修改或移除相关联数据库的数据库文件。 数据库文件具有以下属性：  
  
 **逻辑名称**  
 输入或修改文件的名称。  
  
 **文件类型**  
 从列表中选择文件类型。 文件类型可以为 **“数据”** 、 **“日志”** 或 **“Filestream 数据”** 。 您无法修改现有文件的文件类型。  
  
 如果要将文件（容器）添加到内存优化文件组，请选择“Filestream 数据”。  
  
 要将文件（容器）添加到 Filestream 数据文件组，必须启用 FILESTREAM。 可以通过 [服务器属性（“高级”页）](../../database-engine/configure-windows/server-properties-advanced-page.md) 对话框启用 FILESTREAM。  
  
 **文件组**  
 从列表中为文件选择文件组。 默认情况下，文件组为 PRIMARY。 通过选择 \<new filegroup>，然后在“新建文件组”对话框中输入有关文件组的信息，可以创建新的文件组。 您也可以在 **“文件组”** 页上创建新的文件组。 您无法修改现有文件的文件组。  
  
 将文件（容器）添加到内存优化文件组时，“文件组”字段将填充数据库的内存优化文件组的名称。  
  
 **初始大小**  
 输入或修改文件的初始大小 (MB)。 默认情况下，这是 **model** 数据库的值。  
  
 此字段对于 FILESTREAM 文件无效。  
  
 对于内存优化文件组中的文件，此字段不能修改。  
  
 **自动增长**  
 选择或显示文件的自动增长属性。 这些属性控制在达到文件的最大文件大小时文件的扩展方式。 若要编辑自动增长值，请单击所需文件的自动增长属性旁的编辑按钮，然后更改 **“更改自动增长设置”** 对话框中的值。 默认情况下，它们是 **model** 数据库的值。  
  
 此字段对于 FILESTREAM 文件无效。  
  
 对于内存优化文件组中的文件，此字段应该是 **无限制**。  
  
 **路径**  
 显示所选文件的路径。 若要指定新文件的路径，请单击文件路径旁的编辑按钮，再导航到目标文件夹。 您无法修改现有文件的路径。  
  
 对于 FILESTREAM 文件，该路径是一个文件夹。 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 将在此文件夹中创建基础文件。  
  
 **文件名**  
 显示文件的名称。  
  
 此字段对于 FILESTREAM 文件（包括内存优化文件组中的文件）无效。  
  
 **添加**  
 将新文件添加到数据库。  
  
 **删除**  
 从数据库中删除所选文件。 除非文件为空，否则无法移除文件。 无法移除主数据文件和日志文件。  
  
 有关文件的信息，请参阅 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
  
