---
description: '从数据库刷新 (AccessToSQL) '
title: 从数据库刷新 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 3b671f49-c4cc-44fd-801e-e738a8c79415
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: df61abefdab6642c098cbf456cd6147417cb2dd7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066302"
---
# <a name="refresh-from-database-accesstosql"></a>从数据库刷新 (AccessToSQL) 
使用 " **从数据库刷新** " 对话框，您可以选择要在 Access 数据库中刷新的对象。 对话框中的行是根据元数据的状态进行颜色编码的：  
  
-   如果对象元数据已在本地进行更改，并且在 Access 数据库中，则行为蓝色。  
  
-   如果对象元数据在 Access 数据库中发生更改，但在 SSMA 中不存在，则该行为黄色。  
  
-   如果对象元数据已在本地更改，但在 Access 数据库中不存在，则该行为绿色。  
  
-   如果对象在 Access 数据库中是新的，则该行为粉红色。  
  
您可以在 " **项目设置** " 对话框中指定默认对象刷新设置。 有关详细信息，请参阅 [AccessToSQL&#41;&#40;加载&#41; &#40;对象的项目设置 ](../../ssma/access/project-settings-loading-objects-accesstosql.md)  
  
若要访问 " **从数据库刷新** " 对话框，请在 "访问元数据资源管理器" 中右键单击任意 **数据库** 节点，然后单击 " **从数据库刷新**"。  
  
