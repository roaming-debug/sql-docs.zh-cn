---
title: XML 输入文件引用
titleSuffix: Database Engine Tuning Advisor
description: 本文总结了数据库引擎优化顾问用于优化数据库的 XML 输入文件的元素。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 05e5e5f0-d6df-4336-b18e-e9bc2835a766
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: ce7ce983b1658cdccac010aed9816dde417ac727
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689087"
---
# <a name="xml-input-file-reference-database-engine-tuning-advisor"></a>XML 输入文件引用（数据库引擎优化顾问）

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可以使用 XML 输入文件来优化数据库。 此 XML 文件指定要用于优化会话的数据库、表、工作负荷文件或表以及优化选项。 您还可以使用此文件指定一个用户指定的配置来执行“假设”分析。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 输入文件包含 XML 元素（每个元素包含文本或其他用于指定优化会话设置的元素）的层次结构。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 输入文件必须符合格式正确的 XML 的标准，因此所有的元素名称都区分大小写。 使用 Pascal case 来指定元素，即第一个字符大写，随后的所有串连单词的第一个字母都大写。  
  
 所有元素值都必须符合 XML 命名约定。 有关这些约定的详细信息，请参阅 MSDN Library 中的 [XML Textual Content](/previous-versions/windows/desktop/ms763742(v=vs.85)) 。  
  
 请注意，此参考并非综合性参考。 有关可用于定义 XML 输入的所有元素的信息，请参考 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 架构 (DTASchema.xsd)。  
  
## <a name="xml-declaration"></a>XML 声明  
  
-   [XML 数据 (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
## <a name="dtaxml-root-element"></a>DTAXML 根元素  
  
-   [DTAXML 元素 (DTA)](../../tools/dta/dtaxml-element-dta.md)  
  
## <a name="dtainput-elements"></a>DTAInput 元素  
  
-   [DTAInput 元素 (DTA)](../../tools/dta/dtainput-element-dta.md)  
  
-   [服务器元素 (DTA)](../../tools/dta/server-element-dta.md)  
  
-   [工作负荷元素 (DTA)](../../tools/dta/workload-element-dta.md)  
  
-   [TuningOptions 元素 (DTA)](../../tools/dta/tuningoptions-element-dta.md)  
  
-   [配置元素 (DTA)](../../tools/dta/configuration-element-dta.md)  
  
## <a name="server-elements"></a>服务器元素  
  
-   [服务器的名称元素 (DTA)](../../tools/dta/name-element-for-server-dta.md)  
  
-   [服务器的数据库元素 (DTA)](../../tools/dta/database-element-for-server-dta.md)  
  
## <a name="workload-elements"></a>工作负荷元素  
  
-   [文件元素 (DTA)](../../tools/dta/file-element-dta.md)  
  
-   [工作负荷的数据库元素 (DTA)](../../tools/dta/database-element-for-workload-dta.md)  
  
-   [EventString 元素 (DTA)](../../tools/dta/eventstring-element-dta.md)  
  
## <a name="tuning-options-elements"></a>优化选项元素  
  
-   [TuningTimeInMin 元素 (DTA)](../../tools/dta/tuningtimeinmin-element-dta.md)  
  
-   [StorageBoundInMB 元素 (DTA)](../../tools/dta/storageboundinmb-element-dta.md)  
  
-   [TestServer 元素 (DTA)](../../tools/dta/testserver-element-dta.md)  
  
-   [FeatureSet 元素 (DTA)](../../tools/dta/featureset-element-dta.md)  
  
-   [分区元素 (DTA)](../../tools/dta/partitioning-element-dta.md)  
  
-   [DropOnlyMode 元素 (DTA)](../../tools/dta/droponlymode-element-dta.md)  
  
-   [KeepExisting 元素 (DTA)](../../tools/dta/keepexisting-element-dta.md)  
  
-   [OnlineIndexOperation 元素 (DTA)](../../tools/dta/onlineindexoperation-element-dta.md)  
  
-   [DatabaseToConnect 元素 (DTA)](../../tools/dta/databasetoconnect-element-dta.md)  
  
## <a name="configuration-elements"></a>配置元素  
  
-   [用于配置的服务器元素 (DTA)](../../tools/dta/server-element-for-configuration-dta.md)  
  
-   [配置的数据库元素 (DTA)](../../tools/dta/database-element-for-configuration-dta.md)  
  
-   [建议元素 (DTA)](../../tools/dta/recommendation-element-dta.md)  
  
-   [创建元素 (DTA)](../../tools/dta/create-element-dta.md)  
  
-   [索引元素 (DTA)](../../tools/dta/index-element-dta.md)  
  
-   [索引的名称元素 (DTA)](../../tools/dta/name-element-for-index-dta.md)  
  
-   [索引的列元素 (DTA)](../../tools/dta/column-element-for-index-dta.md)  
  
-   [列的名称元素 (DTA)](../../tools/dta/name-element-for-column-dta.md)  
  
-   [索引的文件组元素 (DTA)](../../tools/dta/filegroup-element-for-index-dta.md)  
  
## <a name="database-elements"></a>数据库元素  
  
-   [数据库的名称元素 (DTA)](../../tools/dta/name-element-for-database-dta.md)  
  
-   [数据库的架构元素 (DTA)](../../tools/dta/schema-element-for-database-dta.md)  
  
-   [架构的名称元素 (DTA)](../../tools/dta/name-element-for-schema-dta.md)  
  
-   [架构的表元素 (DTA)](../../tools/dta/table-element-for-schema-dta.md)  
  
-   [表的名称元素 (DTA)](../../tools/dta/name-element-for-table-dta.md)  
  
## <a name="see-also"></a>另请参阅  
 [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)  
  
