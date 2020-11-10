---
description: Integration Services (SSIS) 连接
title: Integration Services (SSIS) 连接 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.asvs.connectionmanager.f1
- sql13.dts.designer.adddtsconnection.f1
helpviewer_keywords:
- Integration Services packages, connections
- SSIS packages, connections
- sources [Integration Services], connections
- packages [Integration Services], connections
- destinations [Integration Services], connections
- tasks [Integration Services], connections
- connections [Integration Services], about connections
- connections [Integration Services]
- SQL Server Integration Services packages, connections
ms.assetid: 72f5afa3-d636-410b-9e81-2ffa27772a8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aac745594f2dcd23974cc91cb0f9c57ebff91f29
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384896"
---
# <a name="integration-services-ssis-connections"></a>Integration Services (SSIS) 连接

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包使用连接来执行不同的任务以及实现 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 功能：  
  
-   连接到源和目标数据存储区（如文本、XML、Excel 工作簿和关系数据库），以提取和加载数据。  
  
-   连接到包含引用数据的关系数据库，以执行完全查找或模糊查找。  
  
-   连接到关系数据库，以运行 SQL 语句（例如，SELECT、DELETE 和 INSERT 命令）以及存储过程。  
  
-   连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以执行维护和传输任务，例如，备份数据库和传输登录名。  
  
-   在文本和 XML 文件中以及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中写入日志项，并将包配置写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
-   连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以创建某些转换在执行其工作时需要的临时工作表。  
  
-   连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目和数据库，以访问数据挖掘模型、处理多维数据集和维度，并运行 DDL 代码。  
  
-   指定现有的文件和文件夹，或创建新的文件和文件夹，以便用于 Foreach 循环枚举器和任务。  
  
-   连接到消息队列、Windows Management Instrumentation (WMI)、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象 (SMO)、Web 和邮件服务器。  
  
 为了创建这些连接， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用了连接管理器，如下一部分所述。  
  
## <a name="connection-managers"></a>连接管理器  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用连接管理器作为连接的逻辑表示形式。 在设计时，可设置连接管理器的属性，以描述当包运行时 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 创建的物理连接。 例如，连接管理器包含在设计时设置的 **ConnectionString** 属性；在运行时，使用该连接字符串属性中的值创建物理连接。  
  
 包可以使用一种连接管理器类型的多个实例，您可以在每个实例上设置这些属性。 在运行时，一种连接管理器类型的每个实例创建具有不同属性的连接。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了不同类型的连接管理器，从而使得包可以连接到多种数据源和服务器：  
  
-   提供了内置连接管理器，在您安装 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]时安装程序将安装这些连接管理器。  
  
-   提供了可从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 网站下载的连接管理器。  
  
-   如果现有的连接管理器没有满足您的需求，可以创建自己的自定义连接管理器。  

### <a name="package-level-and-project-level-connection-managers"></a>包级别和项目级别连接管理器
可以在包级别或项目级别创建连接管理器。 在项目级别创建的连接管理器对项目中的所有包可用。 而在包级别创建的连接管理器对该特定包可用。  
  
 您使用在项目级别创建的连接管理器来替代数据源将连接共享到源。 要添加项目级别的连接管理器， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目必须使用项目部署模型。 将一个项目配置为使用此模型时， **“连接管理器”** 文件夹显示在 **“解决方案资源管理器”** 中，而 **“数据源”** 文件夹则从 **“解决方案资源管理器”** 中删除。  
  
> [!NOTE]  
>  如果您要使用包中的数据源，需要将项目转换为包部署模型。  
>   
>  有关两个模型以及将项目转换为项目部署模型的详细信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。

### <a name="built-in-connection-managers"></a>内置连接管理器  
 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的连接管理器类型。  
  
