---
description: 创建自定义工作流 (Master Data Services)
title: 创建自定义工作流
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: 8e4403e9-595c-4b6b-9d0c-f6ae1b2bc99d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ce80f26d1df3a1059a94881889cfbc566c1a5412
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350081"
---
# <a name="create-a-custom-workflow-master-data-services"></a>创建自定义工作流 (Master Data Services)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 使用业务规则创建基本工作流解决方案，如自动更新和验证数据，并根据指定的条件发送电子邮件通知。 当您需要的处理比内置工作流操作提供的处理更复杂时，请使用自定义工作流。 自定义工作流是您创建的 .NET 程序集。 在调用您的工作流程序集时，您的代码可以执行您的情况所需要的任何操作。 例如，如果您的工作流要求复杂的事件处理（如多层审批或复杂的决策树），您可以配置 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 来启动自定义工作流，以便分析数据和确定将其发送到何处以待审批。  
  
## <a name="how-custom-workflows-are-processed"></a>如何处理自定义工作流  
 有三个涉及处理自定义工作流的主要组件：[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] Web 应用程序、SQL Server MDS Workflow Integration Service 和工作流处理程序程序集。 这些组件按如下所示处理自定义工作流：  
  
1.  您使用 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 来验证启动工作流的实体。  
  
2.  [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 将满足业务规则条件的成员发送到 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库中的 Service Broker 队列。  
  
3.  SQL Server MDS Workflow Integration Service 定期调用 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库中的存储过程。  
  
4.  当此存储过程发现 Service Broker 队列中的记录时，它将这些记录返回到 SQL Server MDS Workflow Integration Service。  
  
5.  SQL Server MDS Workflow Integration Services 将数据路由到您的工作流处理程序程序集。  
  
> [!NOTE]  
>  注意：SQL Server MDS Workflow Integration Service 旨在触发简单的过程。 如果您的自定义代码需要复杂处理，请在单独的线程中或工作流进程之外完成您的处理。  
  
## <a name="configure-master-data-services-for-custom-workflows"></a>为自定义工作流配置 Master Data Services  
 创建自定义工作流需要编写一些自定义代码，并将 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 配置为将工作流数据传递给工作流处理程序。 请按照以下步骤启用自定义工作流处理：  
  
1.  创建一个实现 [IWorkflowTypeExtender] 的 .NET 程序集 (/previous-versions/sql/sql-server-2016/hh758785 (v = MasterDataServices) 。  
  
2.  配置 SQL Server MDS Workflow Integration Service 以连接您的 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库，并将标记与您的工作流处理程序关联。  
  
3.  启动 SQL Server MDS Workflow Integration Service。  
  
4.  在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 中创建业务规则，以启动标记了工作流处理程序名称的工作流。  
  
5.  将此业务规则应用到成员，以触发您的自定义工作流。  
  
### <a name="create-the-workflow-handler-assembly"></a>创建工作流处理程序程序集  
 自定义工作流是实现 [IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) 接口的 .net 类库程序集。 SQL Server MDS Workflow Integration Service 调用 [WorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130))  ，以运行你的代码。 有关实现 [MasterDataServices](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130)) 的示例代码，请参阅 [Master Data Services&#41;&#40;自定义工作流示例 ](../../master-data-services/develop/create-a-custom-workflow-example.md)。  
  
 按照以下步骤使用 Visual Studio 2010 创建 SQL Server MDS Workflow Integration Service 可以调用来处理自定义工作流的程序集：  
  
1.  在 Visual Studio 2010 中，创建使用所选语言的新“类库”项目。 若要创建 C# 类库，请选择“Visual C#\Windows”项目类型，并选择“类库”模板。 输入项目名称，如“MDSWorkflowTest”，然后单击“确定”。  
  
2.  将引用添加到 Microsoft.MasterDataServices.WorkflowTypeExtender.dll。 此程序集可在 \<Your installation folder> \Master Data services\webapplication\bin 中找到。  
  
3.  将“using Microsoft.MasterDataServices.Core.Workflow;”添加到 C# 代码文件。  
  
4.  继承类声明中的[MasterDataServices。](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) 该类声明应类似于：“public class WorkflowTester : IWorkflowTypeExtender”。  
  
5.  实现 [MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender](/previous-versions/sql/sql-server-2016/hh758785(v=sql.130)) 接口。 SQL Server MDS Workflow Integration Service 调用 [WorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130))  方法来启动工作流。  
  
