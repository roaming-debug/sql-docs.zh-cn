---
description: 修复失败的 SQL Server 安装
title: 修复失败的 SQL Server 安装 | Microsoft Docs
deescription: This article describes the scenarios where you can try a repair operation to fix failed SQL Server installation.
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 90c11b28-892b-44d6-978e-0eee48c75b7d
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 7524ea73ce04db70b75df3b8a4306c7304f475de
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125817"
---
# <a name="repair-a-failed-sql-server-installation"></a>修复失败的 SQL Server 安装

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

可以在以下情况下使用修复操作：  
  
- 修复在成功安装后损坏的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
- 如果在将实例名称映射到新升级的实例后取消了升级操作或升级操作失败，修复 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
    - 如果在摘要日志中看到以下消息，则可以修复失败的升级实例：  
  
         “[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级失败。 若要继续操作，请调查失败的原因，更正问题，然后修复您的安装。”  
  
    - 如果在摘要日志中看到以下消息，则需要先卸载再重新安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 不能修复 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 
  
         “[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 升级失败。 若要继续操作，请调查失败的原因，更正问题。”  
  
 在修复 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例时：  
  
- 将替换所有缺失或损坏的文件。 
  
- 将替换所有缺失或损坏的注册表项。 
  
- 所有缺失或无效的配置值将设置为默认值。 
  
 继续之前，请查阅有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集的以下重要信息：  
  
- 必须在单个群集节点上运行修复。 
  
- 若要在“准备”操作失败之后修复故障转移群集节点，请使用 **“删除节点”** ，然后再次执行“准备”步骤。 有关详细信息，请参阅[在 SQL Server 故障转移群集中添加或删除节点（安装程序）](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。 
  
## <a name="repair-a-failed-installation-of-ssnoversion-from-the-installation-center"></a>从安装中心修复失败的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装 
  
1. 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装介质中启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序 (setup.exe)。 
  
2. 安装必备组件并进行系统验证之后，安装程序会显示“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装中心”页。 
  
3. 单击左侧导航区域中的“维护”  ，然后单击“修复”  启动修复操作。 
  
   >[!TIP]  
   > 如果使用“开始”菜单启动了安装中心，您将需要在此时提供安装介质的位置。 
  
4. 将运行安装程序支持规则和文件例程，以确保您的系统上安装了必备组件，并且计算机能够通过安装程序验证规则。 单击 **“确定”** 或 **“安装”** 以继续操作。 
  
5. 在“ 选择实例 ”页上选择要修复的实例，然后单击“ **下一步** ”继续操作。 
  
6. 将运行修复规则以验证修复操作。 若要继续，请单击 **“下一步”** 。 
  
7. “准备修复”页指示修复操作已准备就绪，可以继续。 若要继续，请单击 **“修复”** 。 
  
8. “修复进度”页显示修复操作的状态。 “完成”页指示修复操作已完成。 
  
### <a name="to-repair-a-failed-installation-of-ssnoversion-using-command-prompt"></a>使用命令提示符修复失败的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装  
  
1. 在命令提示符下运行以下命令：  
  
    ```  
    Setup.exe /q /ACTION=Repair /INSTANCENAME=instancename  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [查看和读取 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)   
 [安装操作指南文章](/previous-versions/sql/)  
  
