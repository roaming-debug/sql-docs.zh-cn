---
title: 使用结果列表搜索文档
description: 了解如何在项目、解决方案或文件系统文件夹中包含的所有文件中搜索和替换文本。 匹配项显示在“查找结果 1”窗口中，可以双击匹配项以查看包含该项的行。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], result lists
- result list searches [SQL Server Management Studio]
- searches [SQL Server Management Studio], multiple files
- Query Editor [SQL Server Management Studio], search multiple files
ms.assetid: 275e1b6c-fbd0-4408-af77-35903f90657c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 79bd3ff3e7998c92e1c3f6474480cb36c352a98a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354584"
---
# <a name="search-documents-using-results-lists"></a>使用结果列表搜索文档
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  使用 **“查找和替换”** 对话框，可以搜索和替换项目或解决方案中或文件系统文件夹中的所有文件中的文本（即使这些文件未在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中打开）。 使用 **“查找和替换”** 对话框执行的搜索返回的匹配项将显示在“查找结果 1”和“查找结果 2”窗口中。这样，您可以查看包含匹配项的行中的完全匹配文本。  
  
### <a name="to-search-in-multiple-files"></a>在多个文件中搜索  
  
1.  在 **“编辑”** 菜单上，指向 **“查找和替换”** ，再单击 **“在文件中查找”** 。  
  
2.  在 **“查找内容”** 文本框中，输入要搜索的文本。  
  
3.  在 **“查找范围”** 列表中，单击 **“所有打开的文档”** 、 **“当前项目”** 、 **“整个解决方案”** ，或者键入目录路径。  
  
4.  在 **“文件类型”** 列表中，选择一个列出的文件扩展名组，或输入要搜索的文件类型的扩展名（由分号分隔）。 使用 \*.\* 可以搜索在“查找范围”下拉列表中列出的目录中的所有文件。  
  
5.  选择其余搜索选项以提高搜索的准确性。  
  
6.  单击 **“查找”** 以开始执行搜索。  
  
 在默认情况下，搜索匹配项将显示在“查找结果 1”窗口中。 可以通过双击“查找结果 1”窗口中的每个条目浏览搜索匹配项。 若要在新窗口中查看搜索结果，请选择 **“在查找 2 中显示”** 。  
  
#### <a name="to-replace-across-multiple-files-or-folders"></a>在多个文件或文件夹中替换  
  
1.  在 **“编辑”** 菜单上，指向 **“查找和替换”** ，再单击 **“在文件中替换”** 。  
  
2.  在 **“查找内容”** 文本框中，输入要搜索的文本。  
  
3.  在 **“替换为”** 框中，输入用来替换搜索文本的文本。  
  
4.  在 **“查找范围”** 列表中，单击 **“所有打开的文档”** 、 **“当前项目”** 、 **“整个解决方案”** ，或者键入目录路径。  
  
5.  单击 **“替换”** 以使用 **“替换为”** 框中的文本替换当前的搜索匹配项。 可以通过单击 **“查找下一个”** 跳过单个匹配项，或通过单击 **“跳过文件”** 跳过整个文件。  
  
     \- 或 -  
  
     选择 **“全部替换”** 以使用 **“替换为”** 框中的文本替换所有匹配项。 若要在其他时间撤消某些替换，请选择 **“在全部替换后使已修改文件保持打开状态”** 。  
  
    > [!NOTE]  
    >  如果选择 **“全部替换”** ，则系统将替换所有搜索匹配项，包括已使用 **“跳过文件”** 或 **“查找下一个”** 跳过的那些搜索匹配项。 只能针对在替换操作完成后仍保持打开状态的文件中进行的替换使用 **“撤消”** 操作。  
  
 默认情况下，替换信息将显示在“查找结果 1”窗口中。 可以通过双击“查找结果 1”窗口中的各个条目来浏览替换项。  
  
## <a name="see-also"></a>另请参阅  
 [搜索和替换](./search-and-replace.md)   
 [交互式搜索文档](./search-documents-interactively.md)   
 [使用通配符搜索文本](./search-text-with-wildcards.md)   
 [使用正则表达式搜索文本](./search-text-with-regular-expressions.md)  
  
