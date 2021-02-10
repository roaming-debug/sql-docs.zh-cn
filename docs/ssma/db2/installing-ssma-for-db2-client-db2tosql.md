---
title: 安装 SSMA for DB2 client (DB2ToSQL) |Microsoft Docs
description: 了解 DB2 客户端的 SQL Server 迁移助手 (SSMA) 的安装先决条件以及安装方法。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 3ae2a470-6afd-4512-b6d1-fcbe6afe88ad
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: dd9c4116acd482fa12b74556b9d1ad1fe9b06cdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014727"
---
# <a name="installing-ssma-for-db2-client-db2tosql"></a>安装 SSMA for DB2 client (DB2ToSQL) 

SSMA 客户端由执行以下任务的程序文件组成：

- 连接到 DB2 数据库。
- 连接到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
- 将 DB2 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法。
- 将对象加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
- 将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

本主题提供安装 SSMA 的安装先决条件和说明。

## <a name="prerequisites"></a>必备条件

SSMA 设计用于在 z/OS 版本9.0 和10.0 上使用 db2，在 LUW 版本9.8 和10.1 或更高版本上使用 db2，适用于版本7.1 或更高版本和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 或更高版本的 db2。

安装 SSMA 之前，请确保计算机满足以下要求：

- Windows 7 或更高版本，或 Windows Server 2008 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- DB2 的 Microsoft OLE DB 提供程序版本5或更高版本，以及与要迁移的 DB2 数据库的连接。
- 对承载或 Azure SQL 数据库的目标的计算机拥有足够的权限，你将在该计算机上 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 迁移数据库对象和数据。 有关详细信息，请参阅 [连接到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)。
- 建议使用 4 GB RAM。

## <a name="microsoft-ole-db-provider-for-db2"></a>Microsoft OLE DB Provider for DB2

若要下载 DB2 版本6.0 的 OLE DB 提供程序，请访问 [Microsoft® SQL Server®2017功能包](https://www.microsoft.com/download/details.aspx?id=55992)。

SSMA 是一款可以从 Web 下载的工具。 若要下载最新版本，请参阅 [SQL Server 迁移助手下载 "页](https://aka.ms/ssmafordb2)。

安装 SSMA 客户端：

1. 双击 SSMAforDB2_ " ***n***"，其中 *n* 是生成号。
2. 在“欢迎”页上，选择“下一步”。

   如果尚未安装必备组件，则会出现一条消息，指示必须首先安装所需的组件。 请确保已安装所有必备组件，然后再次运行安装程序。

3. 阅读 End-User 许可协议。 如果同意，请选择 " **我接受协议**"，然后选择 " **下一步**"。
4. 在 " **选择安装类型** " 页上，选择 " **典型**"。
5. 在 " **准备安装** " 页上，可以启用或禁用每次启动该工具时的遥测和自动更新检查。 单击“安装”以开始安装。

> [!IMPORTANT]
> 安装新版本之前，请卸载所有以前版本的 SSMA for DB2。

默认安装位置为 `C:\Program Files\Microsoft SQL Server Migration Assistant for DB2`。

## <a name="see-also"></a>另请参阅

- [在 SQL Server 上安装 SSMA 组件](../../ssma/db2/installing-ssma-components-on-sql-server-db2tosql.md)
- [将 DB2 数据库迁移到 SQL Server](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
