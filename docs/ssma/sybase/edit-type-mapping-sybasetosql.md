---
description: 编辑类型映射 (SybaseToSQL)
title: 编辑类型映射 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 513f071a-d5e6-4ed5-acca-269bf76323c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 7fc5d4a2e2e907feb867d5cd9e44d2b7d8e77bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064222"
---
# <a name="edit-type-mapping-sybasetosql"></a>编辑类型映射 (SybaseToSQL)
利用 " **编辑类型映射** " 对话框，您可以指定如何在源数据库对象和目标数据库对象之间映射类型。  
  
可以在以下几个位置访问此对话框：  
  
-   选择源数据库或数据库对象时，" **类型映射** " 选项卡将显示在 "元数据资源管理器" 右侧。 单击 " **添加** " 以添加新的类型映射，或者单击 " **编辑** " 更改现有的类型映射。  
  
-   在 " **工具** " 菜单上，选择 " **项目设置** " 或 " **默认项目设置**"。 在生成的对话框中，选择 " **类型映射**"。 单击 " **添加** " 以添加新的类型映射，或者单击 " **编辑** " 更改现有的类型映射。  
  
表特定的类型映射会重写数据库和项目类型映射。 数据库特定的映射会替代项目映射。  
  
## <a name="options"></a>选项  
**源类型**  
选择要映射到数据类型的源数据类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
如果数据类型的长度可变，以下字段将显示在 " **源类型**" 下：  
  
**From**  
指定此映射的最小长度。 例如，对于 **nchar** 数据类型，可以输入10来指定此映射适用于从 **nchar (10)** 开始的范围。  
  
**To**  
指定此映射的最大长度。 例如，对于 **nchar** 数据类型，可以输入20来指定此映射适用于以 **nchar (20)** 结束的范围。  
  
**目标类型**  
选择 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 源数据类型要映射到的数据类型。 当 SSMA 在中创建表或存储过程时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，源数据类型将更改为此数据类型。  
  
如果数据类型的长度可变，以下字段将显示在 " **目标类型**" 下：  
  
**Replace with**  
指定此映射的目标长度。 例如，对于 **nvarchar** 数据类型，可以输入20来指定应将指定的源数据类型映射到 **nvarchar (20)**。  
  
