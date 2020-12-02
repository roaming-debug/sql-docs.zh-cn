---
description: 第 1 课：使用 SSIS 创建项目和基本包
title: 第 1 课：使用 SSIS 创建项目和基本包 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 84d0b877-603f-4f8e-bb6b-671558ade5c2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 428295430a2abb50738742db088b9573a7bf35a6
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88461983"
---
# <a name="lesson-1-create-a-project-and-basic-package-with-ssis"></a>第 1 课：使用 SSIS 创建项目和基本包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



在本课中，你将创建简单的 ETL 包，该包从单个平面文件源提取数据，使用两个查找转换组件转换数据，并将转换后的数据写入“AdventureWorksDW2012”示例数据库中的“FactCurrencyRate”事实数据表的副本。 在本课中，将了解如何创建新包、添加和配置数据源和目标连接以及使用新的控制流和数据流组件。  
  
在创建包之前，需要了解源数据和目标数据中使用的格式。 然后，就可定义将源数据映射到目标数据所需的转换。  

## <a name="prerequisites"></a>先决条件

本教程依赖于 Microsoft SQL Server Data Tools、一组示例包和一个示例数据库。

* 要安装 SQL Server Data Tools，请参阅[下载 SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)。  
  
* 要下载本教程的所有课程包，请执行以下操作：

    1.  导航到 [Integration Services 教程文件](https://www.microsoft.com/download/details.aspx?id=56827)。

    2.  选择“下载”按钮。

    3.  选择“创建简单的 ETL Package.zip”文件，然后选择“下一步”。

    4.  下载文件后，将其内容解压缩到本地目录。  

* 要安装和部署“AdventureWorksDW2012”示例数据库，请参阅[安装和配置 AdventureWorks 示例数据库 - SQL](../samples/adventureworks-install-configure.md)。
  
## <a name="look-at-the-source-data"></a>查看源数据
在本教程中，源数据是名为“SampleCurrencyData.txt”的平面文件中的一组历史货币数据。 源数据具有以下四列：货币的平均汇率、货币键、日期键和收盘汇率。  
  
下面是 SampleCurrencyData.txt 文件中的源数据示例：  
  
```
1.00070049USD9/3/05 0:001.001201442  
1.00020004USD9/4/05 0:001  
1.00020004USD9/5/05 0:001.001201442  
1.00020004USD9/6/05 0:001  
1.00020004USD9/7/05 0:001.00070049  
1.00070049USD9/8/05 0:000.99980004  
1.00070049USD9/9/05 0:001.001502253  
1.00070049USD9/10/05 0:000.99990001  
1.00020004USD9/11/05 0:001.001101211  
1.00020004USD9/12/05 0:000.99970009
```
  
在使用平面文件源数据时，需要了解平面文件连接管理器如何解释平面文件数据，这一点很重要。 如果平面文件源是 Unicode 编码的，则平面文件连接管理将所有列定义为 [DT_WSTR]，默认列宽为 50。 如果平面文件源是 ANSI 编码的，则将列定义为 [DT_STR]，默认列宽为 50。 可能必须更改这些默认设置，才能使字符串列类型更适用于你的数据。 需要查看目标的数据类型，然后在平面文件连接管理器中选择该类型。  
  
## <a name="look-at-the-destination-data"></a>查看目标数据
源数据的目标是“AdventureWorksDW”中“FactCurrencyRate”事实数据表的副本。 “FactCurrencyRate”事实数据表有四列，并且与两个维度表有关，如下表所示。  
  
|列名|数据类型|查找表|查找列|  
|---------------|-------------|----------------|-----------------|  
|AverageRate|FLOAT|无|无|  
|CurrencyKey|int (FK)|DimCurrency|CurrencyKey (PK)|  
|DateKey|int (FK)|DimDate|DateKey (PK)|  
|EndOfDayRate|FLOAT|无|无|  
  
## <a name="map-the-source-data-to-the-destination"></a>源数据映射到目标  
对源数据和目标数据格式的分析指出需要查找 CurrencyKey 和 DateKey 值。 将执行这些查找的转换通过使用 DimCurrency 和 DimDate 维度表中的备用键来获取这些值。  
  
|平面文件列|表名称|列名|数据类型|  
|--------------------|--------------|---------------|-------------|  
|0|FactCurrencyRate|AverageRate|FLOAT|  
|1|DimCurrency|CurrencyAlternateKey|nchar(3)|  
|2|DimDate|FullDateAlternateKey|date|  
|3|FactCurrencyRate|EndOfDayRate|FLOAT|  
  
## <a name="lesson-tasks"></a>课程任务  
本课程包含以下任务：  
  
-   [步骤 1：创建新的 Integration Services 项目](../integration-services/lesson-1-1-creating-a-new-integration-services-project.md)  
  
-   [步骤 2：添加和配置平面文件连接管理器](../integration-services/lesson-1-2-adding-and-configuring-a-flat-file-connection-manager.md)  
  
-   [步骤 3：添加和配置 OLE DB 连接管理器](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
-   [步骤 4：向包添加数据流任务](../integration-services/lesson-1-4-adding-a-data-flow-task-to-the-package.md)  
  
-   [步骤 5：添加和配置平面文件源](../integration-services/lesson-1-5-adding-and-configuring-the-flat-file-source.md)  
  
-   [步骤 6：添加和配置查找转换](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
-   [步骤 7：添加和配置 OLE DB 目标](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
-   [步骤 8：为第 1 课包添加批注并设置格式](../integration-services/lesson-1-8-making-the-lesson-1-package-easier-to-understand.md)  
  
-   [步骤 9：测试第 1 课包](../integration-services/lesson-1-9-testing-the-lesson-1-tutorial-package.md)  
  
## <a name="start-the-lesson"></a>开始课程  
[步骤 1：创建新的 Integration Services 项目](../integration-services/lesson-1-1-creating-a-new-integration-services-project.md)  
  
