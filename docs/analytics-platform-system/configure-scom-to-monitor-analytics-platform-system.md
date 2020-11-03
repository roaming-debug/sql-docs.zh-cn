---
title: 配置 System Center Operations Manager 以监视 AP
description: 请按照以下步骤配置用于分析平台系统的 System Center Operations Manager (SCOM) 管理包。 管理包是从 SCOM 监视分析平台系统所需的。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 2840ba401c0b7f3df5b0b27726cbdaa05390f952
ms.sourcegitcommit: 442fbe1655d629ecef273b02fae1beb2455a762e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93235264"
---
# <a name="configure-system-center-operations-manager-scom-to-monitor-analytics-platform-system"></a>配置 System Center Operations Manager (SCOM) 以监视分析平台系统
请按照以下步骤配置用于分析平台系统的 System Center Operations Manager (SCOM) 管理包。 管理包是从 SCOM 监视分析平台系统所需的。  
  
## <a name="before-you-begin"></a><a name="BeforeBegin"></a>开始之前  
**先决条件**  
  
必须安装并运行 System Center Operations Manager 2007 R2。  
  
必须安装和配置管理包。 请参阅 [安装 Scom 管理包 &#40;Analytics Platform system&#41;](install-the-scom-management-packs.md) 并 [导入用于 PDW &#40;Analytics 平台系统&#41;的 scom 管理包 ](import-the-scom-management-pack-for-pdw.md)。  
  
## <a name="configure-run-as-profile-in-system-center"></a><a name="ConfigureRunAsProfile"></a>在 System Center 中配置 Run-As 配置文件  
若要配置 System Center，必须执行以下步骤：  
  
-   为 **Ap 观察** 程序域用户创建运行方式帐户，并将其映射到 **Microsoft Ap 观察程序帐户。**  
  
-   为 **monitoring_user** 的 ap 用户创建运行方式帐户，并将其映射到 **Microsoft ap 操作帐户** 。  
  
下面是有关如何执行这些任务的详细说明：  
  
1.  为 **Ap 观察** 程序域用户创建包含 **Windows** 帐户类型的 **ap 观察** 程序运行方式帐户。  
  
    1.  导航到 " **管理** " 窗格，右键单击 " **运行方式配置**  ->  **帐户** "，然后选择 " **创建运行方式帐户 ...** "  
  
        ![显示 "创建运行方式帐户" 选项的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/ConfigureScomCreateRunAsAccount.png "ConfigureScomCreateRunAsAccount")  
  
    2.  将打开 " **创建运行方式帐户向导** " 对话框。 在 " **简介** " 页上，单击 " **下一步** "。  
  
    3.  在 " **常规属性** " 页上，从 " **运行方式帐户类型** " 中选择 " **Windows** "，然后指定 "ap 观察程序" 作为 **显示名称** 。  
  
        ![显示 "创建运行方式帐户向导" 的 "常规属性" 页的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/CreateRunAsAccountWizardGeneralProperties.png "CreateRunAsAccountWizardGeneralProperties")  
  
    4.  " **凭据** " 页上 ![显示 "创建运行方式帐户向导" 的 "凭据" 页的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/CreateRunAsAccountWizardCredentials.png "CreateRunAsAccountWizardCredentials")  
  
    5.  在 " **分发安全性** " 页上，选择 " **不安全** "，然后单击 " **创建** " 按钮完成操作。  
  
        ![显示 "创建运行方式帐户向导" 的 "分发安全性" 页的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/CreateRunAsAccountWizardDistributionSecurity.png "CreateRunAsAccountWizardDistributionSecurity")  
  
        1.  如果你决定使用 " **更安全** " 选项，则必须手动指定将向其分发凭据的计算机。 为此，请在创建运行方式帐户后，右键单击它并选择 " **属性** "。  
  
        2.  导航到 " **分发** " 选项卡并 **添加** 所需的计算机。  
  
            ![显示 "运行方式帐户属性" 对话框的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/RunAsAccountProperties.png "RunAsAccountProperties")  
  
2.  设置 **MICROSOFT ap** 观察程序帐户配置文件以使用 **ap 观察** 程序运行方式帐户。  
  
    1.  导航到 " **管理** " "  ->  **运行方式配置** " 配置  ->  **文件** 。  
  
        ![显示配置文件选项的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/AdministrationRunAsConfigurationProfiles.png "AdministrationRunAsConfigurationProfiles")  
  
    2.  右键单击列表中的 " **MICROSOFT Ap 观察程序帐户** "，然后选择 " **属性** "。  
  
        ![显示 Properties 选项的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/MicrosoftApsWatcherAccountProperties.png "MicrosoftApsWatcherAccountProperties")  
  
    3.  将打开 " **运行方式配置文件向导** " 对话框。 单击 " **下一步** "，跳过 " **简介** " 页。  
  
    4.  在“常规属性”  页上，单击“下一步”  。  
  
    5.  在 " **运行方式帐户** " 页上，单击 " **添加 ...** " 按钮，然后选择以前创建的 " **Ap 观察** 程序" 运行方式帐户。  
  
        ![显示 "添加运行方式帐户" 对话框的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/RunAsProfileWizardAdd.png "RunAsProfileWizardAdd")  
  
    6.  单击 " **保存** " 完成配置文件分配。  
  
3.  等待 AP 设备发现完成。  
  
    1.  导航到 " **监视** " 窗格，打开 " **SQL Server 设备** "  ->  **Microsoft Analytics Platform System**  ->  **设备** 状态 "视图。  
  
        ![显示 "设备" 选项的屏幕截图。](./media/configure-scom-to-monitor-analytics-platform-system/SqlServerApplianceMicrosoftApsAppliances.png "SqlServerApplianceMicrosoftApsAppliances")  
  
    2.  等待设备显示在列表中。 设备的名称应等于在注册表中指定的名称。 发现完成后，应会看到所有已列出但未被监视的设备。 若要启用监视，请执行后续步骤。  
  
    > [!NOTE]  
    > 当你等待初始设备发现完成时，可以并行完成后续步骤。  
  
4.  创建另一个新的运行方式帐户以查询用于运行状况数据检索的接入点。  
  
    1.  开始创建新的运行方式帐户，如步骤1中所述。  
  
    2.  在 " **常规属性** " 页上，选择 " **基本身份验证** 帐户类型"。  
  
        ![屏幕截图，显示在 "运行方式帐户类型" 下拉列表中选择 "基本身份验证" 的 "创建运行方式帐户向导" 的 "常规属性" 页。](./media/configure-scom-to-monitor-analytics-platform-system/CreateRunAsAccountWizardGeneralProperties2.png "CreateRunAsAccountWizardGeneralProperties2")  
  
    3.  在 " **凭据** " 页上，提供有效的凭据以访问 ap 健康状况状态 dmv。  
  
        ![显示 "创建运行方式帐户向导" 的 "凭据" 页的屏幕截图，其中使用有效的凭据 wo 访问 AP 运行状况状态 Dmv。](./media/configure-scom-to-monitor-analytics-platform-system/CreateRunAsAccountWizardCredentials2.png "CreateRunAsAccountWizardCredentials2")  
  
5.  配置 **MICROSOFT Ap 操作帐户** 配置文件，以对该 ap 实例使用新创建的运行方式帐户。  
  
    1.  根据步骤2中的说明导航到 **MICROSOFT Ap 操作帐户** 属性。  
  
    2.  在 " **运行方式帐户** " 页上，单击 " **添加 ...** "，然后 
    3.  选择新创建的运行方式帐户。  
  
        ![屏幕截图：显示 "添加运行方式帐户" 对话框，其中包含从 "运行方式帐户" 下拉列表中选择的 "AP" 操作。](./media/configure-scom-to-monitor-analytics-platform-system/RunAsProfileWizardAdd2.png "RunAsProfileWizardAdd2")  
  
## <a name="next-step"></a>下一步  
配置管理包后，便可以开始监视设备了。 有关详细信息，请参阅 [使用 &#40;分析平台系统&#41;监视设备 System Center Operations Manager ](monitor-the-appliance-by-using-system-center-operations-manager.md)。  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
  
