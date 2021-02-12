---
title: SQL Server 并行数据仓库连接类型 | Microsoft Docs
description: 利用本文中有关 SQL Server 并行数据仓库连接类型的信息了解如何生成数据源。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 3925fd3d-2aa1-4768-96ad-cfc2c0ba9283
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dfa373b98dfda2af0c8d18b1cc58fc7f0df5a1cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030959"
---
# <a name="sql-server-parallel-data-warehouse-connection-type-ssrs"></a>SQL Server Parallel Data Warehouse 连接类型 (SSRS)

  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] 是一种可扩展的数据仓库工具，可以通过大规模并行处理提供高性能和可扩展性。 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 使用 SQL Server 数据库进行分布式处理和数据存储。  
  
 该工具可跨多个物理节点对大型数据库表进行分区，每个节点运行自己的 SQL Server 实例。 当报表连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 以检索报表数据时，它将连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 工具中的控制节点，该节点负责管理查询处理。 建立连接后，无论是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 环境内部还是外部的 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 实例，都没有任何区别。  
  
 若要在报表中包含来自 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 的数据，你必须拥有一个基于类型为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并行数据仓库的报表数据源的数据集。 此内置数据源类型基于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并行数据仓库数据扩展插件。 使用此数据源类型可连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]并从中检索数据。  
  
 此数据扩展插件支持多值参数、服务器聚合以及与连接字符串分开管理的凭据。  
   
  
 使用本主题中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。  
  
##  <a name="connection-string"></a><a name="Connection"></a> 连接字符串  
 连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]时，也会连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 工具中的数据库对象。 可在查询设计器中指定要使用的数据库对象。 如果未在连接字符串中指定数据库，则将连接到管理员为您分配的默认数据库。 请联系数据库管理员，获取连接信息以及用于连接到数据源的凭据。 下面的连接字符串示例指定 **工具中的** CustomerSales [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 示例数据库：  
  
```  
HOST=<IP address>; database= CustomerSales; port=<port>  
```  
  
 此外，可以使用 **“数据源属性”** 对话框提供用户名和密码等凭据。系统会自动将 `User Id` 和 `Password` 选项追加到连接字符串中，您无需将它们作为连接字符串的一部分键入。 用户界面还提供了相关选项，用于指定 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 工具中的控制节点的 IP 地址及端口号。 默认情况下，该端口为 17000。 该端口可由管理员配置，您的连接字符串可能会使用不同的端口号。  
  
 有关连接字符串示例的更多信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
##  <a name="credentials"></a><a name="Credentials"></a> 凭据  
 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 提供其自己的安全技术，以实现和存储用户名和密码。 不能使用 Windows 身份验证。 如果试图使用 Windows 身份验证连接到 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] ，则会发生错误。  
  
 凭据必须具有足够的权限访问数据库。 根据要执行的查询，您可能需要具有其他权限，例如访问表和视图的足够权限。 外部数据源的所有者必须配置相应的凭据，使这些凭据足以提供对所需数据库对象的只读访问。  
  
 在报表创作客户端，可以使用下列选项指定凭据：  
  
-   使用存储的用户名和密码。 若要协商当包含报表数据的数据库与报表服务器不同时产生的双跃点，请选择使用凭据作为 Windows 凭据的选项。 也可以选择在连接到数据源后模拟经过身份验证的用户。  
  
-   不需要提供任何凭据。 若要使用此选项，您必须具有为报表服务器配置的无人参与的执行帐户。 有关详细信息，请参阅[配置无人参与的执行帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)。 
  
 有关详细信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) 或[为报表数据源指定凭据和连接信息](specify-credential-and-connection-information-for-report-data-sources.md)。  
  
  
##  <a name="queries"></a><a name="Query"></a> 查询  
 查询指定了要为报表数据集检索哪些数据。  
  
 查询的结果集中的列填充数据集的字段集合。 如果查询返回多个结果集，则报表仅处理查询所检索的第一个结果集。 默认情况下，如果您创建一个新查询，或者打开一个可在图形查询设计器中显示的现有查询，则可以使用关系查询设计器。 可以通过下列方式指定查询：  
  
-   以交互方式生成查询。 使用关系查询设计器，此设计器显示表、视图及其他数据库项按数据库架构组织的层次结构视图。 选择表或视图中的列。 通过指定筛选条件、分组和聚合，限制要检索的数据行数。 通过设置参数选项自定义报表运行时的筛选器。  
  
