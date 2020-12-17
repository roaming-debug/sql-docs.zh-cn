---
title: SQL Server 属性（“AlwaysOn 高可用性”选项卡）
description: 若要将可用性组用作高可用性和灾难恢复解决方案，请在 SQL Server 中启用 Always On 可用性组功能。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d8630923-a600-4f1c-aca1-027453a3ec82
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: d623bd63cfb9d2d72f41bf498259b1d13fe651b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478418"
---
# <a name="sql-server-properties-always-on-high-availability-tab"></a>SQL Server 属性（“AlwaysOn 高可用性”选项卡）
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager “SQL Server 属性”对话框中的“AlwaysOn 高可用性”选项卡启用或禁用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中的“AlwaysOn 可用性组”功能。 启用 AlwaysOn 可用性组是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例将可用性组用作高可用性和灾难恢复解决方案的一个先决条件。  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a>先决条件  
 若要启用 AlwaysOn 可用性组，服务器实例必须满足以下先决条件：  
  
-   该服务器实例必须驻留在 Windows Server 故障转移群集 (WSFC) 节点上。  
  
-   若要位于同一个可用性组中，实例必须位于同一个 WSFC 群集中。 可用性组无法跨越多个 WSFC 群集。  
  
-   服务器实例必须正在运行支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]版本。  
  
-   一次仅为一个服务器实例启用 AlwaysOn 可用性组。 在启用 AlwaysOn 可用性组之后，一直等待，直到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务已重启，然后才启用下一个服务器实例。  
  
> [!NOTE]  
>  有关功能支持的信息和有关 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]其他先决条件、局限性和建议的信息，请参阅 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 联机丛书。  
  
## <a name="dialog-options"></a>对话框选项  
 **Windows 故障转移群集名称**  
 显示本地计算机在其中作为一个节点的 WSFC 群集的名称。  
  
 **启用 AlwaysOn 可用性组**  
 使用此复选框可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的这一实例上启用或禁用 AlwaysOn 可用性组，如下所示：  
  
-   如果此复选框为空，则当前禁用了 AlwaysOn 可用性组。 若要启用 AlwaysOn 可用性组，请选中此复选框，单击“确定”，然后手动重启 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  
  
-   如果已选中此复选框，则当前启用了 AlwaysOn 可用性组。 若要禁用 AlwaysOn 可用性组，请取消选中此复选框，然后单击“确定”。 这会导致服务器实例重新启动。  
  
    > [!TIP]  
    >  禁用 AlwaysOn 可用性组之后，应从服务器实例中删除任何本地可用性副本。 如果您删除了给定可用性组的最后一个副本，则还应删除此组。  
  
## <a name="ui-element-list"></a>UI 元素列表  
  
> [!NOTE]  
>  有关在禁用 AlwaysOn 可用性组后如何进行操作的详细信息以及如何创建和配置可用性组的信息，请参阅 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 联机丛书。  
  
  
