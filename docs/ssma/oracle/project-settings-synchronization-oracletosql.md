---
description: 项目设置（同步）(OracleToSQL)
title: " (同步)  (OracleToSQL) 的项目设置 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e223fb7d-05ec-4fa5-8973-d845c33a23dd
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: ff0869f3219eb21d079be062203f20e4602217c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067772"
---
# <a name="project-settingssynchronization-oracletosql"></a>项目设置（同步）(OracleToSQL)
" **项目设置** " 对话框的 "同步" 页包含用于自定义 SSMA 将数据库对象（如表和存储过程）加载和刷新到的方式的设置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
默认操作选项指定用于刷新 Oracle 数据库中的对象的默认设置，并指定与 SQL Server 数据库同步对象的默认设置。 有关详细信息，请参阅 [从数据库中刷新-Oracle](../../ssma/oracle/refresh-from-database-oracletosql.md)。  
  
您可以访问两个包含相同设置的不同同步页：  
  
-   若要指定所有未来 SSMA 项目的设置，请在 " **工具** " 菜单上单击 " **默认项目设置**"，然后单击左窗格底部的 " **同步** "。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上单击 " **项目设置**"，然后单击左窗格底部的 " **同步** "。  
  
## <a name="miscellaneous-options"></a>“杂项”选项  
**尝试次数**  
指定在将对象加载到时 SSMA 应执行的尝试次数 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 当前尝试中未加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的对象将重试，直到 SSMA 达到当前同步过程中的最大尝试次数。 默认值设置为 **2**  
  
## <a name="synchronization-for-oracle-options"></a>Oracle 选项同步  
**对本地和远程对象的操作更改**  
当对象定义在 SSMA 中和数据库服务器上发生更改时，在同步对话框中指定默认设置。 默认值设置为 " **从数据库刷新**"。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**对本地对象的操作更改**  
当对象在 SSMA 中发生更改时，在同步对话框中指定默认设置。 默认值设置为 **Skip**。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**对远程对象的操作更改**  
在数据库服务器上的对象发生更改时，在同步对话框中指定默认设置。 默认值设置为 " **从数据库刷新**"。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**缺少本地对象元数据时的操作**  
当缺少本地元数据时，指定同步对话框中的默认设置。 默认值设置为 " **从数据库刷新**"。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
## <a name="synchronization-for-sql-server-options"></a>SQL Server 选项的同步  
**对本地和远程对象的操作更改**  
当对象定义在 SSMA 中和数据库服务器上发生更改时，在同步对话框中指定默认设置。 默认值设置为 **写入数据库**。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择了 " **写入数据库**"，则在满足条件时，SSMA 将根据 SSMA 元数据内容来更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**对本地对象的操作更改**  
当对象在 SSMA 中发生更改时，在同步对话框中指定默认设置。 默认值设置为 **写入数据库**。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 会根据 SSMA 元数据内容更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**对远程对象的操作更改**  
在数据库服务器上的对象发生更改时，在同步对话框中指定默认设置。  默认值设置为 " **从数据库刷新**"。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 会将数据库定义加载到元数据中。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 会根据 SSMA 元数据内容更新数据库中的对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
**缺少本地对象元数据时的操作**  
当缺少本地元数据时，指定同步对话框中的默认设置。 默认值设置为 " **从数据库刷新**"。  
  
-   如果选择 " **从数据库刷新**"，则在满足条件时，SSMA 将选择 " **从数据库刷新** " 选项。  
  
-   如果选择 " **写入数据库**"，则在满足条件时，SSMA 将从数据库中删除该对象。  
  
-   如果选择 " **跳过**"，SSMA 不会执行任何刷新操作。  
  