6.  将程序集复制到 \<Your installation folder> \Master Data services\webapplication\bin 中名为 Microsoft.MasterDataServices.Workflow.exe 的 SQL SERVER MDS Workflow Integration Service 可执行文件的位置。  
  
### <a name="configure-sql-server-mds-workflow-integration-service"></a>配置 SQL Server MDS Workflow Integration Service  
 按照如下步骤编辑 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 配置文件以包含您的 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库的连接信息，并将标记与您的工作流处理程序程序集关联：  
  
1.  在 \<Your installation folder> \Master Data services\webapplication\bin 中查找 Microsoft.MasterDataServices.Workflow.exe.config  
  
2.  将 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库连接信息添加为“ConnectionString”设置。 如果您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装使用区分大小写的排序规则，则必须以与数据集中相同的大小写输入数据库的名称。 例如，完整设置标记可能类似以下形式：  
  
    ```xml  
    <setting name="ConnectionString" serializeAs="String">  
        <value>Server=myServer;Database=myDatabase;Integrated Security=True</value>  
    </setting>  
    ```  
  
3.  在“ConnectionString”设置下，添加“WorkflowTypeExtenders”设置以将标记名称与工作流处理程序程序集关联。 例如：  
  
    ```xml  
    <setting name="WorkflowTypeExtenders" serializeAs="String">  
        <value>TEST=MDSWorkflowTestLib.WorkflowTester, MDSWorkflowTestLib</value>  
    </setting>  
    ```  
  
     标记的内部文本 \<value> 的格式为 \<Workflow tag> = \<assembly-qualified workflow type name> 。 \<Workflow tag> 是在中创建业务规则时用于标识工作流处理程序程序集的名称 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 。 \<assembly-qualified workflow type name> 是工作流类的命名空间限定名称，后跟一个逗号，然后是程序集的显示名称。 如果您的程序集采用强名称，则还必须包括版本信息及其 PublicKeyToken。 \<setting>如果已为不同类型的工作流创建多个工作流处理程序，则可以包含多个标记。  
  
> [!NOTE]  
>  根据服务器的配置，在尝试保存 Microsoft.MasterDataServices.Workflow.exe.config 文件时，可能会显示一条“拒绝访问”错误。 如果发生这种情况，则暂时禁用服务器上的用户帐户控制 (UAC)。 为此，打开“控制面板”，单击“系统与安全”。 在“操作中心”下，单击“更改用户帐户控制设置”。 在“用户帐户控制设置”对话框中，将滑动条移动到底部以不再接收通知。 重新启动您的计算机，然后重复前面的步骤以编辑您的配置文件。 保存该文件之后，将您的 UAC 设置重置为默认级别。  
  
### <a name="start-sql-server-mds-workflow-integration-service"></a>启动 SQL Server MDS Workflow Integration Service  
 默认情况下，不安装 SQL Server MDS Workflow Integration Service。 您必须先安装此服务，才能使用它。 为获得最大安全性，为该服务创建一个本地用户，并仅为该用户授予执行工作流操作所需的权限。 若要创建用户，请按照以下步骤安装此服务，然后启动该服务：  
  
1.  使用“本地用户和组”管理器创建名为（例如，mds_workflow_service）的本地用户。  
  
2.  使用 SQL Server Management Studio 向 mds_workflow_service 用户授予执行 [mdm].[udpExternalActionsGet] 存储过程的权限。 为此，请为 mds_workflow_service 帐户创建一个新的登录名，并在 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库中创建一个新用户，然后将此用户映射为 mds_workflow_service 登录名，为用户授予对 [mdm].[udpExternalActionsGet] 存储过程的 EXECUTE 权限。  
  
3.  向 mds_workflow_service 用户授予执行工作流处理程序程序集的权限。 为此，将 mds_workflow_service 用户添加到工作流处理程序程序集“属性”的“安全性”选项卡，然后向 mds_workflow_service 用户授予 READ 权限和 EXECUTE 权限。  
  
4.  向 mds_workflow_service 用户授予执行 SQL Server MDS Workflow Integration Service 可执行文件的权限。 为此，请将 mds_workflow_service 用户添加到 Microsoft.MasterDataServices.Workflow.exe 的 **属性** 的 "**安全**" 选项卡中的 " \<Your installation folder> \Master Data Services\WebApplication\bin"，并向 mds_workflow_service 授予 "读取" 和 "执行" 权限。  
  
