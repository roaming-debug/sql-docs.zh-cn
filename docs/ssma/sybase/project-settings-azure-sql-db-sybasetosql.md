---
description: " (Azure SQL 数据库的项目设置 )  (SybaseToSQL) "
title: " (Azure SQL 数据库 )  (SybaseToSQL) 的项目设置 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 57002374-0d4d-43c1-b4e9-cbec02355a9c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b532fddbc5eda07ab38aedcb806347846cc0ac59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068762"
---
# <a name="project-settings-azure-sql-database--sybasetosql"></a> (Azure SQL 数据库的项目设置 )  (SybaseToSQL) 
利用 Azure SQL 数据库项目设置，你可以配置要添加到连接对话框中的 Azure SQL 数据库数据库后缀，还允许在 Azure SQL 数据库连接中实施检测信号机制。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "Azure SQL 数据库" 窗格。  
  
-   使用 "项目设置" 对话框可以设置当前项目的配置选项。 若要访问 Azure SQL 数据库设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后选择 " **Azure sql 数据库**"。  
  
-   使用 "默认项目设置" 对话框可以为所有项目设置配置选项。 若要访问 Azure SQL 数据库设置，请在 " **工具** " 菜单上选择 " **DefaultProject 设置**"，单击左侧窗格底部的 " **常规** "，然后选择 " **Azure sql 数据库**"。  
  
## <a name="connectivity"></a>连接  
**检测信号间隔**  
  
指定一个时间间隔，该时间间隔用于检测信号机制以使 Azure SQL 数据库连接保持活动状态 "分钟：秒" 格式。  
  
**默认值**： "4:45"  
  
该值应以 "m:ss" 格式指定 (例如，"4:45" 或 "0:50" ) 。  
  
**Azure SQL 数据库服务器后缀**  
  
指定 Azure SQL 数据库服务器后缀  
  
**默认值**： "database.windows.net"。  
  
