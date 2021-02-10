---
title: 为 SAP ASE 客户端 (SybaseToSQL) 安装 SSMA |Microsoft Docs
description: 了解 SQL Server 迁移助手 (SSMA) 适用于 SAP 自适应服务器企业 (ASE) 和安装方法的安装先决条件。
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e770c2f2-52b9-4471-a207-0d35df41399c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d9b20774b0a32b0d9d007053bedd2674e79d4e12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015357"
---
# <a name="installing-ssma-for-sap-ase-client-sybasetosql"></a>安装 SSMA for SAP ASE client (SybaseToSQL) 

SSMA 客户端包括用于连接到 SAP 自适应服务器企业 (ASE) 数据库服务器和实例或的程序文件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 、将 ASE 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 语法、将其加载到或语法，然后将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 。

本主题提供安装 SSMA 的安装先决条件和说明。

## <a name="prerequisites"></a>必备条件

SSMA 旨在与 SAP ASE 11.9.2 或更高版本以及所有版本结合使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

安装 SSMA 之前，请确保计算机满足以下要求：

- Windows 7 或更高版本，或 Windows Server 2008 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)].NET Framework 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- Sybase OLE DB/ADO.Net/ODBC 提供程序和与 SAP ASE 数据库服务器的连接，其中包含要迁移的数据库。 可以从 SAP ASE 产品媒体安装提供程序。 有关连接的详细信息，请参阅 [连接到 SYBASE ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md)。
- 访问承载目标实例的计算机上的和足够的权限， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或者 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 您将在其中迁移数据库对象和数据的。 有关详细信息，请参阅连接[到 SQL Server &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sql-server-sybasetosql.md) / [连接到 Azure SQL 数据库 &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-azure-sql-db-sybasetosql.md)。
- 建议使用 4 GB RAM。

## <a name="installing-the-ssma-for-sybase-client"></a>为 Sybase 客户端安装 SSMA

SSMA 是一款可以从 Web 下载的工具。 若要下载最新版本，请参阅 [SQL Server 迁移助手下载 "页](https://aka.ms/ssmaforsybase)。

安装 SSMA 客户端：

1. 双击 SSMAforSybase_ " ***n***"，其中 *n* 是生成号。
2. 在欢迎页上，单击 " **下一步**"。

   如果未安装必备组件，则会出现一条消息，指示必须首先安装所需的组件。 请确保已安装所有必备组件，然后再次运行安装程序。

3. 阅读 End-User 许可协议。 如果同意，请选择 " **我接受协议**"，然后单击 " **下一步**"。
4. 在 "选择安装类型" 页上，单击 " **典型**"。
5. 在 " **准备安装** " 页上，可以启用或禁用每次启动该工具时的遥测和自动更新检查。 单击“安装”以开始安装。

> [!IMPORTANT]
> 请先卸载所有以前版本的 SSMA for Sybase，然后再安装新版本。

默认安装位置为 `C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase`。

除了 SSMA 程序文件之外，还必须在上安装用于 Sybase 扩展包的 SSMA [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [SQL Server &#40;SybaseToSQL&#41;上安装 SSMA 组件 ](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)。

## <a name="see-also"></a>另请参阅

- [在 SQL Server 上安装 SSMA 组件](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
- [将 Sybase ASE 数据库迁移到 SQL Server-Azure SQL 数据库](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
