---
description: SQL Server 导入和导出向导中的数据类型映射
title: SQL Server 导入和导出向导中的数据类型映射 | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 669be403-cb17-4b12-bbbf-e7a74003c4b6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 946bb57a3d821186ebcca132539713cf515ab20f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122776"
---
# <a name="data-type-mapping-in-the-sql-server-import-and-export-wizard"></a>SQL Server 导入和导出向导中的数据类型映射

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导中，可在新的目标表和目标文件中设置列的名称、数据类型和数据类型属性，但不能指定列值的自定义转换。 因此，从源到目标的数据类型内置映射非常重要。  
  
##  <a name="how-does-the-wizard-map-data-types-between-source-and-destination"></a><a name="wizardMapping"></a> 向导如何在源和目标之间映射数据类型？
向导使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 安装的映射文件将数据类型从一个数据库系统或版本映射到另一个数据库系统或版本。 例如，它可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型映射到 Oracle 数据类型。 默认情况下，XML 格式的映射文件安装在以下文件夹中。
-   **C:\Program Files\Microsoft SQL Server\130\DTSMappingFiles\\** （针对 64 位）
-   **C:\Program Files (x86)\Microsoft SQL Server\130\DTSMappingFiles\\** （针对 32 位）。  
  
 如果编辑现有映射文件，或者向文件夹中添加新的映射文件，则必须关闭并重新打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导或 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ，以便加载新的或更改过的映射文件。  
 
## <a name="you-can-change-an-existing-mapping-file"></a>可以更改现有的映射文件
如果业务需要在数据类型之间进行不同的映射，则可以更新映射文件以更改向导所使用的映射。 例如，在你将数据从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 传输到 DB2 时，如果要让  nchar 数据类型映射到 DB2 GRAPHIC 数据类型而不是 DB2 VARGRAPHIC 数据类型[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，可将“SqlClientToIBMDB2.xml”映射文件中的“nchar”映射更改为使用 GRAPHIC 而不是 VARGRAPHIC。  
  
## <a name="you-can-add-a-new-mapping-file"></a>可以添加新的映射文件
[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会在许多常用的源和目标组合之间安装映射。 你也可以向 **MappingFiles** 目录添加新的映射文件，以支持其他源和目标。 新的映射文件必须符合已发布的 XSD 架构，并且必须在源和目标的唯一组合之间进行映射。 映射文件的架构 **DataTypeMapping.xsd** 发布在 [此处](https://schemas.microsoft.com/sqlserver/2008/07/IntegrationServices/DataTypeMapping/DataTypeMapping.xsd)。
 
## <a name="sample-mapping-file"></a>示例映射文件
以下是 XML 映射文件的一部分，该文件从 SQL Server 数据类型（更具体地说，是从用于 SQL Server 的 .NET Framework 数据提供程序所使用的数据类型）映射到 Oracle 数据类型。 例如，你可以看到 SQL Server **int** 数据类型映射到 Oracle **INTEGER** 数据类型。
  
```xml  
  
<dtm:DataTypeMappings  
    xmlns:dtm="https://www.microsoft.com/SqlServer/Dts/DataTypeMapping.xsd"   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    SourceType="System.Data.SqlClient.SqlConnection"   
    MinSourceVersion="*"   
    MaxSourceVersion="*"   
    DestinationType="MSDAORA;OraOLEDB.Oracle;System.Data.OracleClient.OracleConnection"   
    MinDestinationVersion="08.*"   
    MaxDestinationVersion="*">  
  
    <!-- smallint -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>smallint</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
    <!-- int -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>int</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
        ...  
  
</dtm:DataTypeMappings>  
  
```  

