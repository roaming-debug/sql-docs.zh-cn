---
description: 数据流目标
title: 数据流目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL11.DTS.DESIGNER.DATASTREAMINGDEST.F1
ms.assetid: 640e6a19-49ae-4ee8-ac07-008370158f0e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 34f4ba8e001f43d4c29379dac0de36b595163679
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127225"
---
# <a name="data-streaming-destination"></a>数据流目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  “数据流目标”是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS) 目标组件，它能让 OLE DB Provider for SSIS 将 SSIS 包的输出作为表格结果集使用。 可以创建使用 OLE DB Provider for SSIS 的链接服务器，然后在链接服务器上运行 SQL 查询以显示由 SSIS 包返回的数据。  
  
 在下面的示例中，以下查询从 SSIS 目录的 Power BI 文件夹中的 SSISPackagePublishing 项目中的 Package.dtsx 包返回输出。 此查询使用名为 [Integration Services 的默认链接服务器] 的链接服务器，该服务器会反过来使用新的 OLE DB Provider for SSIS。 该查询包括 SSIS 目录中的文件夹名称、项目名称和包名称。 OLE DB Provider for SSIS 运行你在查询中指定的包，并返回表格结果集。  
  
```sql
SELECT * FROM OPENQUERY([Default Linked Server for Integration Services], N'Folder=Power BI;Project=SSISPackagePublishing;Package=Package.dtsx')  
  
```  
  
## <a name="data-feed-publishing-components"></a>数据馈送发布组件  
 数据馈送发布组件包含以下组件：OLE DB Provider for SSIS、数据流目标和 SSIS 包发布向导。 该向导让你将 SSIS 包作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库实例中的 SQL 视图发布。 该向导帮助你创建使用 OLE DB Provider for SSIS 的链接服务器和表示链接服务器上的查询的 SQL 视图。 你可以运行该视图，从作为表格数据集的 SSIS 包查询结果。  
  
 若要确认 SSISOLEDB 提供程序已安装，在 SQL Server Management Studio 中，展开 **服务器对象**、 **链接服务器**、 **提供程序**，并确认你看到了 **SSISOLEDB** 提供程序。 双击 **SSISOLEDB**，启用“允许进程内”（如果它未启用），然后单击“确定”。  
  
## <a name="publish-an-ssis-package-as-a-sql-view"></a>将 SSIS 包作为 SQL 视图发布  
 以下过程描述将 SSIS 包作为 SQL 视图发布的步骤。  
  
1.  使用 **数据流目标** 组件创建 SSIS 包，并将包部署到 SSIS 目录。  
  
2.  通过运行 C:\Program Files\Microsoft SQL Server\130\DTS\Binn 的 ISDataFeedPublishingWizard.exe 或通过从“开始”菜单运行“数据流发布向导”来运行“SSIS 包发布向导”。  
  
     该向导创建一个使用 OLE DB Provider for SSIS (SSISOLEDB) 的链接服务器，然后在链接服务器上创建一个包含查询的 SQL 视图。 该查询包括 SSIS 目录中的文件夹名称、项目名称和包名称。  
  
3.  在 SQL Server Management Studio 中执行 SQL 视图并从 SSIS 包查看结果。 该视图通过你创建的链接服务器将查询发送到 OLE DB Provider for SSIS。 OLE DB Provider for SSIS 执行你在查询中指定的包，然后返回表格结果集。  
  
> [!IMPORTANT]  
>   有关详细步骤，请参阅 [Walkthrough: Publish an SSIS Package as a SQL View](../../integration-services/data-flow/walkthrough-publish-an-ssis-package-as-a-sql-view.md)。  

## <a name="configure-data-streaming-destination"></a>配置数据流目标
  通过使用“数据流目标高级编辑器”  对话框配置数据流目标。 在数据流设计器中双击该组件或右键单击该组件然后单击“编辑”来打开此对话框。  
  
 此对话框包含三个选项卡：“组件属性” 、“输入列” 和“输入属性和输出属性” 。  
  
## <a name="component-properties-tab"></a>“组件属性”选项卡  
 该选项卡具有以下可编辑字段：  
  
|字段|说明|  
|-----------|-----------------|  
|名称|包中数据流目标组件的名称。|  
|ValidateExternalMetadata|指示设计时是否使用外部数据源对组件进行了验证。 如果设置为 False，对外部数据源的验证将推迟到运行时。|  
|IDColumnName|数据馈送发布向导生成的视图有此附加 ID 列。 当其他应用程序将该数据用作 OData 馈送时，ID 列将作为数据流的输出数据的 EntityKey。<br /><br /> 此列的默认值为 _ID。 可以为 ID 列指定不同名称。|  
  
## <a name="input-columns-tab"></a>“输入列”选项卡  
 在此选项卡的顶部窗格中，你会看到所有可用输入列。 选择你想要在此组件的输出中包含的列。 所选的列会显示在底部窗格的列表中。 可以通过在列表中 **输出别名** 字段中输入新名称来更改输出列名。  
  
## <a name="input-output-properties-tab"></a>“输入属性和输出属性”选项卡  
 类似于“输入列”选项卡，可以在此选项卡中更改输出列的名称。在左侧树视图中，展开“数据流目标输入”  ，然后展开“输入列” 。 在右窗格中单击输入列名称并更改输出列名称。
