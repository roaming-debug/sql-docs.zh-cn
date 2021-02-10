---
description: 从数据库刷新 (OracleToSQL)
title: 从数据库刷新 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 84492f44-c368-4c75-954d-7307a2d2bbc0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ecf7edd43af59f2d388f83b118ad8b6a1583b0be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067642"
---
# <a name="refresh-from-database-oracletosql"></a>从数据库刷新 (OracleToSQL)
**通过 "从数据库刷新**" 对话框，您可以从 Oracle 数据库中选择要刷新的对象。 对话框中的行是根据元数据的状态进行颜色编码的：  
  
-   如果在本地和 Oracle 数据库中更改了对象元数据，则行为蓝色。  
  
-   如果对象元数据在 Oracle 数据库中发生更改，但在 SSMA 中不存在，则行为黄色。  
  
-   如果对象元数据已在本地更改，但在 Oracle 数据库中不存在，则该行为绿色。  
  
-   如果该对象是 Oracle 数据库中的新对象，则该行为粉红色。  
  
您可以在 " **项目设置** " 对话框中指定默认对象刷新设置。 有关详细信息，请参阅 [项目设置&#40;同步&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-synchronization-oracletosql.md)。  
  
若要访问 **从数据库刷新** 对话框，请在 Oracle 元数据资源管理器中右键单击对象，然后单击 " **从数据库刷新**"。  
  
## <a name="options"></a>选项  
**折叠 (-)**  
折叠所有对象组以隐藏单个对象。  
  
**展开 (+)**  
展开 "所有对象组" 以显示单个对象。  
  
**隐藏/显示相等对象**  
如果在 Oracle 数据库和 SSMA 中，对象元数据相同，则隐藏列表中的对象。  
  
**从数据库刷新 (箭头按钮)**  
使用箭头按钮指定应在 SSMA 中更新所选对象的元数据。  
  
**不从数据库刷新 (X 按钮)**  
使用 X 按钮指定不应在 SSMA 中更新所选对象的元数据。  
  
**图例**  
显示 " **图例** " 对话框。 图例包含行颜色和元数据状态之间的映射。  
  
若要使 " **图例** " 对话框位于 " **从数据库刷新** " 对话框的顶部，请选中 " **顶部显示** " 复选框。  
  
