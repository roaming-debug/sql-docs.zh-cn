---
title: 使用 SysPrep 安装 SQL Server
description: 通过 SQL Server Sysprep，可在计算机上准备 SQL Server 的独立实例，以后再完成配置。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: e1792eeb-2874-4653-b20e-3063f4eb4e5d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ff653258a42883d2942170f9b26d04f288fcb7f3
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104611109"
---
# <a name="considerations-for-installing-sql-server-using-sysprep"></a>使用 SysPrep 安装 SQL Server 的注意事项

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过 SysPrep，你可以在计算机上准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的独立实例，以后再完成配置。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 提供一个两步过程来得到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已配置独立实例。 这些步骤包括以下内容：  
  
- [准备映像](#BKMK_PrepareImage)  
  
    此步骤在安装了产品二进制文件后停止安装过程，并且不为正准备的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例配置计算机、网络或帐户特定的信息。  
  
- [完成映像](#BKMK_CompleteImage)  
  
    此步骤允许您完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已准备实例的配置。 在此步骤中，您可以提供计算机、网络和帐户特定的信息。  
  
有关如何使用 SysPrep 安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅[使用 SysPrep 安装 SQL Server](../../database-engine/install-windows/install-sql-server-using-sysprep.md)。  
  
## <a name="common-uses-for-ssnoversion-sysprep"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 的常见用途  
可以通过以下任意方式使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 功能：  
  
- 通过使用“准备映像”步骤，可以在同一台计算机上准备一个或多个未配置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 可以在同一台计算机上使用“完成映像”步骤配置这些已准备实例。  
  
- 您可以捕获已准备实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序配置文件，并且使用它在多台计算机上准备其他未配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，以用于以后的配置。  
  
- 与 Windows 系统准备工具（也称为 Windows SysPrep）结合使用，您可以在源计算机上创建操作系统的一个映像（包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未配置已准备实例）， 然后可以将该操作系统映像部署到多台计算机。 在完成操作系统的配置后，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序的“完成映像”步骤配置已准备实例。  
  
    Windows SysPrep 工具用于准备 Windows 操作系统映像。 它用于捕获操作系统的自定义映像，以便在整个组织中部署。 有关 SysPrep 及其用法的详细信息，请参阅 [SysPrep](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)。  
  
## <a name="installation-media-considerations"></a>安装介质注意事项  
 如果您使用的是完整版的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，请注意以下事项：  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 Express 以外的版本：  
  
    - “准备映像”步骤使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Evaluation 版本来安装产品二进制文件。 在完成该实例后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的版本依赖于在“完成映像”步骤中提供的产品 ID。  
  
    - 如果您提供 Evaluation Edition 产品 ID，则评估期将设置为在已准备实例完成后的 180 天到期。  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 Express 版本：  
  
    - 若要准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express 版本的实例，请使用 Express 安装介质。  
  
    - 不能为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express 版本的已准备实例指定产品 ID。  
  
## <a name="supported-ssnoversion-installations"></a>支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装  
[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中的 SysPrep 支持所有功能，包括工具。  
  
您可以为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 或早期版本的并行安装准备多个实例。 这些实例的功能必须支持 SysPrep。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 在“准备映像”步骤结束时自动安装和完成。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Writer 在您准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例时自动准备。 它们在使用“完成映像”步骤完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例时完成。  
  
若要了解 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 受支持的版本，请参阅 [SQL Server 2017 各个版本及其支持的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)和 [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md)  
  
您可以在配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已准备实例时执行版本升级。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express 版本，不支持此选项。  
  
从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]开始， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 支持从命令行进行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集安装。  
  
## <a name="ssnoversion-sysprep-limitations"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep 限制  
不支持修复已准备实例。 如果在“准备映像”或“完成映像”步骤中安装程序失败，您必须运行卸载。  
  
##  <a name="prepare-image"></a><a name="BKMK_PrepareImage"></a> 准备映像  
“准备映像”步骤安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品和功能，但不配置安装。  
  
可在此步骤中指定要安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 产品安装文件的安装位置。 你可以通过“安装中心”的“高级”页上的“SysPrep 部署的独立实例的映像准备”或从命令提示符准备 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。  
  
- 您可以在同一台计算机上准备可以在以后完成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的多个实例。  
  
- 您可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的现有已准备实例中添加或删除 SysPrep 安装支持的功能。  
  
 在已准备了该实例后， **“开始”** 菜单上的快捷方式可用于完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已准备实例的配置。  
  
##  <a name="complete-image"></a><a name="BKMK_CompleteImage"></a> 完成映像  
可以使用以下方法之一来完成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例：  
  
- 使用“开始”菜单上的快捷方式。  
  
- 访问“安装中心”的“高级”页上的“已准备独立实例的映像完成”步骤。  
  
## <a name="see-also"></a>另请参阅  
[计划 SQL Server 安装](../../sql-server/install/planning-a-sql-server-installation.md)