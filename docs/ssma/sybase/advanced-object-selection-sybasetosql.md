---
description: 高级对象选择 (SybaseToSQL)
title: " (SybaseToSQL) 的高级对象选择 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d2baa90f-1b77-47ce-988d-1910c7c74103
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5767d75d6f93b0d6305deb4a161261094c70beaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073331"
---
# <a name="advanced-object-selection-sybasetosql"></a>高级对象选择 (SybaseToSQL)
通过 " **高级对象部分** " 对话框，您可以使用对象名称中的字符串和子字符串筛选数据库对象，然后选择或取消选择这些对象。 SSMA 对所选对象执行转换和迁移操作。  
  
若要访问此对话框，请在元数据资源管理器中右键单击，然后选择 " **高级对象选择**"。  
  
第一次打开对话框时，请单击 " **显示子类别项** " 以显示所有已加载到项目中的元数据的对象。 然后，可以输入字符串来筛选这些项。 例如，输入字符串 "company" 可显示名称中包含该字符串的所有项目。  
  
在使用此对话框之前，您可能希望通过转换架构或保存项目来强制 SSMA 加载所有元数据。  
  
## <a name="options"></a>选项  
**检查所有项**  
在 "所有项" 旁边添加复选标记。 将在元数据资源管理器中立即选择这些项。  
  
**取消选中所有项**  
删除所有项旁边的复选标记。 这些项将立即在元数据资源管理器中清除。  
  
**列表视图模式**  
在列表中显示筛选的项。  
  
**表视图模式**  
在表中显示筛选的项。  
  
**仅显示已加载项**  
切换类别或项的显示。 选择此按钮后，SSMA 将显示与筛选条件匹配的所有项以及之前加载的项。 如果未选择此按钮，SSMA 会显示类别文件夹。  
  
**Filter**  
输入要用于筛选项的字符串。 例如，若要查找项目名称中包含字符串 "ID" 的所有可用项，请在 " **筛选器** " 框中输入字符串 "id"。  
  
如果项与筛选条件匹配，则键入字符串时将显示类别或项。 若要查看匹配项，我们建议您单击 " **仅显示已加载项** " 按钮。  
  
**清除筛选器**  
清除 **筛选** 框。  
  