|类型|说明|主题|  
|----------|-----------------|-----------|  
|ADO|连接到 ActiveX 数据对象 (ADO) 对象。|[ADO 连接管理器](../../integration-services/connection-manager/ado-connection-manager.md)|  
|ADO.NET|使用 .NET 提供程序连接到数据源。|[ADO.NET 连接管理器](../../integration-services/connection-manager/ado-net-connection-manager.md)|  
|CACHE|从数据流或从缓存文件 (.caw) 中读取数据，并可将数据保存到缓存文件。|[缓存连接管理器](../../integration-services/connection-manager/cache-connection-manager.md)|  
|DQS|连接到某一 Data Quality Services 服务器以及该服务器上的 Data Quality Services 数据库。|[DQS 清理连接管理器](../../integration-services/connection-manager/dqs-cleansing-connection-manager.md)|  
|EXCEL|连接到 Excel 工作簿文件。|[Excel 连接管理器](../../integration-services/connection-manager/excel-connection-manager.md)|  
|FILE|连接到文件或文件夹。|[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)|  
|FLATFILE|连接到单个平面文件中的数据。|[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)|  
|FTP|连接到 FTP 服务器。|[FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)|  
|HTTP|连接到 Web 服务器。|[HTTP 连接管理器](../../integration-services/connection-manager/http-connection-manager.md)|  
|MSMQ|连接到消息队列。|[MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)|  
|MSOLAP100|连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目。|[Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)|  
|MULTIFILE|连接到多个文件和文件夹。|[多文件连接管理器](../../integration-services/connection-manager/multiple-files-connection-manager.md)|  
|MULTIFLATFILE|连接到多个数据文件和文件夹。|[多平面文件连接管理器](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|  
|OLEDB|使用 OLE DB 访问接口连接到数据源。|[OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)|  
|ODBC|使用 ODBC 连接到数据源。|[ODBC 连接管理器](../../integration-services/connection-manager/odbc-connection-manager.md)|  
|SMOServer|连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理对象 (SMO) 服务器。|[SMO 连接管理器](../../integration-services/connection-manager/smo-connection-manager.md)|  
|SMTP|连接到 SMTP 邮件服务器。|[SMTP 连接管理器](../../integration-services/connection-manager/smtp-connection-manager.md)|  
|SQLMOBILE|连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact 数据库。|[SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|  
|WMI|连接到服务器，并指定服务器上 Windows Management Instrumentation (WMI) 管理的范围。|[WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)|  
  
### <a name="connection-managers-available-for-download"></a>可供下载的连接管理器  
 下表列出了可从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 网站下载的其他连接管理器类型。  
  
> [!IMPORTANT]  
>  下表中列出的连接管理器只能用于 [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] 和 [!INCLUDE[ssDeveloperEd11](../../includes/ssdevelopered11-md.md)]。  
  
|类型|说明|主题|  
|----------|-----------------|-----------|  
|ORACLE|连接到 Oracle \<version info\> 服务器。|Oracle 连接管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 的连接管理器组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Oracle by Attunity 还包括源和目标。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://www.microsoft.com/download/details.aspx?id=55179)。|  
|SAPBI|连接到 SAP NetWeaver BI 7 版系统。|SAP BI 连接管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 的连接管理器组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for SAP BI 还包括源和目标。 有关详细信息，请访问下载页 [Microsoft SQL Server 2008 功能包](https://www.microsoft.com/download/details.aspx?id=44277)。|  
|TERADATA|连接到 Teradata \<version info\> 服务器。|Teradata 连接管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata by Attunity 的连接管理器组件。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector for Teradata by Attunity 还包括源和目标。 有关详细信息，请访问下载页 [Microsoft Connectors for Oracle and Teradata by Attunity](https://www.microsoft.com/download/details.aspx?id=55179)。|  
  
### <a name="custom-connection-managers"></a>自定义连接管理器  
 您还可以编写自定义连接管理器。 有关详细信息，请参阅 [Developing a Custom Connection Manager](../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-custom-connection-manager.md)。  
  
## <a name="create-connection-managers"></a>创建连接管理器
  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含多种连接管理器以满足连接不同类型的服务器和数据源的任务的需要。 在不同类型的数据存储中提取和加载数据的数据流组件，以及将日志写入服务器、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表或文件的日志提供程序，都使用连接管理器。 例如，具有发送邮件任务的包使用 SMTP 类型的连接管理器来连接到简单邮件传输协议 (SMTP) 服务器。 具有执行 SQL 任务的包可以使用 OLE DB 连接管理器来连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 有关详细信息，请参阅 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)。  
  
 若要在创建新包时自动创建和配置连接管理器，可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导。 该向导还有助于创建和配置使用连接管理器的源和目标。 有关详细信息，请参阅 [Create Packages in SQL Server Data Tools](../../integration-services/create-packages-in-sql-server-data-tools.md)。  
  
 若要手动创建新的连接管理器并将其添加到现有包，可以使用在 **设计器的** “控制流” **、** “数据流” **和** “事件处理程序” **选项卡上出现的** “连接管理器” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 区域。 从 **“连接管理器”** 区域，选择要创建的连接管理器的类型，并使用 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器提供的对话框设置连接管理器的属性。 有关详细信息，请参阅本主题后面的“使用连接管理器区域”部分。  
  
 将连接管理器添加到包之后，可以在任务、Foreach 循环容器、源、转换和目标中使用它。 有关详细信息，请参阅 [Integration Services 任务](../../integration-services/control-flow/integration-services-tasks.md)、[Foreach 循环容器](../../integration-services/control-flow/foreach-loop-container.md)和[数据流](../../integration-services/data-flow/data-flow.md)。  
  
### <a name="using-the-connection-managers-area"></a>使用连接管理器区域  
 可以在 **设计器的** “控制流” **、** “数据流” **或** “事件处理程序” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 选项卡活动时创建连接管理器。  
  
 以下关系图显示 **设计器的** “控制流” **选项卡上的** “连接管理器” [!INCLUDE[ssIS](../../includes/ssis-md.md)] 区域。  
  
 ![具有包的控制流设计器的屏幕快照](../../integration-services/connection-manager/media/samplecontrolflow.gif "具有包的控制流设计器的屏幕快照")    
  
### <a name="32-bit-and-64-bit-providers-for-connection-managers"></a>用于连接管理器的 32 位和 64 位提供程序  
 连接管理器所使用的很多提供程序都有 32 位和 64 位版本。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 设计环境是 32 位环境，设计包时您只能看到 32 位提供程序。 因此，如果还安装了同一个提供程序的 32 位版本，则只能将连接管理器配置为使用特定的 64 位提供程序。  
  
 在运行时，系统将使用正确的版本，即使在设计时指定了 32 位版本的提供程序也没有关系。 即使包运行在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 中，也可以运行 64 位版本的提供程序。  
  
  两种版本的提供程序都有相同的 ID。 若要指定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时是否使用可用的 64 位版本的提供程序，需要设置 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目的 Run64BitRuntime 属性。 如果将 Run64BitRuntime 属性设置为 **true** ，运行时将发现并使用该 64 位提供程序；如果 Run64BitRuntime 为 **false** ，则运行时将发现并使用 32 位提供程序。 有关可以对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目进行设置的属性的详细信息，请参阅 [Integration Services (SSIS) 与 Studio 环境](../integration-services-ssis-development-and-management-tools.md)。   

## <a name="add-a-connection-manager"></a>添加连接管理器
###  <a name="add-a-connection-manager-when-you-create-a-package"></a><a name="wizard"></a> 创建包时添加连接管理器  
  
-   使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导  
  
     除了创建和配置连接管理器之外，该向导还有助于创建和配置使用该连接管理器的源和目标。 有关详细信息，请参阅 [Create Packages in SQL Server Data Tools](../../integration-services/create-packages-in-sql-server-data-tools.md)。  
  
###  <a name="add-a-connection-manager-to-an-existing-package"></a><a name="package"></a> 向现有包中添加连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在“解决方案资源管理器”中，双击该包将其打开  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“控制流”** 选项卡、 **“数据流”** 选项卡或 **“事件处理程序”** 选项卡，以使 **“连接管理器”** 区域可用。  
  
4.  右键单击“连接管理器”  区域中的任意位置，然后执行下列操作之一：  
  
    -   单击要添加到包中的连接管理器类型。  
  
         -或-  
  
    -   如果没有列出您要添加的类型，请单击 **“新建连接”** 打开 **“添加 SSIS 连接管理器”** 对话框，选择某种连接管理器类型，然后单击 **“确定”** 。  
  
     随即打开与所选连接管理器类型对应的自定义对话框。 有关连接管理器类型以及可用选项的详细信息，请参阅下面的选项表。  
  
    |“ODBC 源编辑器”|选项|  
    |------------------------|-------------|  
    |[ADO 连接管理器](../../integration-services/connection-manager/ado-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[ADO.NET 连接管理器](../../integration-services/connection-manager/ado-net-connection-manager.md)|[配置 ADO.NET 连接管理器](./ado-net-connection-manager.md)|  
    |[Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Excel 连接管理器](../../integration-services/connection-manager/excel-connection-manager.md)|[Excel 连接管理器编辑器](./excel-connection-manager.md)|  
    |[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)|[文件连接管理器编辑器](./file-connection-manager.md)|  
    |[多文件连接管理器](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[“添加文件连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)|[平面文件连接管理器编辑器（“常规”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“列”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“高级”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“预览”页）](./flat-file-connection-manager.md)|  
    |[多平面文件连接管理器](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[多平面文件连接管理器编辑器（“常规”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“列”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“高级”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“预览”页）](./multiple-flat-files-connection-manager.md)|  
    |[FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)|[FTP 连接管理器编辑器](./ftp-connection-manager.md)|  
    |[HTTP 连接管理器](../../integration-services/connection-manager/http-connection-manager.md)|[HTTP 连接管理器编辑器（“服务器”页）](./http-connection-manager.md)<br /><br /> [HTTP 连接管理器编辑器（“代理”页）](./http-connection-manager.md)|  
    |[MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)|[MSMQ 连接管理器编辑器](./msmq-connection-manager.md)|  
    |[ODBC 连接管理器](../../integration-services/connection-manager/odbc-connection-manager.md)|[ODBC 连接管理器 UI 参考](./odbc-connection-manager.md)|  
    |[OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[SMO 连接管理器](../../integration-services/connection-manager/smo-connection-manager.md)|[SMO 连接管理器编辑器](./smo-connection-manager.md)|  
    |[SMTP 连接管理器](../../integration-services/connection-manager/smtp-connection-manager.md)|[SMTP 连接管理器编辑器](./smtp-connection-manager.md)|  
    |[SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[SQL Server Compact Edition 连接管理器编辑器（“连接”页）](./sql-server-compact-edition-connection-manager.md)<br /><br /> [SQL Server Compact Edition 连接管理器编辑器（“全部”页）](./sql-server-compact-edition-connection-manager.md)|  
    |[WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)|[WMI 连接管理器编辑器](./wmi-connection-manager.md)|  
  
     **“连接管理器”** 区域列出已添加的连接管理器。  
  
5.  还可以右键单击连接管理器，单击“重命名”  ，然后修改连接管理器的默认名称。  
  
6.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
###  <a name="add-a-connection-manager-at-the-project-level"></a><a name="project"></a> 在项目级别添加连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中打开 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在“解决方案资源管理器”  中，右键单击“连接管理器”  单击“新建连接管理器”  。  
  
3.  在 **“添加 SSIS 连接管理器”** 对话框中，选择连接管理器的类型，然后单击 **“添加”** 。  
  
     随即打开与所选连接管理器类型对应的自定义对话框。 有关连接管理器类型以及可用选项的详细信息，请参阅下面的选项表。  
  
    |“ODBC 源编辑器”|选项|  
    |------------------------|-------------|  
    |[ADO 连接管理器](../../integration-services/connection-manager/ado-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[ADO.NET 连接管理器](../../integration-services/connection-manager/ado-net-connection-manager.md)|[配置 ADO.NET 连接管理器](./ado-net-connection-manager.md)|  
    |[Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Excel 连接管理器](../../integration-services/connection-manager/excel-connection-manager.md)|[Excel 连接管理器编辑器](./excel-connection-manager.md)|  
    |[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)|[文件连接管理器编辑器](./file-connection-manager.md)|  
    |[多文件连接管理器](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[“添加文件连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)|[平面文件连接管理器编辑器（“常规”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“列”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“高级”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“预览”页）](./flat-file-connection-manager.md)|  
    |[多平面文件连接管理器](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[多平面文件连接管理器编辑器（“常规”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“列”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“高级”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“预览”页）](./multiple-flat-files-connection-manager.md)|  
    |[FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)|[FTP 连接管理器编辑器](./ftp-connection-manager.md)|  
    |[HTTP 连接管理器](../../integration-services/connection-manager/http-connection-manager.md)|[HTTP 连接管理器编辑器（“服务器”页）](./http-connection-manager.md)<br /><br /> [HTTP 连接管理器编辑器（“代理”页）](./http-connection-manager.md)|  
    |[MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)|[MSMQ 连接管理器编辑器](./msmq-connection-manager.md)|  
    |[ODBC 连接管理器](../../integration-services/connection-manager/odbc-connection-manager.md)|[ODBC 连接管理器 UI 参考](./odbc-connection-manager.md)|  
    |[OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[SMO 连接管理器](../../integration-services/connection-manager/smo-connection-manager.md)|[SMO 连接管理器编辑器](./smo-connection-manager.md)|  
    |[SMTP 连接管理器](../../integration-services/connection-manager/smtp-connection-manager.md)|[SMTP 连接管理器编辑器](./smtp-connection-manager.md)|  
    |[SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[SQL Server Compact Edition 连接管理器编辑器（“连接”页）](./sql-server-compact-edition-connection-manager.md)<br /><br /> [SQL Server Compact Edition 连接管理器编辑器（“全部”页）](./sql-server-compact-edition-connection-manager.md)|  
    |[WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)|[WMI 连接管理器编辑器](./wmi-connection-manager.md)|  
  
     您添加的连接管理器将显示在 **“解决方案资源管理器”** 中的 **“连接管理器”** 节点下。 它还将显示在项目中所有包的 **“SSIS 设计器”** 窗口的 **“连接管理器”** 选项卡中。 此选项卡中的连接管理器名称具有 **(project)** 前缀，以便将此项目级别的连接管理器与包级别的连接管理器区别开来。  
  
4.  或者，在“解决方案资源管理器”  窗口中的“连接管理器”  节点下或在“SSIS 设计器”  窗口的“连接管理器”  选项卡中，右键单击连接管理器，再单击“重命名”  ，然后修改连接管理器的默认名称。  
  
    > [!NOTE]  
    >  在“SSIS 设计器”窗口的“连接管理器”选项卡中，不能覆盖连接管理器名称中的 (project) 前缀    。 这是设计的结果。  

### <a name="add-ssis-connection-manager-dialog-box"></a>添加 SSIS 连接管理器对话框
使用 **“添加 SSIS 连接管理器”** 对话框可以选择要为包添加的连接类型。  
  
 若要了解有关连接管理器的详细信息，请参阅 [Integration Services (SSIS) 连接](../../integration-services/connection-manager/integration-services-ssis-connections.md)。  
  
#### <a name="options"></a>选项  
 **连接管理器类型**  
 选择一个连接类型，再单击“添加”，或双击一个连接类型，使用与各连接类型相应的编辑器来指定连接属性。  
  
 **添加**  
 使用与各连接类型相应的编辑器指定连接属性。  
   
##  <a name="create-a-parameter-for-a-connection-manager-property"></a><a name="parameter"></a> 创建连接管理器属性的参数  
  
1.  在“连接管理器”  区域中，右键单击要为其创建参数的连接管理器，然后单击“参数化”  。  
  
2.  在 **“参数化”** 对话框中配置参数设置。 有关详细信息，请参阅 [Parameterize Dialog Box](../integration-services-ssis-package-and-project-parameters.md)。  

## <a name="delete-a-connection-manager"></a>删除连接管理器 
###  <a name="delete-a-connection-manager-from-a-package"></a><a name="DeletePackageLevel"></a> 从包中删除连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“控制流”** 选项卡、 **“数据流”** 选项卡或 **“事件处理程序”** 选项卡，以使 **“连接管理器”** 区域可用。  
  
4.  右键单击要删除的连接管理器，然后单击“删除”  。  
  
     如果删除包元素（例如执行 SQL 任务或 OLE DB 源）使用的连接管理器，您会得到以下结果：  
  
    -   使用已删除的连接管理器的包元素上会出现错误图标。  
  
    -   包验证失败。  
  
    -   包无法运行。  
  
5.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
###  <a name="delete-a-shared-connection-manager-project-level-connection-manager"></a><a name="DeleteProjectLevel"></a> 删除共享连接管理器（项目级别连接管理器）  
  
1.  若要删除项目级别的连接管理器，请在“解决方案资源管理器”  窗口的“连接管理器”  节点下，右键单击连接管理器，然后单击“删除”  。 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] 显示下面的警告消息：  
  
    > [!WARNING]  
    >  在您删除某一项目连接管理器后，使用该连接管理器的包可能会不运行。 不能撤消此操作。 是否要删除该连接管理器?  
  
2.  单击“确定”将删除该连接管理器，单击“取消”将保留它。  
  
    > [!NOTE]  
    >  您也可以从为项目中任何包打开的 **“SSIS 设计器”** 窗口的 **“连接管理器”** 选项卡中删除项目级别的连接管理器。 为此，右键单击此选项卡中的连接管理器，然后单击“删除”  。 
    
## <a name="set-the-properties-of-a-connection-manager"></a>设置连接管理器的属性
所有连接管理器都可以使用 **“属性”** 窗口进行配置。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 还提供了用于在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中修改不同类型的连接管理器的自定义对话框。 对话框中所包含的选项集将因连接管理器类型的不同而变化。  
  
### <a name="modify-a-connection-manager-using-the-properties-window"></a>使用“属性”窗口修改连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 SSIS 设计器中，单击 **“控制流”** 选项卡、 **“数据流”** 选项卡或 **“事件处理程序”** 选项卡，以使 **“连接管理器”** 区域可用。  
  
4.  右键单击该连接管理器，再单击“属性”  。  
  
5.  在 **“属性”** 窗口中，编辑属性值。 **“属性”** 窗口提供对在连接管理器的标准编辑器中无法配置的一些属性的访问。  
  
6.  单击“确定”。  
  
7.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  
  
### <a name="modify-a-connection-manager-using-a-connection-manager-dialog-box"></a>使用连接管理器对话框修改连接管理器  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“控制流”** 选项卡、 **“数据流”** 选项卡或 **“事件处理程序”** 选项卡，以使 **“连接管理器”** 区域可用。  
  
4.  在“连接管理器”区域中，双击连接管理器以打开“连接管理器”对话框   。 有关特定连接管理器类型以及每种类型可用的选项的信息，请参阅下表。  
  
    |“ODBC 源编辑器”|选项|  
    |------------------------|-------------|  
    |[ADO 连接管理器](../../integration-services/connection-manager/ado-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[ADO.NET 连接管理器](../../integration-services/connection-manager/ado-net-connection-manager.md)|[配置 ADO.NET 连接管理器](./ado-net-connection-manager.md)|  
    |[Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)|[“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)|  
    |[Excel 连接管理器](../../integration-services/connection-manager/excel-connection-manager.md)|[Excel 连接管理器编辑器](./excel-connection-manager.md)|  
    |[文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)|[文件连接管理器编辑器](./file-connection-manager.md)|  
    |[多文件连接管理器](../../integration-services/connection-manager/multiple-files-connection-manager.md)|[“添加文件连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-file-connection-manager-dialog-box-ui-reference.md)|  
    |[平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)|[平面文件连接管理器编辑器（“常规”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“列”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“高级”页）](./flat-file-connection-manager.md)<br /><br /> [平面文件连接管理器编辑器（“预览”页）](./flat-file-connection-manager.md)|  
    |[多平面文件连接管理器](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)|[多平面文件连接管理器编辑器（“常规”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“列”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“高级”页）](./multiple-flat-files-connection-manager.md)<br /><br /> [多平面文件连接管理器编辑器（“预览”页）](./multiple-flat-files-connection-manager.md)|  
    |[FTP 连接管理器](../../integration-services/connection-manager/ftp-connection-manager.md)|[FTP 连接管理器编辑器](./ftp-connection-manager.md)|  
    |[HTTP 连接管理器](../../integration-services/connection-manager/http-connection-manager.md)|[HTTP 连接管理器编辑器（“服务器”页）](./http-connection-manager.md)<br /><br /> [HTTP 连接管理器编辑器（“代理”页）](./http-connection-manager.md)|  
    |[MSMQ 连接管理器](../../integration-services/connection-manager/msmq-connection-manager.md)|[MSMQ 连接管理器编辑器](./msmq-connection-manager.md)|  
    |[ODBC 连接管理器](../../integration-services/connection-manager/odbc-connection-manager.md)|[ODBC 连接管理器 UI 参考](./odbc-connection-manager.md)|  
    |[OLE DB 连接管理器](../../integration-services/connection-manager/ole-db-connection-manager.md)|[配置 OLE DB 连接管理器](./ole-db-connection-manager.md)|  
    |[SMO 连接管理器](../../integration-services/connection-manager/smo-connection-manager.md)|[SMO 连接管理器编辑器](./smo-connection-manager.md)|  
    |[SMTP 连接管理器](../../integration-services/connection-manager/smtp-connection-manager.md)|[SMTP 连接管理器编辑器](./smtp-connection-manager.md)|  
    |[SQL Server Compact Edition 连接管理器](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md)|[SQL Server Compact Edition 连接管理器编辑器（“连接”页）](./sql-server-compact-edition-connection-manager.md)<br /><br /> [SQL Server Compact Edition 连接管理器编辑器（“全部”页）](./sql-server-compact-edition-connection-manager.md)|  
    |[WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)|[WMI 连接管理器编辑器](./wmi-connection-manager.md)|  
  
5.  若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存选定项”** 。  

## <a name="related-content"></a>相关内容  
  
-   technet.microsoft.com 上的视频 [利用 Microsoft Attunity Connector for Oracle 来增强包性能](/previous-versions/dn912438(v=msdn.10))  
  
-   social.technet.microsoft.com 上的 Wiki 文章 [SSIS 连接](https://social.technet.microsoft.com/wiki/contents/articles/sql-server-integration-services-ssis.aspx#Connectivity)  
  
-   blogs.msdn.com 上的博客文章 [从 SSIS 连接到 MySQL](https://techcommunity.microsoft.com/t5/sql-server-integration-services/connecting-to-mysql-from-ssis/ba-p/387400)。  
  
-   msdn.microsoft.com 上的技术文章 [在 SQL Server Integration Services 中提取和加载 SharePoint 数据](/previous-versions/sql/sql-server-2012/hh368261(v=msdn.10))。  
  
-   support.microsoft.com 上的技术文章 [在 SSIS 中使用 Oracle 连接管理器时收到“DTS_E_CANNOTACQUIRECONNECTIONFROMCONNECTIONMANAGER”错误消息](https://go.microsoft.com/fwlink/?LinkId=233696)。  
  
