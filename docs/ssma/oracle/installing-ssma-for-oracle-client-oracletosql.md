---
title: 为 Oracle 客户端 (OracleToSQL) 安装 SSMA |Microsoft Docs
description: 了解用于 Oracle 客户端的 SQL Server 迁移助手 (SSMA) 的安装必备组件以及如何安装。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5bed1589c0852acc2d782028f6b9cc253301babe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073486"
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>安装 SSMA for Oracle client (OracleToSQL) 

SSMA 客户端由执行以下任务的程序文件组成：  
  
- 连接到 Oracle 数据库。
- 连接到的实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
- 将 Oracle 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语法。
- 将对象加载到中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。
- 将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

本主题提供安装 SSMA 的安装先决条件和说明。

## <a name="prerequisites"></a>必备条件

SSMA 旨在用于 Oracle 9 或更高版本以及的所有版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

安装 SSMA 之前，请确保计算机满足以下要求：

- Windows 7 或更高版本，或 Windows Server 2008 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3.1 或更高版本。
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] 版本4.7.2 或更高版本。 你可以从 [.NET Framework 开发人员中心](https://go.microsoft.com/fwlink/?LinkId=48882)获取它。
- 与要迁移的 Oracle 数据库的连接。
- 访问承载目标实例的计算机上的和足够的权限， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或者 [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] 您将在其中迁移数据库对象和数据的。 有关详细信息，请参阅 [连接到 SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)。
- 建议使用 4 GB RAM。  
  
## <a name="installing-the-ssma-for-oracle-client"></a>安装 SSMA for Oracle 客户端

SSMA 是一款可以从 Web 下载的工具。 若要下载最新版本，请参阅 [SQL Server 迁移助手下载 "页](https://aka.ms/ssmafororacle)。

安装 SSMA 客户端：

1. 双击 SSMAforOracle_ " ***n***"，其中 *n* 是生成号。
2. 在欢迎页上，单击 " **下一步**"。

   如果未安装必备组件，则会出现一条消息，指示必须首先安装所需的组件。 请确保已安装所有必备组件，然后再次运行安装程序。  

3. 阅读 End-User 许可协议。 如果同意，请选择 " **我接受协议**"，然后单击 " **下一步**"。
4. 在 " **选择安装类型** " 页上，单击 " **典型**"。
5. 在 " **准备安装** " 页上，可以启用或禁用每次启动该工具时的遥测和自动更新检查。 单击“安装”以开始安装。

> [!IMPORTANT]
> 安装新版本之前，请卸载所有以前版本的 SSMA for Oracle。

默认安装位置为 `C:\Program Files\Microsoft SQL Server Migration Assistant for Oracle`。

除了 SSMA 程序文件之外，还必须在上安装 SSMA for Oracle 扩展包 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 有关详细信息，请参阅 [在 SQL Server 上安装 SSMA 组件](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)。

## <a name="see-also"></a>另请参阅

- [在 SQL Server 上安装 SSMA 组件](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)
- [将 Oracle 数据库迁移到 SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
