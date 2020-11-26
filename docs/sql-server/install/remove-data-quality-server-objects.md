---
title: 删除数据质量服务器对象 | Microsoft Docs
description: 如果从 SQL Server 实例卸载数据质量服务器，则必须手动删除一些数据质量服务器对象，包括 DQS 数据库。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1b7c6dbb-b40e-4822-9caa-608e1056af8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3f3fd7d71c4c26de0aca445eaa49896c24a73d57
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127501"
---
# <a name="remove-data-quality-server-objects"></a>删除数据质量服务器对象
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  从 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 实例中卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]或者完全删除具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 实例并不删除某些 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 对象，包括 DQS 数据库。 这意味着，如果您使用 SQL Server 安装程序卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] ，并不会丢失您的 DQS 数据。 在卸载过程完成后，您必须手动删除这些 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 对象。  
  
> [!NOTE]
>  -   在卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]前，请考虑通过将所有现有知识库导出到 .dqsb 文件来备份这些知识库，以供以后使用该文件来将所有知识库导入回新的 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 安装。 只能从命令提示符使用适当的命令行参数来运行 DQSInstaller.exe，以导出和导入所有 DQS 知识库。 有关详细信息，请参阅 [使用 DQSInstaller.exe 导出和导入 DQS 知识库](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)  
> -   在删除 DQS 数据库之前，如果您想要保留数据库，则考虑备份数据库，在以后可以使用备份来还原数据。 有关此操作的信息，请参阅 [管理 DQS 数据库](../../data-quality-services/manage-dqs-databases.md)。  
  
## <a name="uninstall-data-quality-server-from-a-sql-server-instance"></a>从 SQL Server 实例卸载数据质量服务器  
 如果您从 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 实例仅卸载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则在完成 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 卸载过程后，必须手动从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例删除以下 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 对象：  
  
-   DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA 数据库。  
  
-   \##MS_dqs_db_owner_login## 和 ##MS_dqs_service_login## 登录名。  
  
-   来自 master 数据库的 DQInitDQS_MAIN 存储过程。  
  
 你可以通过右键单击对象，然后在快捷菜单中单击“删除”，在 SQL Server Management Studio 中删除这些对象。  
  
> [!IMPORTANT]  
>  如果通过命令提示符使用 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] 命令行参数从 SQL Server 实例中仅卸载 `-uninstall` ，则在卸载过程中将删除所有 DQS 对象。 您不必在卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]之后手动删除它们。 若要从命令提示符处卸载 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] ，请在命令提示符处键入以下命令，然后按 ENTER：   
> `dqsinstaller.exe -uninstall`  
  
## <a name="uninstall-sql-server-instance-containing-data-quality-server"></a>卸载包含数据质量服务器的 SQL Server 实例  
 如果您在完全卸载具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]实例，则在卸载过程完成后，您必须手动从计算机上删除 DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA 数据库。 对于默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装，DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA 数据库文件在 C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA 处提供。  
  
## <a name="see-also"></a>另请参阅  
 [卸载现有 SQL Server 实例（安装程序）](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)   
 [卸载 SQL Server 2016](../../sql-server/install/uninstall-sql-server.md)  
  
  
