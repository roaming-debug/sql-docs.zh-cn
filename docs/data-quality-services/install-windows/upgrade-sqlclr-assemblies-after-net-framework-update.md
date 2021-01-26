---
title: .NET Framework 更新后升级 SQLCLR 程序集
description: 了解在更新 .NET framework 后，如何更新 SQL Server Data Quality Services (DQS) 使用的 SQLCLR 程序集。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: b1a008cc-7e6b-4655-a869-bd429f986400
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 4b835c60da2d2848b33dd453605290a1fb056180
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765485"
---
# <a name="upgrade-sqlclr-assemblies-after-net-framework-update"></a>.NET framework 更新后升级 SQLCLR 程序集

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) 是引用 Microsoft .NET Framework 4 程序集的 SQL 公共语言运行时 (SQLCR) 例程的集合。 如果在计算机上安装的任何 .NET Framework 更新影响任何此类引用的 .NET Framework 程序集，则将导致全局程序集缓存 (GAC) 中的程序集的模块版本 ID (MVID) 发生更改。 这样会导致 GAC 中的引用程序集与 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中的程序集之间的 MVID 不匹配。  
  
 如果 .NET Framework 更新要求您重新启动 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 计算机，则受到影响的 SQLCLR 程序集将自动升级，以便修复在重新启动 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 计算机时产生的 MVID 不匹配问题。 但是，对于不要求您重新启动 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 计算机的 .NET Framework 更新，由于在您尝试使用 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 连接到 [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)]时程序集的 MVID 中的不匹配，将出现错误：  
  
```  
A new version of .NET was installed on this machine. In order to continue to work with DQS please run dqsinstaller.exe -upgradedlls.  
```  
  
 若要修复此问题，必须升级 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中受影响的 SQLCLR 程序集。 为此，您可以使用 **upgradedlls** 命令行参数运行 DQSInstaller.exe 文件，以跳过重新创建 DQS 数据库，而只升级受影响的程序集。 这样可确保保留您的知识库、数据质量项目以及 DQS 中的任何其他数据。  
  
## <a name="prerequisites"></a>先决条件  
  
-   您必须作为 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 计算机上 Administrators 组的成员登录。  
  
-   您的 Windows 用户帐户必须是安装了 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 的 SQL Server 实例中 sysadmin 固定服务器角色的成员。  
  
### <a name="to-upgrade-sqlclr-assemblies"></a>升级 SQLCLR 程序集  
  
1.  启动命令提示符。  
  
2.  在命令提示符下，将目录更改为 DQSInstaller.exe 出现的位置。 如果你已安装了 SQL Server 的默认实例，DQSInstaller.exe 文件将出现在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn 下：  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  在命令提示符下，键入以下命令，再按 Enter：  
  
    ```  
    dqsinstaller.exe -upgradedlls  
    ```  
  
4.  剩下的步骤与 [运行 DQSInstaller.exe 以便完成数据质量服务器安装](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer) 中 [从“开始”屏幕、“开始”菜单或 Windows 资源管理器运行 DQSInstaller.exe](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md)部分中的步骤 2-6 相同。  
  
## <a name="see-also"></a>另请参阅  
 [安装 Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [在安装 SQL Server 更新后升级 DQS 数据库架构](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md)  
  
  
