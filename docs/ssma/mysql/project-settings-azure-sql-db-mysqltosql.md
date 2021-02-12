---
description: " (Azure SQL 数据库的项目设置)  (MySQLToSQL) "
title: " (Azure SQL 数据库)  (MySQLToSQL) 的项目设置 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8c06420a-533b-4de0-948d-a0c6b368c544
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a5c3075180e60737931adfb1f544ace8bb38b9b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071861"
---
# <a name="project-settings-azure-sql-database-mysqltosql"></a> (Azure SQL 数据库的项目设置)  (MySQLToSQL) 
通过 SQL Azure 项目设置，你可以配置要添加到连接对话框中的 Azure SQL 数据库后缀，还允许在 SQL Azure 连接中实现检测信号机制。  
  
"SQL Azure" 窗格在 " **项目设置** " 和 " **默认项目设置** " 对话框中可用。  
  
-   使用 "项目设置" 对话框可以设置当前项目的配置选项。 若要访问 SQL Azure 设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后选择 " **SQL Azure**"。  
  
-   使用 "默认项目设置" 对话框可以为所有项目设置配置选项。 若要访问 SQL Azure 设置，请在 " **工具** " 菜单上选择 " **DefaultProject 设置**"，选择 "迁移项目类型" 作为 "从 **迁移目标版本** 中 SQL Azure" 下拉菜单，若要访问 SQL Azure 窗格中的设置，请单击左窗格底部的 " **常规** "，然后选择 " **SQL Azure**"。  
  
## <a name="options"></a>选项  
  
## <a name="connectivity"></a>连接  
**检测信号间隔**  
  
指定一个时间间隔，该时间间隔用于检测信号机制以使 SQL Azure 连接以 "分钟：秒" 格式保持活动状态。  
  
**默认值**： "4:45"  
  
该值应以 "m:ss" 格式指定 (例如，"4:45" 或 "0:50" ) 。  
  
**SQL Azure 服务器后缀**  
  
指定 SQL Azure 服务器后缀  
  
**默认值**： "database.windows.net"。  
  