5.  通过使用 .NET 安装实用工具（名为 InstallUtil.exe）来安装 SQL Server MDS Workflow Integration Service。 InstallUtil.exe 位于 .NET 安装文件夹中，如 C:\Windows\Microsoft.NET\Framework\v4.0.30319\\。 通过在提升的命令提示符中输入以下命令安装 SQL Server MDS Workflow Integration Service：  
  
    ```  
    C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil Microsoft.MasterDataServices.Workflow.exe  
    ```  
  
     在安装过程中出现提示时，指定 mds_workflow_service 用户。  
  
6.  通过使用“服务”管理单元来启动 SQL Server MDS Workflow Integration Service。 为此，在服务管理单元中找到 SQL Server MDS Workflow Integration Service，然后单击“开始”链接。  
  
### <a name="create-a-workflow-business-rule"></a>创建工作流业务规则  
 在应用时，使用 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 来创建和发布将启动工作流的业务规则。 应确保业务规则包含更改属性值的操作，以便该规则应用一次后，其计算结果为 False。 例如，当 Price 属性值大于 500 且 Approved 属性值为空时，您的业务规则可能计算为 True。 该规则可能包含两个操作：一个操作是将 Approved 属性值设置为“挂起”，另一个操作是启动工作流。 或者，建议创建一个使用“已更改”条件的规则，并将属性添加到更改跟踪组。 有关业务规则的详细信息，请参阅[业务规则 &#40;Master Data Services&#41;](../../master-data-services/business-rules-master-data-services.md)。  
  
 按照以下步骤在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 中创建用于启动自定义工作流的业务规则：  
  
1.  在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 的业务规则编辑器中，指定了业务规则条后，将“启动工作流”操作从“外部操作”列表拖动到“THEN”窗格的“操作”标签。  
  
2.  在“编辑操作”窗格中的“工作流类型”框中，键入标识工作流处理程序程序集的标记。 这是您在您的程序集的配置文件中指定的标记，例如 TEST。  
  
3.  或者，选中“包含成员数据”复选框。 选中此复选框可以在 XML 中包括传递到工作流处理程序的的属性名称和值。  
  
4.  在“工作流站点”框中，键入网站名称。 对于您的自定义工作流，此方法不适用，但可用于添加的内容。  
  
5.  在“工作流名称”框中，键入来自 Visual Studio 的工作流名称。 对于您的自定义工作流，此方法不适用，但可用于添加的内容。  
  
6.  保存并发布业务规则。  
  
### <a name="apply-business-rules-to-start-a-workflow"></a>应用业务规则以启动工作流  
 将此业务规则应用于您的数据，以启动工作流。 为此，使用 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 以编辑包含要验证的成员的实体。 单击“应用业务规则”。 为了响应此业务规则，[!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 将填充 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库的 Service Broker 队列。 当 SQL Server MDS Workflow Integration Service 检查该队列时，它将数据发送到指定的工作流处理程序程序集并清除该队列。 工作流处理程序程序集执行已编码到队列中的任何操作。  
  
## <a name="troubleshoot-custom-workflows"></a>排除自定义工作流故障  
 如果工作流处理程序程序集不接收数据，则可以尝试调试 SQL Server MDS Workflow Integration Service 或查看 Service Broker 队列。  
  
### <a name="debug-sql-server-mds-workflow-integration-service"></a>调试 SQL Server MDS Workflow Integration Service  
 若要调试 SQL Server Workflow Integration Service，请执行以下步骤：  
  
1.  使用“服务”管理单元以停止服务。  
  
2.  打开命令提示符，导航到此服务的位置，通过输入 Microsoft.MasterDataServices.Workflow.exe -console 在控制台中运行此服务。  
  
3.  在 [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] 中，更新您的成员，并再次应用业务规则。 控制台窗口中将显示详细日志。  
  
### <a name="view-the-service-broker-queue"></a>查看 Service Broker 队列  
 包含作为工作流一部分传递的主数据的 Service Broker 队列为：mdm.microsoft/mdm/queue/externalaction。 队列位于 [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] 数据库的 Service Broker 节点下的 SQL Management Studio 的“对象资源管理器”中。 请注意，如果此服务已正确清除队列，则此队列将为空。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Master Data Services 的自定义工作流示例&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md)   
 [自定义工作流 XML 说明 &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-xml-description.md)  
  
  
