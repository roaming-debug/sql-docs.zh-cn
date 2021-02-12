---
title: 安装 SSMA for MySQL client (MySQLToSQL) |Microsoft Docs
description: 了解 SQL Server 迁移助手 (SSMA) for MySQL client 和如何安装的安装必备组件。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing client,Licensing
ms.assetid: ede3128c-370d-45a5-a815-3d94eecaea30
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e2eaa9daf9128b43ac061cad66e7e993882e43d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070132"
---
# <a name="installing-ssma-for-mysql-client-mysqltosql"></a>安装 SSMA for MySQL client (MySQLToSQL) 

SSMA for MySQL 客户端包含执行以下任务的程序文件：

- 连接到 MySQL 数据库。  
- 连接到或的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。
- 将 MySQL 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 对象。
- 将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。
- 将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

本主题提供安装 SSMA for MySQL 客户端的安装先决条件和说明。

## <a name="prerequisites"></a>必备条件

SSMA for MySQL 旨在使用 MySQL 4.1 或更高版本，以及2012或更高版本的所有版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

安装 SSMA 之前，请确保计算机满足以下要求：

- Windows 7 或更高版本，或 Windows Server 2008 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- MySQL ODBC 5.1 驱动程序和与要迁移的 MySQL 数据库的连接。 你可以从 MySQL 网站安装 MySQL。 有关连接的详细信息，请参阅 [连接到 MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)。
- 在宿主目标实例的计算机上访问和足够的权限，你将在该计算机上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移数据库对象和数据。 有关详细信息，请参阅 [连接到 SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)。
- 如果是 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 项目，则对要在 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 其中迁移数据库对象和数据的实例的访问权限和足够权限。 有关详细信息，请参阅 [连接到 AZURE SQL 数据库 &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)。
- 建议使用 4 GB RAM。

## <a name="installing-ssma-for-mysql-client"></a>安装 SSMA for MySQL 客户端

SSMA 是一款可以从 Web 下载的工具。 若要下载最新版本，请参阅 [SQL Server 迁移助手下载 "页](https://aka.ms/ssmaformysql)。

安装 SSMA 客户端：

1. 双击 SSMAforMySQL_ " ***n***"，其中 *n* 是生成号。
2. 在“欢迎”页面上，单击“下一步”。 

   如果未安装必备组件，则将显示一条消息，指示必须首先安装所需的组件。 再次运行安装程序之前，请确保已安装所有必备组件。

3. 阅读 End-User 许可协议。 如果同意，请选择 " **我接受协议**"，然后单击 " **下一步**"。
4. 在 " **选择安装类型** " 页上，单击 " **典型**"。
5. 在 " **准备安装** " 页上，可以启用或禁用每次启动该工具时的遥测和自动更新检查。 单击“安装”以开始安装。

> [!IMPORTANT]
> 安装新版本之前，请卸载所有以前版本的 SSMA for MySQL。

默认安装位置为 `C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL`。

## <a name="see-also"></a>另请参阅

- [将 MySQL 数据库迁移到 SQL Server-Azure SQL 数据库](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
