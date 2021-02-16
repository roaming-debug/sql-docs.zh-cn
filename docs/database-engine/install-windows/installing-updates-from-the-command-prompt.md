---
title: 从命令提示符安装更新 | Microsoft Docs
description: 本文介绍 SQL Server 更新安装的命令语法。 可根据你组织的需求测试并修改安装脚本。
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: bc98ba2b-aae9-4d01-aa85-d4c36428cb0b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ff4d078c165b91f147e39cecb3976939929cac7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353062"
---
# <a name="installing-updates-from-the-command-prompt"></a>从命令提示符安装更新

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

请根据您所在单位的需要测试并修改安装脚本。 
 
## <a name="sample-syntax-for-installation"></a>安装的示例语法 
更新包的名称可能会有变化，可能包含语言、版本和处理器组件。 在命令提示符下应用更新，从而将 <package_name> 替换为更新包的名称： 
 
- 更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的单个实例和所有共享组件（如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和管理工具）：您可以通过使用 InstanceName 参数或 InstanceID 参数指定实例。 若要更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的已准备实例，必须指定 InstanceID 参数。

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance
    ```
    或 
    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceID=\<Instance ID>. 
    ```

- 安装程序可以将最新的产品更新与主安装相集成，以便可以同时安装主产品及其适用的更新。 可以准备安装数据库引擎实例，以包括产品更新： 

    ```
    setup.exe /q /IAcceptSQLServerLicenseTerms /ACTION=PrepareImage /UpdateEnabled=True /UpdateSource=\<path where the update is downloaded> /INSTANCEID=\<Instance ID> /FEATURES=SQLEngine. 
    ```

- 仅更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件（如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和管理工具）： 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch 
    ```

- 更新计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和所有共享组件（如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和管理工具）： 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances. 
    ```

- 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的单个实例和所有共享组件（如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和管理工具）删除更新： 

    ```
    <package_name>.exe /qs /Action=RemovePatch /InstanceName=MyInstance. 
    ```

- 仅从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件（如 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和管理工具）删除更新： 

    ```
    <package_name>.exe /qs /Action=RemovePatch 
    ```

  > [!NOTE] 
  > 更新安装程序可以确保共享组件始终采用不低于最高级别的实例版本。 
 
## <a name="supported-parameters"></a>支持的参数 
 
> [!IMPORTANT] 
> 请尽可能在运行时提供安全凭据。 如果必须将凭据存储在脚本文件中，请确保该文件的安全以防受到未经授权的访问。 
 
|开关|说明| 
|------------|-----------------| 
|**/?**|显示无人参与安装命令提示符帮助| 
|**/action=Patch 或 /action=RemovePatch**|指定安装操作：Patch 或 RemovePatch。| 
|**/allinstances**|将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新应用于所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例以及所有不识别实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件。| 
|**/instancename=InstanceName***|将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新应用于名为 InstanceName 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例以及所有不识别实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件。| 
|**/InstanceID=Inst1**|将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inst1 实例，以及所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件和不识别实例的组件。| 
|**/quiet**|在无人参与模式下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新安装程序。| 
|**/qs**|仅显示进度 UI 对话。| 
|**/UpdateEnabled**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序是否应发现和加入产品更新。 有效值为 True 和 False 或 1 和 0。 默认情况下， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将包含找到的更新。| 
|**/IAcceptSQLServerLicenseTerms**|仅在为无人参与安装指定了 /Q 或 /QS 参数时是必需的。| 
 
 \* 不能通过指定此参数来将更新应用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的已准备实例。 必须指定 /instanceID 参数。 
 
## <a name="see-also"></a>另请参阅 
 [SQL Server 服务安装概述](./install-sql-server-servicing-updates.md) 
 
