---
title: 报表服务器 ExecutionLog 和 ExecutionLog3 视图 | Microsoft Docs
description: 了解 Reporting Services 中的报表服务器执行日志，该日志包含有关原生模式或 SharePoint 场中服务器上的报表信息。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], execution
- execution logs [Reporting Services]
ms.assetid: a7ead67d-1404-4e67-97e7-4c7b0d942070
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 28d083e053e31c1fcce26e233ba22211e211a993
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125580"
---
# <a name="report-server-executionlog-and-the-executionlog3-view"></a>报表服务器 ExecutionLog 和 ExecutionLog3 视图
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]报表服务器执行日志包含有关在一个或多个服务器上在本机模式扩展部署或 SharePoint 场中执行的报表的信息。 您可以使用报表执行日志来查明报表的请求频率、最常用的输出格式以及每个处理阶段所用的处理时间（毫秒）。 该日志包含与执行报表的数据集查询所用的时间长度和处理数据所用的时间长度有关的信息。 如果您是报表服务器管理员，则可以查看日志信息并标识长时间运行的任务，并且向报表作者就其可以改进的报表方面（数据集或处理）提出建议。  
  
 配置为 SharePoint 模式的报表服务器也可以利用 SharePoint ULS 日志。 有关详细信息，请参阅 [为 SharePoint 跟踪日志 (ULS) 启用 Reporting Services 事件](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
##  <a name="viewing-log-information"></a><a name="bkmk_top"></a> 查看日志信息  
 报表服务器执行日志将报表执行情况的有关数据记录在内部数据库表中。 表中的信息可从 SQL Server 视图获得。  
  
 报表执行日志存储于默认名为 **ReportServer** 的报表服务器数据库中。 SQL 视图提供执行日志信息。 “2”和“3”视图已在最近的版本中添加，并且包含新字段或者所包含字段的名称比以前版本更友好。 较旧的视图仍保留在产品中，这样，依赖于它们的自定义应用程序将不会受到影响。 如果您不依赖于较旧的视图，例如 ExecutionLog，则建议您使用最新视图 ExecutionLog **3**。  
  
 本主题内容：  
  
-   [针对 SharePoint 模式报表服务器的配置设置](#bkmk_sharepoint)  
  
-   [针对本机模式报表服务器的配置设置](#bkmk_native)  
  
-   [日志字段 (ExecutionLog3)](#bkmk_executionlog3)  
  
-   [AdditionalInfo 字段](#bkmk_additionalinfo)  
  
-   [日志字段 (ExecutionLog2)](#bkmk_executionlog2)  
  
-   [日志字段 (ExecutionLog)](#bkmk_executionlog)  
  
##  <a name="configuration-settings-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepoint"></a> 针对 SharePoint 模式报表服务器的配置设置  
 您可以从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的系统设置启用或禁用报表执行日志记录。  
  
 默认情况下，日志条目保留 60 天。 超过此日期的条目将于每日凌晨 2:00 删 除。 对于成熟的安装，在任何给定时间都只保留 60 天的信息。  
  
 不能针对行数或所记录的条目类型设置限制。  
  
 **启用执行日志记录：**  
  
1.  在 SharePoint 管理中心内，在 **“应用程序管理”** 组中单击 **“管理服务应用程序”** 。  
  
2.  单击要配置的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的名称。  
  
3.  单击 **“系统设置”** 。  
  
4.  在 **“日志记录”** 部分中选择 **“启用执行日志记录”** 。  
  
5.  单击“确定”。  
  
 **启用详细日志记录：**  
  
 您需要如前面的步骤中所述启用日志记录，然后完成以下内容：  
  
1.  从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的“系统设置”页，找到“用户定义”部分 。  
  
2.  将 **ExecutionLogLevel** 更改为 **verbose**。 该字段是文本输入字段，其两个可能的值是 **verbose** 和 **normal**。  
  
##  <a name="configuration-settings-for-a-native-mode-report-server"></a><a name="bkmk_native"></a> 针对本机模式报表服务器的配置设置  
 从 SQL Server Management Studio 的“服务器属性”页，您可以启用或禁用报表执行日志记录。 **EnableExecutionLogging** 是高级属性。  
  
 默认情况下，日志条目保留 60 天。 超过此日期的条目将于每日凌晨 2:00 删 除。 对于成熟的安装，在任何给定时间都只保留 60 天的信息。  
  
 不能针对行数或所记录的条目类型设置限制。  
  
 **启用执行日志记录：**  
  
1.  使用管理权限启动 SQL Server Management Studio。 例如，用鼠标右键单击 Management Studio 图标，然后单击“以管理员身份运行”。  
  
2.  连接到所需报表服务器。  
  
3.  右键单击服务器名称并单击“属性”。 如果“属性”选项被禁用，请确认您是在使用管理权限运行 SQL Server Management Studio。  
  
4.  单击 **“日志记录”** 页。  
  
5.  选择“启用报表执行日志记录” 。  
  
 **启用详细日志记录：**  
  
 您需要如前面的步骤中所述启用日志记录，然后完成以下内容：  
  
1.  在 **“服务器属性”** 对话框中，单击 **“高级”** 页。  
  
2.  在“用户定义”部分中，将 ExecutionLogLevel 更改为 verbose  。 该字段是文本输入字段，其两个可能的值是 **verbose** 和 **normal**。  
  
##  <a name="log-fields-executionlog3"></a><a name="bkmk_executionlog3"></a> 日志字段 (ExecutionLog3)  
 此视图在基于 XML 的 **AdditionalInfo** 列中添加了其他性能诊断节点。 AdditionalInfo 列包含 1 对多的其他字段的 XML 结构信息。 下面是一个示例 Transact SQL 语句，从视图 ExecutionLog3 检索行。 该示例假定报表服务器数据库名为 **ReportServer**：  
  
```  
Use ReportServer  
select * from ExecutionLog3 order by TimeStart DESC  
```  
  
 下表描述在报表执行日志中捕获的数据  
  
|列|描述|  
|------------|-----------------|  
|InstanceName|处理请求的报表服务器实例的名称。 如果您的环境具有多个报表服务器，则可以对 InstanceName 分布进行分析，以便监视并确定您的网络负载平衡器是否按预期跨多个报表服务器分布请求。|  
|ItemPath|存储报表或报表项的位置的路径。|  
|UserName|用户标识符。|  
|ExecutionID|与请求关联的内部标识符。 同一用户会话的请求共享相同的执行 ID。|  
|RequestType|可能的值：<br /><br /> 交互<br /><br /> 订阅<br /><br /> <br /><br /> 分析按 RequestType=Subscription 筛选的日志数据和按 TimeStart 排序的数据可揭示大量使用订阅的时间段，这样您可能要将某些报表订阅修改为其他时间。|  
|格式|呈现格式。|  
|参数|用于执行报表的参数值。|  
|ItemAction|可能的值：<br /><br /> 呈现<br /><br /> 排序<br /><br /> BookMarkNavigation<br /><br /> DocumentNavigation<br /><br /> GetDocumentMap<br /><br /> Findstring<br /><br /> 执行<br /><br /> RenderEdit|  
|TimeStart|指示报表进程的持续时段的开始时间和结束时间。|  
|TimeEnd||  
|TimeDataRetrieval|用于检索数据的毫秒数。|  
|TimeProcessing|用于处理报表的毫秒数。|  
|TimeRendering|用于呈现报表的毫秒数。|  
|源|报表执行的源。 可能的值：<br /><br /> 实时<br /><br /> Cache：指示缓存的执行，例如，数据集查询不实时执行。<br /><br /> 快照<br /><br /> 历史记录<br /><br /> AdHoc：指示基于动态生成的报表模型的钻取报表，或在客户端上预览的报表生成器报表（使用报表服务器进行处理和呈现）。<br /><br /> 会话：指示已建立会话内的跟进请求。  例如，初始请求为查看第一页，跟进请求为使用当前会话状态导出到 Excel。<br /><br /> RDCE：指示报表定义自定义扩展插件。 在执行报表时，RDCE 自定义扩展插件可以在将某一报表定义传递到处理引擎前动态自定义该报表定义。|  
|状态|状态（rsSuccess 或错误代码；如果发生多个错误，则只记录第一个）。|  
|ByteCount|所呈现的报表的大小（字节）。|  
|RowCount|查询返回的结果行数。|  
|AdditionalInfo|包含与执行有关的附加信息的 XML 属性包。 对于每一行，内容可以不同。|  
  
##  <a name="the-additionalinfo-field"></a><a name="bkmk_additionalinfo"></a> AdditionalInfo 字段  
 AdditionalInfo 字段是包含与执行有关的其他信息的 XML 属性包或结构。 对于日志中的每一行，内容可以不同。  
  
 下面显示标准日志记录和详细日志记录模式下 AddtionalInfo 字段的内容示例：  
  
 **AddtionalInfo 的标准日志记录示例**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>147</ConnectionOpenTime>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>642</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>63</ExecuteReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>157</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>60</ExecuteReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 **AdditionalInfo 的详细日志记录示例**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>127</ConnectionOpenTime>  
      <DataSource>  
        <Name>DataSource1</Name>  
        <DataExtension>SQL</DataExtension>  
      </DataSource>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>33</ExecuteReaderTime>  
          <DataReaderMappingTime>30</DataReaderMappingTime>  
          <DisposeDataReaderTime>1</DisposeDataReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>1</ExecuteReaderTime>  
          <DataReaderMappingTime>0</DataReaderMappingTime>  
          <DisposeDataReaderTime>0</DisposeDataReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 下面是你将在 AdditionalInfo 字段中看到的某些值：  
  
-   **ProcessingEngine**  
  
     1=SQL Server 2005，2=新的按需处理引擎。 如果您的大多数报表仍在显示值 1，则可以研究如何对它们进行重新设计，以便利用更新且效率更高的按需处理引擎。  
  
     `<ProcessingEngine>2</ProcessingEngine>`  
  
-   **ScalabilityTime**  
  
     在处理引擎中执行与进制相关的运算所用的毫秒数。 值为 0 指示没有额外的时间用于进制运算，值为 0 还指示请求没有处于内存不足的状态。  
  
    ```  
    <ScalabilityTime>  
        <Processing>0</Processing>  
    </ScalabilityTime>  
    ```  
  
-   **EstimatedMemoryUsageKB**  
  
     每个组件在特定请求过程中使用的内存峰值的估计值，以 KB 为单位。  
  
    ```  
    <EstimatedMemoryUsageKB>  
        <Processing>38</Processing>  
    </EstimatedMemoryUsageKB>  
    ```  
  
-   **DataExtension**  
  
     在报表中使用的数据扩展插件或数据源的类型。 该数目是特定数据源出现的次数。  
  
    ```  
    <DataExtension>  
       <DAX>2</DAX>  
    </DataExtension>  
    ```  
  
-   **ExternalImages**  
  
     该值以毫秒计。 此数据可用于诊断性能问题。 从外部 Web 服务器检索图像所需的时间可能使总体报表执行速度变慢。  
  
    ```  
    <ExternalImages>  
        <Count>3</Count>  
        <ByteCount>9268</ByteCount>  
        <ResourceFetchTime>9</ResourceFetchTime>  
    </ExternalImages>  
    ```  
  
-   **连接**  
  
     多级别结构  
  
    ```  
    <Connections>  
        <Connection>  
          <ConnectionOpenTime>127</ConnectionOpenTime>  
          <DataSource>  
            <Name>DataSource1</Name>  
            <DataExtension>SQL</DataExtension>  
          </DataSource>  
          <DataSets>  
            <DataSet>  
              <Name>DataSet1</Name>  
              <RowsRead>16</RowsRead>  
              <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>33</ExecuteReaderTime>  
              <DataReaderMappingTime>30</DataReaderMappingTime>  
              <DisposeDataReaderTime>1</DisposeDataReaderTime>  
            </DataSet>  
            <DataSet>  
              <Name>DataSet2</Name>  
              <RowsRead>3</RowsRead>  
              <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>1</ExecuteReaderTime>  
              <DataReaderMappingTime>0</DataReaderMappingTime>  
              <DisposeDataReaderTime>0</DisposeDataReaderTime>  
            </DataSet>  
          </DataSets>  
        </Connection>  
    </Connections>  
  
    ```  
  
##  <a name="log-fields-executionlog2"></a><a name="bkmk_executionlog2"></a> 日志字段 (ExecutionLog2)  
 此视图添加了几个新字段并且重命名了其他几个字段。 下面是一个示例 Transact SQL 语句，从视图 ExecutionLog2 检索行。 该示例假定报表服务器数据库名为 **ReportServer**：  
  
```  
Use ReportServer  
select * from ExecutionLog2 order by TimeStart DESC  
```  
  
 下表描述在报表执行日志中捕获的数据  
  
|列|描述|  
|------------|------------------------------------------------------------|  
|InstanceName|处理请求的报表服务器实例的名称。|  
|ReportPath|报表的路径结构。 作为“test”保存在根文件夹中的报表具有“/test”的 ReportPath。<br /><br /> 保存在文件夹“samples”中的名为“test”的报表将具有“/Samples/test/”的 ReportPath|  
|UserName|用户标识符。|  
|ExecutionID||  
|RequestType|请求类型（用户或系统）。|  
|格式|呈现格式。|  
|参数|用于执行报表的参数值。|  
|ReportAction|可能的值：Render、Sort、BookMarkNavigation、DocumentNavigation、GetDocumentMap、Findstring|  
|TimeStart|指示报表进程的持续时段的开始时间。|
|TimeEnd|指示报表进程的持续时段的结束时间。|
|TimeDataRetrieval|用于检索数据的毫秒数。|
|TimeProcessing|用于处理报表的毫秒数。|
|TimeRendering|用于呈现报表的毫秒数。|
|源|报表执行源（1 = 实时，2 = 缓存，3 = 快照，4 = 历史记录）。|  
|状态|状态（rsSuccess 或错误代码；如果发生多个错误，则只记录第一个）。|  
|ByteCount|所呈现的报表的大小（字节）。|  
|RowCount|查询返回的结果行数。|  
|AdditionalInfo|包含与执行有关的附加信息的 XML 属性包。|  
  
##  <a name="log-fields-executionlog"></a><a name="bkmk_executionlog"></a> 日志字段 (ExecutionLog)  
 下面是一个示例 Transact SQL 语句，从视图 ExecutionLog 检索行。 该示例假定报表服务器数据库名为 **ReportServer**：  
  
```  
Use ReportServer  
select * from ExecutionLog order by TimeStart DESC  
  
```  
  
 下表描述在报表执行日志中捕获的数据  
  
|列|描述|  
|------------|-----------------|  
|InstanceName|处理请求的报表服务器实例的名称。|  
|ReportID|报表标识符。|  
|UserName|用户标识符。|  
|RequestType|可能的值：<br /><br /> True = 订阅请求<br /><br /> False = 交互请求|  
|格式|呈现格式。|  
|参数|用于执行报表的参数值。|  
|TimeStart|指示报表进程的持续时段的开始时间和结束时间。|  
|TimeEnd||  
|TimeDataRetrieval|检索数据、处理报表以及呈现报表所用的毫秒数。|  
|TimeProcessing||  
|TimeRendering||  
|源|报表执行的源。 可能的值：（1 = 实时，2 = 缓存，3 = 快照，4 = 历史记录，5 = 即席，6 = 会话，7 = RDCE）。|  
|状态|可能的值：rsSuccess、rsProcessingAborted 或错误代码。 如果出现多个错误，则只记录第一个错误。|  
|ByteCount|所呈现的报表的大小（字节）。|  
|RowCount|查询返回的结果行数。|  
  
## <a name="see-also"></a>另请参阅  
 [为 SharePoint 跟踪日志 (ULS) 启用 Reporting Services 事件](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)   
 [Reporting Services 日志文件和来源](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [错误和事件参考 (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
