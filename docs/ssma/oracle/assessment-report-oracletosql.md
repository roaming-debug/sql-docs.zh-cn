---
description: 评估报表 (OracleToSQL)
title: 评估报表 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 168b7465-a6d6-4329-b46e-fc6c5a3f2d9d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: b4f66820e4745d16484b81a5723e03c0acad5cbe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058732"
---
# <a name="assessment-report-oracletosql"></a>评估报表 (OracleToSQL)
"评估报告" 窗口显示了将数据库对象转换为语法的结果 [!INCLUDE[tsql](../../includes/tsql-md.md)] ，还可以帮助您估算迁移项目的复杂性和成本。  
  
若要访问评估报表，请在源元数据资源管理器中选择要转换的对象，右键单击 " **架构** " 或 " **同义词**"，然后选择 " **创建报表**"。  
  
## <a name="options"></a>选项  
  
|术语|定义|  
|-|-|  
|**转换统计信息**|按语句类型显示转换统计信息。 如果在左窗格中选择了组对象（如架构）或没有代码的对象，则将显示此窗格。|  
|**按类别分类的对象**|按类别显示对象的数量。 此窗格仅在左窗格中选择了组对象（如架构）或没有代码的对象时可见。|  
|**统计信息**|显示所选对象的转换统计信息。 仅当在左窗格中选择了具有代码的单个对象时，才会显示此窗格。 你可能需要展开 " **统计信息**" （位于 **源** 窗格的紧上方）才能查看此窗格。|  
|**Source**|显示选定对象的 Oracle 代码，并突出显示未转换为的代码 [!INCLUDE[tsql](../../includes/tsql-md.md)] 。 仅当在左窗格中选择了具有代码的单个对象时，才会显示此窗格。<br /><br />单击行号以设置或清除书签。 使用窗格顶部的按钮可以在代码中导航。|  
|**Target**|显示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 所选对象的转换结果代码，以及未转换的代码的错误消息。 仅当在左窗格中选择了具有代码的单个对象时，才会显示此窗格。<br /><br />单击行号以设置或清除书签。 使用窗格顶部的按钮可以在代码中导航。|  
|**消息窗格 (Messages pane)**|显示在创建评估报告时生成的错误、警告和信息性消息。 消息按数字进行分组。 若要查看导致错误的代码，请单击 " **错误**"、" **警告**" 或 " **信息**"，展开 "消息类别"，然后单击一条消息。|  
  
