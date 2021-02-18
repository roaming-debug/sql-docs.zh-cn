---
title: 配置编辑器 (SQL Server Management Studio)
description: 了解如何通过在“选项”对话框中设置选项来自定义 SQL Server Management Studio 编辑器的操作。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 33ddc2599ae030e61059c23a462fb68f9f814966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354738"
---
# <a name="configure-editors-sql-server-management-studio"></a>配置编辑器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

您可以通过为每个编辑器配置选项，自定义 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 编辑器的操作。  
  
## <a name="setting-editor-options"></a>设置编辑器选项  
 通过使用“工具”菜单，然后选择“选项…”以便显示“选项”对话框，可设置大多数编辑器选项  。 在 **“选项”** 对话框中，打开左侧窗格中的 **“文本编辑器”** 节点，以便设置代码和文本编辑选项。 “文本编辑器”下方的节点适用于特定编辑器：  
  
1.  **所有语言** - 使用此节点设置的选项将应用于所有 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 编辑器。 可通过使用其他节点为特定的编辑器设置不同选项，覆盖这些设置。  
  
2.  **纯文本** - 使用此节点设置的选项将应用于 MDX、DMX 和文本编辑器。  
  
3.  **Transact-SQL** - 使用此节点设置的选项将应用于数据库引擎查询编辑器。  
  
4.  **XML** - 使用此节点设置的选项将应用于 XML for Analysis 编辑器。  
  
 打开 **“查询执行”** 或 **“查询结果”** 节点，以便自定义查询的执行以及显示结果的方式。  
  
## <a name="editor-configuration-tasks"></a>编辑器配置任务  
  
|任务说明|主题|  
|----------------------|-----------|  
|介绍如何指定通过在 Windows 资源管理器中双击具有指定扩展名的文件来打开编辑器。|[将文件扩展名与代码编辑器关联](./associate-file-extensions-to-a-code-editor.md)|  
|介绍如何自定义字体以使代码和文本更具可读性。|[更改字体颜色、大小和样式](./change-font-color-size-and-style.md)|  
|介绍如何查看属性。|[使用 Management Studio 中的“属性”窗口](./use-the-properties-window-in-management-studio.md)|  
|编辑器选项对话框的 F1 帮助页的位置。|[“查询选项”页的 F1 帮助](../f1-help/f1-help-for-server-connections-sql-server-management-studio.md)|