---
title: Oracle 连接类型（SSRS 和 Power BI 报表服务器）
description: 利用本文中有关 Oracle 连接类型的信息，以了解如何生成数据源。
ms.date: 11/03/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 9db86dd2-beda-42d8-8af7-2629d58a8e3d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1df8e6c21f4aba600d36314ea224d1a77d6f2332
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464438"
---
# <a name="oracle-connection-type-ssrs--power-bi-report-server"></a>Oracle 连接类型（SSRS 和 Power BI 报表服务器）

若要在报表中使用来自 Oracle 数据库的数据，你必须拥有一个基于 Oracle 类型的报表数据源的数据集。 此内置数据源类型直接使用 Oracle Data Provider，并且需要 Oracle 客户端软件组件。 本文介绍如何下载和安装 Reporting Services、Power BI 报表服务器、报表生成器和 Power BI Desktop 的驱动程序。


使用本文中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。 

> [!IMPORTANT]
> 以下命令使用 Oracle 的 OraProvCfg.exe 工具注册 Oracle 的托管和非托管 ODP.NET 驱动程序，在此作为示例展示如何与上述 Microsoft 产品一起使用。 若要配置特定于环境的 ODP.NET 驱动程序，可能需要联系 Oracle 支持团队或参考 Oracle 文档以[配置用于 .NET 的 Oracle 数据提供程序](https://docs.oracle.com/en/database/oracle/oracle-database/19/odpnt/InstallConfig.html#GUID-1F689B90-2CC4-4907-B8FE-A5F4EE36F673)。

## <a name="64-bit-drivers-for-the-report-servers"></a>用于报表服务器的 64 位驱动程序

在 Oracle 下载网站上，安装 [Oracle 64 位 ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html)。 使用 Oracle ODAC 驱动程序 12.2 和更高版本时，只需执行以下步骤。 否则它们默认安装到新 Oracle 主安装的非计算机范围配置中。 以下步骤假设已将 ODAC 18.x 文件安装到 c:\oracle64 文件夹。


### <a name="paginated-rdl-reports-use-managed-odpnet"></a>分页 (RDL) 报表使用托管的 ODP.NET

Power BI 报表服务器和 SQL Server Reporting Services 2016 和更高版本都对分页 (RDL) 报表使用 Managed ODP.NET。 请按照以下步骤注册托管的 ODP.NET：

1. 向 GAC 注册 ODP.NET 托管客户端：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

2. 将 ODP.NET 托管客户端条目添加到 machine.config：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

### <a name="power-bi-reports-use-unmanaged-odpnet"></a>Power BI 报表使用非管理的 ODP.NET

Power BI 报表服务器对 Power BI 报表使用非托管的 ODP.NET。 请按照以下步骤注册非管理的 ODP.NET：

1. 向 GAC 注册 ODP.NET 非管理的客户端：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```
2. 将 ODP.NET 非管理的客户端条目添加到 machine.config：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

## <a name="32-bit-drivers-for-report-builder"></a>用于报表生成器的 32 位驱动程序

报表生成器使用“托管的 ODP.NET”来编写分页 (RDL) 报表。 使用 Oracle ODAC 驱动程序 12.2 和更高版本时，只需执行以下步骤。 否则它们默认安装到新 Oracle 主安装的非计算机范围配置中。 以下步骤假设已将 ODAC 18.x 文件安装到 c:\oracle32 文件夹（报表生成器就安装在此处）。 请按照以下步骤注册托管的 ODP.NET：

1. 在 Oracle 下载网站上，安装 [Oracle 32 位 ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html)。

2. 向 GAC 注册 ODP.NET 托管客户端：

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

3. 将 ODP.NET 托管客户端条目添加到 machine.config：

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odpm /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\managed\common\Oracle.ManagedDataAccess.dll
   ```

## <a name="64-bit-and-32-bit-drivers-for-power-bi-desktop"></a>用于 Power BI Desktop 的 64 位和 32 位驱动程序

Power BI Desktop 使用非托管的 ODP.NET 编写 Power BI 报表。 使用 Oracle ODAC 驱动程序 12.2 和更高版本时，只需执行以下步骤。 否则它们默认安装到新 Oracle 主安装的非计算机范围配置中。 以下步骤假设已将 ODAC 18.x 文件安装到 64 位 Power BI Desktop 的 c:\oracle64 文件夹或 32 位 Power BI Desktop 的 c:\oracle32 文件夹中。 请按照以下步骤注册非管理的 ODP.NET：

### <a name="64-bit-power-bi-desktop"></a>64 位 Power BI Desktop

1. 在 Oracle 下载网站上，安装 [Oracle 64 位 ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdeploy-4242173.html)。
2. 向 GAC 注册 ODP.NET 非管理的客户端：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. 将 ODP.NET 非管理的客户端条目添加到 machine.config：

   ```
   C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle64\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

### <a name="32-bit-power-bi-desktop"></a>32 位 Power BI Desktop

1. 在 Oracle 下载网站上，安装 [Oracle 32 位 ODAC Oracle Universal Installer (OUI)](https://www.oracle.com/technetwork/topics/dotnet/downloads/odacdev-4242174.html)。

2. 向 GAC 注册 ODP.NET 非管理的客户端：

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:gac /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

3. 将 ODP.NET 非管理的客户端条目添加到 machine.config：

   ```
   C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\OraProvCfg.exe /action:config /force /product:odp /frameworkversion:v4.0.30319 /providerpath:C:\oracle32\product\18.0.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll
   ```

##  <a name="connection-string"></a><a name="Connection"></a> 连接字符串  

请联系数据库管理员，获取连接信息以及用于连接到数据源的凭据。 下面的连接字符串示例指定使用 Unicode 的名为“Oracle18”的服务器上的 Oracle 数据库。 服务器名称必须与 Tnsnames.ora 配置文件中定义的 Oracle 服务器实例名相匹配。  
  
```  
Data Source="Oracle"; Unicode="True"  
```  
  
有关更多连接字符串的示例，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
##  <a name="credentials"></a><a name="Credentials"></a> 凭据  
执行以下操作时需要提供凭据：运行查询、本地预览报表以及从报表服务器预览报表。  
  
报表发布后，您可能需要更改数据源的凭据，以使报表在报表服务器上运行时，用于检索数据的权限有效。  
  
有关详细信息，请参阅[为报表数据源指定凭据和连接信息](specify-credential-and-connection-information-for-report-data-sources.md)  
  
##  <a name="queries"></a><a name="Query"></a> 查询  
若要创建数据集，可以从下拉列表中选择存储过程，也可以创建一个 SQL 查询。 若要生成一个查询，必须使用基于文本的查询设计器。 有关详细信息，请参阅[基于文本的查询设计器用户界面（报表生成器）](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md)。  
  
可以指定只返回一个结果集的存储过程。 不支持使用基于游标的查询。  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameters  

如果查询包括查询变量，则将自动生成对应的报表参数。 此扩展插件支持命名参数。 对于 Oracle 版本 9 或更高版本而言，支持多值参数。  
  
 报表参数是用可能需要修改的默认属性值创建的。 例如，每个报表参数的数据类型均为 **Text**。 创建报表参数后，您可能需要更改默认值。 有关详细信息，请参阅 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)的详细信息。  
  
##  <a name="remarks"></a><a name="Remarks"></a> 注释  

在可以连接 Oracle 数据源之前，系统管理员必须已安装支持从 Oracle 数据库中检索数据的 .NET Data Provider for Oracle 版本。 此数据访问接口必须与报表生成器安装在同一台计算机上，报表服务器上也是如此。  
  
有关详细信息，请参阅以下文章：  
  
- [如何使用 Reporting Services 配置和访问 Oracle 数据源](/archive/blogs/dataaccesstechnologies/configure-oracle-data-source-for-sql-server-reporting-services-ssdt-and-report-server)  
- [如何为 NETWORK SERVICE 安全主体添加权限](https://support.microsoft.com/kb/870668)  
  
### <a name="alternate-data-extensions"></a>备用数据扩展插件 

您还可以通过使用 OLE DB 数据源类型从 Oracle 数据库中检索数据。 有关详细信息，请参阅 [OLE DB 连接类型 (SSRS)](../../reporting-services/report-data/ole-db-connection-type-ssrs.md)。  

::: moniker range=">=sql-server-2017"

### <a name="report-models"></a>报表模型

 您还可以创建基于 Oracle 数据库的模型。  
::: moniker-end

### <a name="platform-and-version-information"></a>平台和版本信息  

有关平台和版本支持的详细信息，请参阅 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  

## <a name="see-also"></a>另请参阅

[报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   

[对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   

[表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)
