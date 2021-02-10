---
description: 项目设置（同步）(MySQLToSQL)
title: " (同步)  (MySQLToSQL) 的项目设置 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 42061ff7-e6e7-497b-a0d9-440b9cf5986c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3f778b2bd361fab5ac1210005076692c813bb4b9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017307"
---
# <a name="project-settings-synchronization-mysqltosql"></a>项目设置（同步）(MySQLToSQL)
同步 **项目设置** 使你可以配置 MySQL 数据库对象与 SQL Server 数据库对象的同步方式。  
  
默认操作将指定用于刷新 MySQL 数据库中的对象的默认设置，并指定与 SQL Server 数据库同步对象的默认设置。 有关详细信息，请参阅 [Refresh from database &#40;MySQLToSQL&#41;](../../ssma/mysql/refresh-from-database-mysqltosql.md)  
  
您可以访问两个包含相同设置的不同同步页：  
  
-   若要指定所有未来 SSMA 项目的设置，请在 " **工具** " 菜单上单击 " **DefaultProject 设置**"，然后单击左窗格底部的 " **同步** "。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上单击 " **项目设置**"，然后单击左窗格底部的 " **同步** "。  
  
## <a name="options"></a>选项  
  
##### <a name="misc"></a>杂项  
  
##### <a name="attempts"></a>尝试次数  
提供要加载到 SQL Server 中的传递对象数的相关信息。 将对象加载到 SQL Server 通常是在多个阶段中执行的。 在第一次传递中无法加载的对象（如外键）可能会在下一步中成功加载。  
  
默认情况下，值为2。  
  
## <a name="synchronization-for-mysql"></a>MySQL 同步  
  
##### <a name="action-on-local-and-remote-object-change"></a>对本地和远程对象的操作更改  
当对象定义在 SSMA 中和数据库服务器上发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 "从数据库刷新"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 "跳过"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-on-local-object-change"></a>对本地对象的操作更改  
当对象在 SSMA 中发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-on-remote-object-change"></a>对远程对象的操作更改  
在数据库服务器上的对象发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>缺少本地对象元数据时的操作  
当缺少本地元数据时，指定同步对话框中的默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，则 SSMA 将不执行任何刷新操作  
  
## <a name="synchronization-for-sql-server"></a>SQL Server 同步  
  
##### <a name="action-on-local-and-remote-object-change"></a>对本地和远程对象的操作更改  
当对象定义在 SSMA 中和数据库服务器上发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择了 " **写入数据库**"，则在满足条件时，SSMA 将根据 SSMA 元数据内容来更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-on-local-object-change"></a>对本地对象的操作更改  
当对象在 SSMA 中发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 会根据 SSMA 元数据内容更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-on-remote-object-change"></a>对远程对象的操作更改  
在数据库服务器上的对象发生更改时，在同步对话框中指定默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 会根据 SSMA 元数据内容更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
##### <a name="action-when-local-object-metadata-is-missing"></a>缺少本地对象元数据时的操作  
当缺少本地元数据时，指定同步对话框中的默认设置。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 将选择 "从数据库刷新" 选项。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 将从数据库中删除该对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
