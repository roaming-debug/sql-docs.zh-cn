---
description: " (转换的项目设置)  (AccessToSQL) "
title: " (转换的项目设置)  (AccessToSQL) |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- conversion, options described
- Project Settings dialog box, Conversion
ms.assetid: bcebc635-c638-4ddb-924c-b9ccfef86388
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7390c30cd245a3f8a46784ec12e97752173a629
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066472"
---
# <a name="project-settings-conversion-accesstosql"></a> (转换的项目设置)  (AccessToSQL) 
转换项目设置使你可以配置如何将对象从 Access 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 AZURE SQL 数据库对象。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "转换" 窗格。  
  
-   使用 " **项目设置** " 对话框可以设置当前项目的配置选项。 若要访问转换设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后选择 " **转换**"。  
  
-   使用 " **默认项目设置** " 对话框可以为所有项目设置配置选项。 若要访问转换设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，从 " **迁移目标版本** " 下拉框中选择需要查看其设置的 "迁移项目类型"，单击左侧窗格底部的 " **常规** "，然后选择 " **转换**"。  
  
## <a name="options"></a>选项  
**添加主密钥**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果访问表没有主键或唯一索引，则在或 SQL Azure 表中创建新的主键。  
  
-   **默认模式**： False  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
连接到 SQL Azure 时，默认情况下为 True。**添加时间戳列**  
指定 SSMA 是否应创建时间戳值（如果需要）。  
  
-   **默认模式**：让 SSMA 决定  
  
-   **乐观模式**：从不  
  
-   **完整模式**：让 SSMA 决定  
  
**包含带有转换评估报表的数据评估报表**  
在评估报表中包含数据评估。  
  
-   **默认模式**： True  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
**主键包含可为空的列时的消息类型**  
指定在查找具有可为 null 的列的主键时，SSMA 在 "输出" 窗格中显示的消息的类型 (警告、错误或无) 。  
  
-   **默认模式**：警告  
  
-   **乐观模式**：没有消息  
  
-   **完整模式**：错误  
  
**外键列的大小不同时的消息类型**  
指定在发现不正确的文本外键时 SSMA 显示在 "输出" 窗格中的消息类型 (警告、错误或无) 。  
  
-   **默认模式**：警告  
  
-   **乐观模式**：没有消息  
  
-   **完整模式**：错误  
  
**为备注列编制索引时的消息类型**  
指定在找到包含 **memo** 列的索引时，SSMA 在 "输出" 窗格中显示的消息的类型 (警告、错误或无) 。  
  
-   **默认模式**：警告  
  
-   **乐观模式**：没有消息  
  
-   **完整模式**：错误  
  
**当复杂查询使用通配符 (#42 时发出警告 \& )**  
在 "输出" 窗格中显示一条警告，并在 SELECT 语句中的列名为通配符 ( * ) 时错误列表。  
  
-   **默认模式**： True  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
**更改标识符名称时发出警告**  
当 SSMA 更改对象标识符名称时，在 "评估" 报表和 "输出" 窗格中显示一条消息。  
  
-   **默认模式**： True  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
**引用标识符时发出警告**  
当 SSMA 引用对象标识符名称时，将在 "评估" 报表和 "输出" 窗格中显示一条消息。 名称为关键字或包含特殊字符时，需要引用标识符。  
  
-   **默认模式**： True  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
## <a name="see-also"></a>另请参阅  
[ (访问) 的用户界面参考 ](./user-interface-reference-accesstosql.md)  
