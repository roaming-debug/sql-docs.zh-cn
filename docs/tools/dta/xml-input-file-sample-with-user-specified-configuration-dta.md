---
title: 使用用户指定配置的 XML 输入文件示例
description: 本文包含使用用户指定配置的 XML 输入文件示例，其可用于优化工作负荷以使用数据库引擎优化顾问。
titleSuffix: DTA
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: b29c9716-e5c3-4003-9efb-3ade2197b630
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: fc4e0f7e4816a6aec1cdbe41d2d4c84c8af55711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340006"
---
# <a name="xml-input-file-sample-with-user-specified-configuration-dta"></a>使用用户指定配置 (DTA) 的 XML 输入文件示例

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此 XML 输入文件示例使用 **Configuration** 元素来指定用户指定的配置，请将此示例文件复制并粘贴到你喜欢的 XML 编辑器或文本编辑器中。 这样将使您能够执行假设分析。 假设分析过程中将涉及使用 **Configuration** 元素为待优化的数据库指定一组假设的物理设计结构。 然后，可以使用数据库引擎优化顾问基于该假设配置对运行工作负荷进行效果分析，以查看它是否改进了查询处理性能。 此类分析具有的优点是，在评估新配置时不会引起实际实施它的开销。 如果假设配置未达到您期望的性能改进，则可以很容易的重新更改配置并进行分析，直到配置可以达到所需的结果。  
  
 将该示例输入文件复制到编辑工具中后，请将为 **Server**、 **Database**、 **Schema**、 **Table**、 **Workload**、 **TuningOptions** 和 **Configuration** 元素指定的值替换为具体的优化会话的值。 有关可以与这些元素一起使用的所有属性和子元素的详细信息，请参阅 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)。 以下示例只使用了部分可用属性和子元素选项。  
  
## <a name="code"></a>代码  
  
```  
<?xml version="1.0" encoding="utf-16" ?>  
<DTAXML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
  <DTAInput>  
    <Server>  
      <Name>MyServerName</Name>  
<!-- To tune multiple databases, list them and their associated tables in the following section. -->  
      <Database>  
        <Name>MyDatabaseName</Name>  
        <Schema>  
          <Name>MyDatabaseSchemaName</Name>  
<!-- You can list as many tables as necessary in the following section. -->  
          <Table>  
            <Name>MyTableName1</Name>  
          </Table>  
          <Table>  
            <Name>MyTableName2</Name>  
          </Table>  
        </Schema>  
      </Database>  
    </Server>  
    <Workload>  
<!-- The following element specifies a workload file, which can be a trace file or a Transact-SQL script file. -->  
      <File>c:\PathToYourWorkloadFile</File>  
    </Workload>  
    <TuningOptions>  
      <TuningTimeInMin>180</TuningTimeInMin>  
      <StorageBoundInMB>10000</StorageBoundInMB>  
      <FeatureSet>IDX_IV</FeatureSet>  
      <Partitioning>NONE</Partitioning>  
      <KeepExisting>NONE</KeepExisting>  
      <OnlineIndexOperation>OFF</OnlineIndexOperation>  
    </TuningOptions>  
    <Configuration SpecificationMode="Absolute">  
      <Server>  
        <Name>MyServerName</Name>  
          <Database>  
            <Name>MyDatabaseName</Name>  
            <Schema>  
              <Name>MyDatabaseSchemaName</Name>  
                <Table>  
                  <Name>MyTableName1</Name>  
                  <Recommendation>  
                    <Create>  
                      <Index Clustered="true" Unique="false" Online="false" IndexSizeInMB="873.75">  
                        <Name>MyIndexName</Name>  
                        <Column Type="KeyColumn" SortOrder="Ascending">  
                          <Name>MyColumnName1</Name>  
                        </Column>  
                        <Filegroup>MyFileGroupName1</FileGroup>  
                      </Index>  
                    </Create>  
                  </Recommendation>  
                </Table>  
            </Schema>  
          </Database>  
      </Server>  
    </Configuration>  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="comments"></a>注释  
  
-   如果为 **Configuration** 元素指定 **Absolute** 模式 (`Configuration SpecificationMode="Absolute"`)，则将无法删除物理设计结构。  
  
-   不能在 **Configuration** 元素的任何一个模式（ **Relative** 或 **Absolute** ）中创建并删除同一个物理设计结构。  
  
## <a name="see-also"></a>另请参阅  
 [启动并使用数据库引擎优化顾问](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [XML 输入文件引用（数据库引擎优化顾问）](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
