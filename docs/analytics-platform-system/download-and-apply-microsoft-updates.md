---
title: 下载 Microsoft 更新
description: 本主题讨论如何将更新从 Microsoft 更新目录下载到 Windows Server Update Services (WSUS) 并将这些更新应用到分析平台系统设备服务器。 Microsoft 更新将安装适用于 Windows 和 SQL Server 的所有适用更新。 WSUS 安装在设备的 VMM 虚拟机上。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: e5f336c3c2c475523d2081bcf01189e67b77fe19
ms.sourcegitcommit: ce15cbbcb0d5f820f328262ff5451818e508b480
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94947912"
---
# <a name="download-and-apply-microsoft-updates-for-analytics-platform-system"></a>下载并应用 Microsoft 更新以进行分析平台系统
本主题讨论如何将更新从 Microsoft 更新目录下载到 Windows Server Update Services (WSUS) 并将这些更新应用到分析平台系统设备服务器。 Microsoft 更新将安装适用于 Windows 和 SQL Server 的所有适用更新。 WSUS 安装在设备的 VMM 虚拟机上。  
  
## <a name="before-you-begin"></a><a name="TOP"></a>开始之前  
  
> [!WARNING]  
> 如果设备或任何设备组件关闭或处于故障转移状态，请不要尝试应用更新。 在这种情况下，请联系支持人员获取帮助。  
>   
> 请勿在设备使用时应用 Microsoft 更新。 应用更新可能会导致装置节点重新启动。 如果未使用设备，则应在维护时段内应用这些更新。  
  
### <a name="prerequisites"></a>先决条件  
执行这些步骤之前，需要：  
  
