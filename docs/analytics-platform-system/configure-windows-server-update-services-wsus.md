---
title: 配置 WSUS
description: 这些说明将指导你完成使用 Windows Server Update Services (WSUS) 配置向导来配置 WSUS for Analytics 平台系统的步骤。
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: e6242699ac8a4079ddd2811c016ba8a1969e448e
ms.sourcegitcommit: ce15cbbcb0d5f820f328262ff5451818e508b480
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94947943"
---
# <a name="configure-windows-server-update-services-wsus-in-analytics-platform-system"></a>在分析平台系统中配置 Windows Server Update Services (WSUS) 
这些说明将指导你完成使用 Windows Server Update Services (WSUS) 配置向导来配置 WSUS for Analytics 平台系统的步骤。 你需要配置 WSUS，然后才能将软件更新应用于设备。 WSUS 已安装在设备的 VMM 虚拟机上。  
  
有关配置 WSUS 的详细信息，请参阅 WSUS 网站上的 [Wsus 循序渐进安装指南](/windows/deployment/deploy-whats-new) 。 配置 WSUS 后，请参阅 [&#40;Analytics Platform System&#41;下载并应用 Microsoft 更新 ](download-and-apply-microsoft-updates.md) 以启动更新。  
  
> [!WARNING]  
> 如果在此配置过程中遇到任何错误，请停止并联系支持部门以获得帮助。 请不要忽略错误，或在收到错误后继续执行过程。  
  
## <a name="before-you-begin"></a>开始之前  
若要配置 WSUS，需执行以下操作：  
  
-   具有分析平台系统设备的域管理员帐户的登录信息。  
  
-   让一个分析平台系统登录名有权访问 **管理控制台** 并查看设备状态信息。  
  
-   如果你计划从上游 WSUS 服务器同步更新，而不是直接从 Microsoft 更新同步更新，则知道上游 WSUS 服务器的 IP 地址。 请确保将上游 WSUS 服务器设置为允许匿名连接并支持 SSL。  
  
-   如果设备将使用代理服务器访问上游服务器或 Microsoft 更新，则知道代理服务器的 IP 地址。  
  
-   在大多数情况下，WSUS 需要访问设备之外的服务器。 若要支持此使用方案，可将分析平台系统 DNS 配置为支持外部名称转发器，这将允许分析平台系统主机和虚拟机 (Vm) 使用外部 DNS 服务器来解析设备之外的名称。 有关详细信息，请参阅 [使用 DNS 转发器解析非设备 DNS 名称 &#40;分析平台系统&#41;](use-a-dns-forwarder-to-resolve-non-appliance-dns-names.md)。  
  
## <a name="to-configure-windows-server-update-services-wsus"></a> (WSUS 配置 Windows Server Update Services)   
  
1.  登录到 **管理控制台**。 在 " **设备状态** " 选项卡上，验证 " **群集** " 和 " **网络** " 列是否为所有节点显示绿色 (或 **NA**) 。 验证 **设备状态** 中所有节点的状态指示器。  
  
    -   可以安全地继续进行绿色或 NA 指示器。  
  
    -   评估非严重 (黄色) 警告错误。 在某些情况下，警告消息不会阻止更新。 如果非关键磁盘卷错误不在 C：\ 上驱动器，你可以在解决磁盘卷错误之前执行下一步。  
  
    -   在继续操作之前，必须解决大多数红色指示器。 如果出现磁盘故障，请使用 **管理控制台** 的 "警报" 页来验证每个服务器或 SAN 阵列内是否不存在多个磁盘故障。 如果每个服务器或 SAN 阵列中不存在多个磁盘故障，则可以在修复磁盘故障之前继续下一步。 请确保与 Microsoft 支持部门联系，尽快修复磁盘故障。  
  
2.  以设备域管理员身份登录到 VMM 虚拟机。  
  
3.  启动配置向导。  
  
    #### <a name="to-launch-the-configuration-wizard"></a>启动配置向导  
  
    1.  在 **服务器管理器仪表板** 中的 " **工具** " 菜单上，单击 " **Windows Server Update Services**"。  
  
    2.  在 "**更新服务**" 窗口的左窗格中，单击以展开虚拟机管理节点服务器 (**_appliance_domain_ VMM**) ，然后单击 "**选项**"。  
  
    3.  在 **选项** 窗格中，单击 " **WSUS 服务器配置向导** " 以启动配置向导。  
  
        ![Server 管理器面板菜单](./media/configure-windows-server-update-services-wsus/WSUS_Wiz0.png "WSUS_Wiz0")  
  
    4.  如果这是你第一次运行 WSUS 向导，系统可能会要求你配置用于存储更新的目录。 `C:\wsus` 是合适的位置;不过，您可以提供不同的路径。  
  
        ![WSUS 路径](./media/configure-windows-server-update-services-wsus/WSUS_Wiz1.png "WSUS_Wiz1")  
  
    5.  在完成该向导之前，请先查看要完成的项目列表， **然后再** 查看这些项目。  
  
        ![WSUS 开始之前](./media/configure-windows-server-update-services-wsus/WSUS_Wiz2.png "WSUS_Wiz2")  
  
    6.  在 " **加入 Microsoft 更新改善计划** " 页上，选择 **"是，我想加入 Microsoft 更新改善计划**"，然后单击 " **下一步**"。  
  
        ![WSUS 改进计划](./media/configure-windows-server-update-services-wsus/WSUS_Wiz3.png "WSUS_Wiz3")  
  
    现在应会看到 " **选择上游服务器** " 页。 下面的屏幕截图是配置向导的起始点。  
  
    ![WSUS 上游服务器同步](./media/configure-windows-server-update-services-wsus/WSUS_Wiz4a.png "WSUS_Wiz4a")  
  
4.  选择上游服务器。  
  
    在 WSUS 配置向导的 " **选择上游服务器** " 页上，你将选择 "虚拟机管理" 节点上的 WSUS 将如何连接到上游服务器来获取软件更新。 你的两个选项是将上游服务器与 [Microsoft 更新](https://go.microsoft.com/fwlink/?LinkId=133349) 同步或与另一个 Windows Server Update Services 服务器同步更新。  
  
    #### <a name="to-update-by-using-microsoft-update"></a>使用 Microsoft 更新更新  
  
    1.  如果选择与 Microsoft 更新同步，则不需要对 " **选择上游服务器** " 页进行任何更改。 单击“下一步”。  
  
        ![WSUS 上游服务器同步](./media/configure-windows-server-update-services-wsus/WSUS_Wiz4a.png "WSUS_Wiz4a")  
  
    #### <a name="to-update-from-another-wsus-server"></a>从另一台 WSUS 服务器更新  
  
    1.  如果选择与 Microsoft 更新 (上游服务器) 之外的源进行同步，请指定服务器 (输入 IP 地址) 以及此服务器将与上游服务器通信的端口。  
  
        ![WSUS 上游服务器从 WSUS 同步](./media/configure-windows-server-update-services-wsus/WSUS_Wiz4b.png "WSUS_Wiz4b")  
  
    2.  若要使用安全套接字层 (SSL) ，请选中 " **同步更新信息时使用 SSL** " 复选框。 在这种情况下，服务器将使用端口443进行同步。  
  
        ![WSUS 上游服务器从 WSUS SSL 同步](./media/configure-windows-server-update-services-wsus/WSUS_Wiz4c.png "WSUS_Wiz4c")  
  
    3.  如果这是副本服务器，请选择 **“这是上游服务器的副本”** 复选框。 **在同步更新信息时**，可以选择使用 SSL，**这是上游服务器的副本**。  
  
        ![WSUS 上游服务器副本](./media/configure-windows-server-update-services-wsus/WSUS_Wiz4d.png "WSUS_Wiz4d")  
  
    4.  此时，已完成上游服务器配置。 单击 " **下一步**"，或在左侧导航窗格中选择 " **指定代理服务器** "。  
  
5.  指定代理服务器。  
  
    如果此服务器需要代理服务器来访问 Microsoft 更新或其他上游服务器，你可以在此处配置代理服务器设置;否则，单击 " **下一步**"。  
  
    ![WSUS 代理](./media/configure-windows-server-update-services-wsus/WSUS_Wiz5a.png "WSUS_Wiz5a")  
  
    #### <a name="to-configure-proxy-server-settings"></a>配置代理服务器设置  
  
    1.  在配置向导的 " **指定代理服务器** " 页上，选中 " **同步时使用代理服务器** " 复选框，然后在对应的框中键入代理服务器 IP 地址 (不 (端口) 80) 和端口号。  
  
    2.  如果你希望通过使用特定用户凭据来连接代理服务器，请选择 **“使用用户凭据连接代理服务器”** 复选框，然后在对应的框中键入用户名称、域和用户密码。 如果要为连接到代理服务器的用户启用基本身份验证，请选中 " **允许基本身份验证 (密码以明文形式发送)** " 复选框。  
  
        ![WSUS 代理凭据](./media/configure-windows-server-update-services-wsus/WSUS_Wiz5b.png "WSUS_Wiz5b")  
  
    3.  此时，你已完成代理服务器配置。 单击 **“下一步”** 转到下一页，这时你可以开始设置同步进程。  
  
6.  开始连接。  
  
    ![WSUS 代理开始连接](./media/configure-windows-server-update-services-wsus/WSUS_Wiz6.png "WSUS_Wiz6")  
  
    单击 " **开始连接**"。  
  
    成功连接后，单击 " **下一步** " 以前往下一页，你可以在其中选择语言。  
  
7.  选择语言。  
  
    选择 **"仅下载这些语言的更新"**。  
  
    选择 " **英语**"，然后单击 " **下一步**"。  
  
    ![选择语言](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSChooseLanguages.png "SQL_Server_PDW_WSUSChooseLanguages")  
  
8.  选择 "产品"。  
  
    > [!NOTE]  
    > 如果使用的是上游服务器，则可能无法选择 "产品"。 如果此选项不可用，请跳过此步骤。

    > [!WARNING]  
    > 请排除任何 SQL Server 2016 更新。
  
    取消选择所有选定的更新。  
  
    选择 **SQL Server 2012**、 **SQL Server 2014**、 **Windows Server 2012 R2**、 **System Center 2012 R2-Virtual Machine Manager**、 **Windows Server 2016** 和 **System center 2016-Virtual Machine Manager** ，然后单击 " **下一步**"。  
  
9. 选择 "分类"。  
  
    > [!NOTE]  
    > 如果使用的是上游服务器，则可能无法选择 "分类"。  如果此选项不可用，请跳过此步骤。  
  
    取消选择以前选择的所有更新。  
  
    选择 " **关键更新**"、" **安全更新** " 和 " **更新汇总** " 获取将为 Analytics Platform System 设备同步的更新，然后单击 " **下一步**"。  
  
    ![选择分类](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSChooseClassifications.png "SQL_Server_PDW_WSUSChooseClassifications")  
  
10. 配置同步计划。  
  
    选择 " **手动同步**"，然后单击 " **下一步**"。  
  
    ![设置同步计划](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSSyncSchedule.png "SQL_Server_PDW_WSUSSyncSchedule")  
  
11. 开始初始同步。  
  
    选择 " **开始初始同步**"，然后单击 " **下一步**"。  
  
12. 完成。  
  
    单击“完成”。  
  
## <a name="group-the-appliance-servers-in-wsus"></a><a name="bkmk_WSUSGroup"></a>在 WSUS 中将设备服务器分组  
配置 WSUS for Analytics 平台系统后，下一步是对设备服务器进行分组。 通过将所有设备服务器添加到组中，WSUS 将能够将软件更新应用于设备中的所有服务器。  
  
> [!NOTE]  
> WSUS 系统设计为以异步方式运行。 启动活动并不总是导致立即更新。 因此，你可能需要等待一段时间，直到计算机在 WSUS 对话框中可见。 运行 `setup.exe /action=ReportMicrosoftUpdateClientStatus /DomainAdminPassword="<password>"` [下载和应用 Microsoft 更新 &#40;Analytics Platform System&#41;](download-and-apply-microsoft-updates.md) 主题结尾所述的命令有助于刷新对话框。  
  
#### <a name="to-group-the-appliance-servers"></a>对设备服务器进行分组  
  
1.  打开 WSUS 控制台，右键单击 " **所有计算机** "，然后单击 " **添加计算机组**"。  
  
    ![添加计算机组。](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSAddComputerGroup.png "SQL_Server_PDW_WSUSAddComputerGroup")  
  
2.  输入计算机组的名称 "AP"，然后单击 " **添加**"。  
  
    ![为您的新计算机组输入名称。](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSSpecifyGroupName.png "SQL_Server_PDW_WSUSSpecifyGroupName")  
  
3.  再次单击 " **所有计算机** "，将 " **状态** " 下拉菜单中的状态更改为 " **任意**"，然后单击 " **刷新**"。 若要查看刚添加的新组，你可能需要通过在左侧的树控件上单击来展开 **所有计算机** 。  
  
    ![将“状态”更改为“任意”并单击“刷新”。](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSChangeStatusToAny.png "SQL_Server_PDW_WSUSChangeStatusToAny")  
  
4.  选择作为设备一部分的所有计算机，右键单击，然后单击 " **更改成员身份**"。  
  
    ![更改所有 PDW 计算机的成员身份。](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSChangeMembership.png "SQL_Server_PDW_WSUSChangeMembership")  
  
5.  通过单击复选框，然后单击 **"确定**"，选择你创建的新计算机组。  
  
    ![设置计算机组成员身份](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSSetComputerGroupMembership.png "SQL_Server_PDW_WSUSSetComputerGroupMembership")  
  
6.  选择新计算机组，将其 **状态** 更改为 " **任意**"，然后单击 " **刷新**"。 现在所有计算机都应分配到此组，并在右窗格中列出。 当节点显示警告（例如 **此节点尚未报告状态**）时，通常可以安全地继续操作。  
  
    ![将状态更改为 "任意"，然后单击 "刷新"。](./media/configure-windows-server-update-services-wsus/SQL_Server_PDW_WSUSChangeStatusAnyRefresh.png "SQL_Server_PDW_WSUSChangeStatusAnyRefresh")  
