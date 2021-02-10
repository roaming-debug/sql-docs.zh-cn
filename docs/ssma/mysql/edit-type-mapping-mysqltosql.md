---
description: 编辑类型映射 (MySQLToSQL)
title: 编辑类型映射 (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 184f7ab2-725f-491e-a15b-b889f2fb6a68
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b1ddf264835cf0a9a008335787353d7a1051431f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016597"
---
# <a name="edit-type-mapping-mysqltosql"></a>编辑类型映射 (MySQLToSQL)
利用 " **编辑类型映射** " 对话框，您可以指定如何在源数据库对象和目标数据库对象之间映射类型。  
  
可以在以下几个位置访问此对话框：  
  
-   选择源数据库或数据库对象时，" **类型映射** " 选项卡将显示在 "元数据资源管理器" 右侧。 单击 " **添加** " 以添加新的类型映射，或者单击 " **编辑** " 更改现有的类型映射。  
  
-   在 " **工具** " 菜单上，选择 " **项目设置** " 或 " **默认项目设置**"。 在生成的对话框中，选择 " **类型映射**"。 单击 " **添加** " 以添加新的类型映射，或者单击 " **编辑** " 更改现有的类型映射。  
  
-   表特定的类型映射会重写数据库和项目类型映射。 数据库特定的映射会替代项目映射。  
  
## <a name="options"></a>选项  
  
##### <a name="source-type"></a>源类型  
选择要映射到 SQL Server 数据类型的源数据类型。  
  
如果数据类型的长度可变，以下字段将显示在 **Sourcetype** 下：  
  
##### <a name="from"></a>From  
指定此映射的最小长度。 例如，对于 **nchar** 数据类型，可以输入10来指定此映射适用于从 **nchar (10)** 开始的范围。  
  
##### <a name="to"></a>功能  
指定此映射的最大长度。 例如，对于 **nchar** 数据类型，可以输入20来指定此映射适用于以 **nchar (20)** 结束的范围。  
  
##### <a name="target-type"></a>目标类型  
选择源数据类型要映射到的 SQL Server 数据类型。 当 SSMA 创建表或 SQL Server 中，源数据类型将更改为此数据类型。  
  
如果数据类型的长度可变，以下字段将显示在 " **目标类型**" 下：  
  
##### <a name="replace-with"></a>替换为  
指定此映射的目标长度。 例如，对于 **nvarchar** 数据类型，可以输入20来指定应将指定的源数据类型映射到 **nvarchar (20) 。**  
  