-   按照 [配置 Windows Server Update Services &#40;wsus&#41; &#40;分析平台系统&#41;](configure-windows-server-update-services-wsus.md)中的说明，在设备上配置 wsus。  
  
-   了解 Fabric 域管理员帐户的登录信息。  
  
-   具有一个登录名，该登录名具有访问分析平台系统管理控制台的权限并查看设备状态信息。  
  
-   在大多数情况下，WSUS 需要访问设备之外的服务器。 若要支持此使用方案，可将分析平台系统 DNS 配置为支持外部名称转发器，这将允许分析平台系统主机和虚拟机 (Vm) 使用外部 DNS 服务器来解析设备之外的名称。 有关详细信息，请参阅 [使用 DNS 转发器解析非设备 DNS 名称 &#40;分析平台系统&#41;](use-a-dns-forwarder-to-resolve-non-appliance-dns-names.md)。  
  
## <a name="to-download-and-apply-microsoft-updates"></a><a name="bkmk_ImportUpdates"></a>下载并应用 Microsoft 更新  
  
#### <a name="verify-the-appliance-state-indicators"></a>验证设备状态指示器  
  
1.  打开管理控制台并导航到 "设备状态" 页。 有关详细信息，请参阅 [使用管理控制台监视设备 &#40;Analytics Platform System&#41;](monitor-the-appliance-by-using-the-admin-console.md)  
  
2.  验证设备状态中所有节点的状态指示器。  
  
    -   可以安全地继续进行绿色或 NA 指示器。  
  
    -   评估非严重 (黄色) 警告错误。 在某些情况下，警告消息不会阻止更新。 如果非关键磁盘卷错误不在 C：\ 上驱动器，你可以在解决磁盘卷错误之前执行下一步。  
  
    -   在继续操作之前，必须解决大多数红色指示器。 如果出现磁盘故障，请使用管理控制台的 "警报" 页来验证每个服务器或 SAN 阵列内是否不存在多个磁盘故障。 如果每个服务器或 SAN 阵列中不存在多个磁盘故障，则可以在修复磁盘故障之前继续下一步。 请确保与 Microsoft 支持部门联系，尽快修复磁盘故障。  
  
#### <a name="synchronize-the-wsus-server"></a>同步 WSUS 服务器  
  
1.  以域管理员身份登录到 VMM 虚拟机。  
  
2.  在 **服务器管理器仪表板** 中的 "**工具**" 菜单上，单击 " **Windows Server Update Services** " **)  (""。**  
  
3.  在 WSUS 管理控制台中，单击 " **同步**"。  
  
4.  如果同步未运行，请单击右窗格中的 " **立即同步** "。 在底部窗格中，将会出现同步状态。 等待同步完成。  
  
#### <a name="approve-microsoft-updates-in-wsus"></a>在 WSUS 中批准 Microsoft 更新  
  
1. 拒绝不在 **System Center** 中的任何更新汇总。

2. 在 WSUS 控制台的左窗格中，单击 " **所有更新**"。  
  
3.  在 " **所有更新** " 窗格中，单击 " **审批** " 下拉菜单，将 " **批准** " 设置为 "已 **拒绝**"。 单击 " **状态** " 下拉菜单，将 " **状态** " 设置为 " **任何**"。 单击“刷新”。  
  
    右键单击 " **标题** " 列，然后选择 " **文件状态** " 以在下载完成后验证文件状态。  
  
    你还可以在左窗格中选择 " **关键更新** " 或 " **安全更新** "，并查看这些类别的可用更新。  
  
    ![选择所有更新并将状态更改为“任意”。](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectAllUpdates.png "SQL_Server_PDW_WSUSSelectAllUpdates")  
  
4.  选择 "所有更新"，然后单击右窗格中的 " **批准** " 链接。  
  
    你还可以右键单击所选的更新，然后单击 " **批准**"。 系统可能会提示你接受 "Microsoft 软件许可条款"。 如果是这样，请在窗口中单击 " **我接受** " 以继续。  
  
    ![选择所有适用的更新并单击“批准”。](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectApprove.png "SQL_Server_PDW_WSUSSelectApprove")  
  
5.  选择在 " [配置 Windows Server Update Services &#40;WSUS&#41; &#40;Analytics 平台系统"&#41;](configure-windows-server-update-services-wsus.md)中创建的设备服务器组。  
  
6.  单击 **“批准安装”** ，然后单击 **“确定”** 。  
  
    ![批准为您的计算机组更新。](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSSelectApprovalType.png "SQL_Server_PDW_WSUSSelectApprovalType")  
  
7.  当审批过程完成时，在 " **批准进度** " 对话框中，单击 " **关闭**"。  
  
    ![批准更新时关闭窗口。](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUSCloseApprovalProgressWindow.png "SQL_Server_PDW_WSUSCloseApprovalProgressWindow")  
  
#### <a name="verify-that-the-updates-are-in-wsus"></a>验证更新是否在 WSUS 中  
  
-  验证所有更新的文件状态。 每个文件都需要标题左侧有一个绿色箭头图标。 这表示该文件已准备好安装。  
  
    ![文件状态为成功](./media/download-and-apply-microsoft-updates/SQL_Server_PDW_WSUS_File_Status.png "SQL_Server_PDW_WSUS_File_Status")  
  
    在安装更新之前，请确保它们都已下载并在 WSUS 控制台中可用。  
  
#### <a name="to-verify-that-all-updates-are-downloaded"></a>验证是否下载了所有更新  
  
-  如以下屏幕截图所示，在 WSUS 控制台中查看更新的 **下载状态** 。 检查 **需要文件的更新** 是否为0以确认下载了所有更新。 如果此数字大于零，你可能需要返回并批准其他更新。  
  
    ![验证所有更新是否均已下载。](./media/download-and-apply-microsoft-updates/SQL_Server_PDS_WSUS_VerifyDownloadUpdateJPG.png "SQL_Server_PDS_WSUS_VerifyDownloadUpdateJPG")  
  
#### <a name="apply-microsoft-updates"></a>应用 Microsoft 更新  
  
1.  在开始之前，请 [使用管理控制台 &#40;分析平台系统&#41;打开监视设备](monitor-the-appliance-by-using-the-admin-console.md)，单击 " **设备状态** " 选项卡，并确认 " **群集** " 和 " **网络** " 列显示所有节点的绿色 (或 NA) 。 如果任一列中存在任何警报，则设备可能无法正确安装更新。 在继续之前，请解决 **群集** 和 **网络** 列中的所有现有警报。  
  
2.  以 Fabric 域管理员身份登录到 _<domain_name>_ **-HST01** 节点。  
  
3.  若要应用所有针对 WSUS 批准的更新，请运行更新程序。 有关说明，请参阅下面 [的运行更新程序](#RunUpdateWizard) 。  
  
#### <a name="verify-the-updates-on-all-nodes"></a>验证所有节点上的更新  
  
1.  在 VMM 节点中，启动 WSUS 管理控制台。 此应用程序可在 " **启动**"、" **管理工具**"、" **Windows Server Update Services**" 下找到。  
  
2.  展开 " **计算机**"。  
  
3.  展开 " **所有计算机**"。  
  
4.  选择在 " [配置 Windows Server Update Services &#40;WSUS&#41; &#40;Analytics 平台系统"&#41;](configure-windows-server-update-services-wsus.md)中创建的设备服务器组。  
  
5.  在 " **状态** " 下拉菜单中，选择 " **任意** "，然后单击 " **刷新**"。  
  
6.  展开 " **更新服务**" *<appliance name>* -VMM、" **更新**" 和 " **所有更新**"，其中 *<appliance name>* 是你的设备名称。  
  
7.  在 " **所有更新** " 窗口中，将 **审批** 设置为 **除拒绝之外的任何** 项。  
  
8.  在 " **所有更新** " 窗口中，将 " **状态** " 设置为 " **失败或需要**"。  
  
9. 单击“刷新”。  
  
10. 如果 **需要的更新** 大于零，请联系支持人员以获得帮助。  
  
#### <a name="ensure-there-are-no-critical-alerts-in-the-sql-server-pdw-admin-console"></a>确保 SQL Server PDW 管理控制台中没有严重警报  
  
1.  打开管理控制台，单击 "设备状态" 选项卡。请参阅 [使用管理控制台 &#40;分析平台系统&#41;来监视设备 ](monitor-the-appliance-by-using-the-admin-console.md)。  
  
2.  验证 **群集** 和 **网络** 列是否为所有节点显示绿色 (或 NA) 。 如果任一列中存在任何警报，则设备可能无法正确安装更新。 如果有任何严重警报，请联系支持人员。  
  
## <a name="run-the-update-program"></a><a name="RunUpdateWizard"></a>运行更新程序  
按照以下说明运行分析平台系统更新程序。  
  
> [!NOTE]  
> WSUS 系统设计为异步运行可能需要一些时间来完全应用所有更新。 启动更新会计划更新，但不能保证立即更新活动。  
  
1.  请确保以 Fabric 域管理员身份登录到 HST01 节点。  
  
2.  打开命令提示符窗口并输入以下命令。 替换 *<parameter>* 为指定的信息。  
  
**运行 Microsoft 更新：**  
  
```  
C:\pdwinst\media\setup.exe /action="MicrosoftUpdate" /DomainAdminPassword="<password>"  
```  
  
**报告 Microsoft 更新状态：**  
  
```  
C:\pdwinst\media\setup.exe /action="ReportMicrosoftUpdateClientStatus" /DomainAdminPassword="<password>"  
```  
  
## <a name="see-also"></a>另请参阅  
[&#40;Analytics Platform System&#41;卸载 Microsoft 更新 ](uninstall-microsoft-updates.md)  
[&#40;Analytics Platform System&#41;应用分析平台系统修补程序 ](apply-analytics-platform-system-hotfixes.md)  
[&#40;Analytics Platform System&#41;卸载分析平台系统修补程序 ](uninstall-analytics-platform-system-hotfixes.md)  
[软件服务 &#40;分析平台系统&#41;](software-servicing.md)  
  
