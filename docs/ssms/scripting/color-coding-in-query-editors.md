---
title: 查询编辑器中的颜色编码
description: 了解如何对文本类别进行颜色编码，以帮助你更轻松地查找特定文本，以及如何配置自定义配色方案。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Query Editor [SQL Server Management Studio], color coding
- color coding [Query Editor]
ms.assetid: 802882dc-c997-4e3f-8a01-994bb43169ae
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96fe150cd54cfd37074949a278ed40570dd18e83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354757"
---
# <a name="color-coding-in-query-editors"></a>查询编辑器中的颜色编码

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在代码编辑器中输入的文本分配有一个类别；每个类别都由某种颜色来标识。 这些颜色有助于您在代码中快速查找文本。 例如，注释突出显示为深绿色。 下表列出了最常用的颜色。 在 **“工具”** 菜单的 **“选项”** 中，可以查看颜色及其类别的完整列表，并可配置自定义配色方案。 有关如何更改默认颜色的详细信息，请参阅 [Change Font Color, Size, and Style](./change-font-color-size-and-style.md)。  
  
## <a name="default-code-colors"></a>默认代码颜色  
  
|Color|类别|  
|-----------|--------------|  
|Red|SQL 字符串|  
|暗绿色|注释|  
|黑色，银色背景|SQLCMD 命令|  
|洋红色|系统函数|  
|绿色|系统表、视图或表值函数。 此外，还包括系统架构 sys 和 INFORMATION_SCHEMA。|  
|蓝色|关键字|  
|青色|行号或模板参数|  
|褐紫红色|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程|  
|深灰色|运算符|  
  
## <a name="status-bar"></a>状态栏  
 您可在对象资源管理器中将已注册的服务器或 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 服务器配置为在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器状态栏中以不同颜色显示。 这样，当您同时打开许多窗口时，可帮助您识别每个编辑器窗口所连接到的服务器。 有关设置状态栏颜色的详细信息，请参阅[状态栏（数据库引擎查询编辑器）](./status-bar-database-engine-query-editor.md)。  
  
 某些类型的编辑器不显示状态栏，或者不支持多种颜色。  
  
