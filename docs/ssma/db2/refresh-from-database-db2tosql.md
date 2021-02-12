---
description: '从数据库刷新 (DB2ToSQL) '
title: 从数据库刷新 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 613a8368-b372-443f-8252-fb6dc31a003d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7c33ccf20e2b90fd967b5b15b491710d31ea83b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071933"
---
# <a name="refresh-from-database-db2tosql"></a>从数据库刷新 (DB2ToSQL) 
" **从数据库刷新** " 对话框允许您从 DB2 数据库中选择要刷新的对象。 对话框中的行是根据元数据的状态进行颜色编码的：  
  
-   如果对象元数据已在本地进行更改，并且在 DB2 数据库中，则行为蓝色。  
  
-   如果在 DB2 数据库中更改了对象元数据，但在 SSMA 中没有更改，则该行为黄色。  
  
-   如果对象元数据已在本地更改，但在 DB2 数据库中不存在，则该行为绿色。  
  
-   如果该对象是 DB2 数据库中的新对象，则该行为粉红色。  
  
您可以在 " **项目设置** " 对话框中指定默认对象刷新设置。 有关详细信息，请参阅 [项目设置&#40;同步&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md)。  
  
若要访问 " **从数据库刷新** " 对话框，请在 DB2 元数据资源管理器中右键单击对象，然后单击 " **从数据库刷新**"。  
  
## <a name="options"></a>选项  
**折叠 (-)**  
折叠所有对象组以隐藏单个对象。  
  
**展开 (+)**  
展开 "所有对象组" 以显示单个对象。  
  
**隐藏/显示相等对象**  
如果对象元数据在 DB2 数据库中和 SSMA 中是相同的，则隐藏列表中的对象。  
  
**从数据库刷新 (箭头按钮)**  
使用箭头按钮指定应在 SSMA 中更新所选对象的元数据。  
  
**不从数据库刷新 (X 按钮)**  
使用 X 按钮指定不应在 SSMA 中更新所选对象的元数据。  
  
**图例**  
显示 " **图例** " 对话框。 图例包含行颜色和元数据状态之间的映射。  
  
若要使 " **图例** " 对话框位于 " **从数据库刷新** " 对话框的顶部，请选中 " **顶部显示** " 复选框。  
  
