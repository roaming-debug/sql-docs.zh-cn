---
description: '连接到 Azure SQL 数据库 (MySQLToSQL) '
title: " (MySQLToSQL) 连接到 Azure SQL 数据库 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 81623d27-25af-444f-9779-1edb8c6fb470
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 44f97ee6686bbe61a90e5fccf2cfb165e7e97cf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069048"
---
# <a name="connect-to-azure-sql-database-mysqltosql"></a>连接到 Azure SQL 数据库 (MySQLToSQL) 
使用 "连接到 SQL Azure" 对话框连接到要迁移的 Azure SQL 数据库中的数据库。  
  
若要访问此对话框，请在 " **文件** " 菜单上，选择 " **连接到 SQL Azure**"。 如果以前已连接，则该命令将 **重新连接到 SQL Azure。**  
  
## <a name="options"></a>选项  
**服务器名称**  
  
选择或输入用于连接到 SQL Azure 的服务器名称。  
  
**Database**  
  
选择，输入或 **浏览** 数据库名称。  
  
> [!IMPORTANT]  
> SSMA for MySQL 不支持连接到 SQL Azure 中的 master 数据库。  
  
**用户名**  
  
输入 SSMA 将用于连接到 Azure SQL 数据库的用户名  
  
**密码**  
  
输入用户名的密码。  
  
**Encrypt**  
  
SSMA 建议将加密连接 SQL Azure。  
  
## <a name="create-azure-database"></a>创建 Azure 数据库  
如果 SQL Azure 帐户中没有数据库，则可以创建第一个数据库。  
  
若要首次创建新数据库，请执行以下步骤：  
  
1.  单击 "连接到 SQL Azure" 对话框中的 "浏览" 按钮  
  
2.  如果没有数据库，将显示以下两个菜单项。  
  
    1.  **(找不到任何数据库) 该数据库** 处于禁用状态并始终灰显  
  
    2.  **创建新数据库** ，该数据库仅在 SQL Azure 帐户上没有数据库时才启用。 单击此菜单项时，将显示 "创建 Azure 数据库" 对话框，其中包含数据库名称和大小。  
  
3.  创建数据库时，会将以下两个参数指定为输入：  
  
    1.  **数据库名称：** 输入数据库名称。  
  
    2.  **数据库大小：** 选择需要在 SQL Azure 帐户中创建的数据库大小。  
  