-   键入或粘贴查询。 使用基于文本的查询设计器直接输入 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 文本、粘贴来自其他来源的查询文本、输入使用关系查询设计器无法生成的复杂查询，或输入基于查询的表达式。  
  
-   从文件或报表中导入现有的查询。 使用任一查询设计器中的 **“导入查询”** 按钮浏览到 .sql 文件或 .rdl 文件，然后导入查询。  
  
 有关详细信息，请参阅[关系查询设计器用户界面（报表生成器）](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md)和[基于文本的查询设计器用户界面（报表生成器）](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md)。  
  
 基于文本的查询设计器支持 [文本](#QueryText) 模式，在此模式下，可以键入 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 命令，选择来自数据源的数据。  
  
-   [Text](#QueryText)  
  
 将 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 用于 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]；将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 用于 SQL Server。 SQL 语言的这两种方言非常相似。 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源连接类型编写的查询通常可以用于 [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] 数据源连接类型。  
  
 如果不对数据进行聚合和汇总以减少查询返回的行数，从大型数据库（包括 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]等数据仓库）检索报表数据的查询所生成的结果集就可能会包含大量的行。 您可以使用图形查询设计器或基于文本的查询设计器，编写包括聚合和分组的查询。  
  
 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 支持查询设计器提供的用于汇总数据的子句、关键字和聚合。  
  
 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 使用的图形查询设计器提供对分组和聚合的内置支持，有助于编写仅检索摘要数据的查询。 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 语言功能包括：GROUP BY 子句、DISTINCT 关键字以及 SUM 和 COUNT 等聚合。 基于文本的查询设计器提供对 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 语言的完全支持，包括分组和聚合。  
  
 有关 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的详细信息，请参阅 [Transact-SQL 引用（数据库引擎）](../../t-sql/language-reference.md)。  
  
###  <a name="using-query-type-text"></a><a name="QueryText"></a> 使用 Text 查询类型  
 在基于文本的查询设计器中，可以键入 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 命令来定义数据集中的数据。 用于从 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 检索数据的查询与用于从不在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 应用程序中运行的 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 实例检索数据的查询是相同的。 例如，下面的 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 查询选择职位为销售助理的所有雇员的姓名：  
  
```  
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```  
  
 单击工具栏上的 **“运行”** 按钮 ( **!** ) 可以运行查询并显示结果集。  
  
 若要参数化此查询，请添加一个查询参数。 例如，将 WHERE 子句更改为下面的内容：  
  
 `WHERE HumanResources.Employee.JobTitle = (@JobTitle)`  
  
 运行查询时，会自动创建与查询参数对应的报表参数。 有关详细信息，请参阅本主题后面的 [查询参数](#Parameters) 。  
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameters  
 如果查询文本包含查询变量或具有输入参数的存储过程，则将自动生成数据集的对应查询参数和报表的报表参数。 查询文本不得包含针对每个查询变量的 DECLARE 语句。  
  
 例如，下面的 SQL 查询将创建一个名为 **EmpID** 的报表参数：  
  
```  
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```  
  
 默认情况下，各个报表参数的数据类型均为“Text”，并具有自动创建的数据集，以提供可用值的下拉列表。 创建报表参数后，您可能需要更改默认值。 有关详细信息，请参阅 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)的详细信息。  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> 注释  
  
###### <a name="platform-and-version-information"></a>平台和版本信息  
 有关平台和版本支持的详细信息，请参阅 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> 操作指南主题  
 本节包含使用数据连接、数据源和数据集的分步说明。  
  
 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [创建共享数据集或嵌入数据集（报表生成器和 SSRS）](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [向数据集添加筛选器（报表生成器和 SSRS）](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="related-sections"></a><a name="Related"></a> 相关章节  
 文档中的这些章节提供有关报表数据的深入概念性信息，以及有关如何定义、自定义和使用与数据相关的报表部件的步骤信息。  
  
 [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 提供访问报表数据的概述。  
  
 [创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 提供有关数据连接和数据源的信息。  
  
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 提供有关嵌入数据集和共享数据集的信息。  
  
 [数据集字段集合（报表生成器和 SSRS）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 提供有关查询生成的数据集字段集合的信息。  
  
 [Reporting Services 支持的数据源 &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)。  
 提供有关每个数据扩展插件的平台和版本支持的详细信息。  

## <a name="next-steps"></a>后续步骤

[报表参数](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[对数据进行筛选、分组和排序](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[表达式](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)