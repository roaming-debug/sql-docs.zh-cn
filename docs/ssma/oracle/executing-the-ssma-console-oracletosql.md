---
description: 执行 SSMA 控制台 (OracleToSQL)
title: 执行 SSMA 控制台 (OracleToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle SSMA Console
- Script File Commands, Script Generation Commands,Manageability Commands
- Script File Commands,Project Commands
ms.assetid: 7228ccba-c69f-4b4c-8664-01a2750183c5
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 97bbd0ae40fff78df4142437d32456f3c542f8e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044417"
---
# <a name="executing-the-ssma-console-oracletosql"></a>执行 SSMA 控制台 (OracleToSQL)
Microsoft 为你提供了一组可靠的脚本文件命令来执行和控制 SSMA 活动。 控制台应用程序使用本部分中所列举的某些标准脚本文件命令。  
  
## <a name="project-script-file-commands"></a>项目脚本文件命令  
项目命令用于处理创建项目、打开、保存和退出项目。  
  
**命令**  
  
create-new-project  
                  ：创建新的 SSMA 项目。  
  
**脚本**  
  
-   `project-folder` 指示已创建项目的文件夹。  
  
-   `project-name` 指示项目的名称。 {string}  
  
-   `overwrite-if-exists`可选属性指示是否应覆盖现有项目。 变量  
  
-   `project-type:`可选的特性。 指示项目类型，即 "sql-server-2005" 项目或 "sql-server-2008" 项目或 "sql-server 2012" 项目或 "sql-2014" 项目或 "sql-azure"。 默认值为 "sql-server-2014"。  
  
**示例：**  
  
```xml  
<create-new-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
   overwrite-if-exists="<true/false>"   (optional)  
  
   project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014>"   (optional)  
  
/>  
```  
默认情况下，属性 "覆盖-exists" 为 **false** 。  
  
默认情况下，属性 "项目类型" 为 **sql-server-2008** 。  
  
**命令**  
  
打开项目：打开现有项目。  
  
**脚本**  
  
-   `project-folder` 指示已创建项目的文件夹。 如果指定的文件夹不存在，则该命令将失败。  {string}  
  
-   `project-name` 指示项目的名称。 如果指定的项目不存在，则该命令将失败。  {string}  
  
**语法示例：**  
  
```xml  
<open-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
/>  
```  
SSMA for Oracle Console 应用程序支持向后兼容性。 你将能够打开以前版本的 SSMA 创建的项目。  
  
**命令**  
  
save-project  
  
保存迁移项目。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<save-project/>  
```  
**命令**  
  
关闭项目  
  
关闭迁移项目。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<close-project  
  
   if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
  
## <a name="database-connection-script-file-commands"></a>数据库连接脚本文件命令  
数据库连接命令有助于连接到数据库。  
  
-   控制台中不支持 UI 的 **浏览** 功能。  
  
-   有关 "创建脚本文件" 的详细信息，请参阅 [创建脚本文件 &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md)。  
  
**命令**  
  
连接-源-数据库  
  
-   执行与源数据库的连接，并加载源数据库的高级元数据，而不是所有元数据。  
  
-   如果无法建立与源的连接，则会生成错误，并使控制台应用程序停止执行  
  
**脚本**  
  
服务器定义是从为服务器连接文件或脚本文件的 server 部分中的每个连接定义的名称属性中检索的。  
  
**语法示例：**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**命令**  
  
强制加载-源/目标-数据库  
  
-   加载源元数据。  
  
-   适用于脱机处理迁移项目。  
  
-   如果无法建立与源/目标的连接，则会生成错误，并使控制台应用程序停止进一步执行  
  
**脚本**  
  
需要一个或多个元数据库节点作为命令行参数。  
  
**语法示例：**  
  
```xml  
<force-load object-name="<object-name>"  
  
  metabase="<source/target>"/>  
```  
或  
  
```xml  
<force-load>  
  
   <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**命令**  
  
重新连接-源-数据库  
  
-   重新连接到源数据库，但不会加载任何元数据，这与连接源数据库命令不同。  
  
-   如果 (无法建立与源) 的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
**命令**  
  
连接目标-数据库  
  
-   连接到目标 SQL Server 数据库，并加载目标数据库的高级元数据，而不是完全加载元数据。  
  
-   如果无法建立与目标的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**  
  
服务器定义从为服务器连接文件或脚本文件的服务器部分中的每个连接定义的名称属性中检索  
  
**语法示例：**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
**命令**  
  
重新连接-目标-数据库  
  
-   重新连接到目标数据库，但不加载任何元数据，这与连接目标数据库命令不同。  
  
-   如果无法建立与目标) 的 (连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-script-file--commands"></a>报表脚本文件命令  
报表命令生成各种 SSMA 控制台活动的性能报告。  
  
**命令**  
  
生成-评估-报表  
  
-   在源数据库上生成评估报告。  
  
-   如果在执行此命令之前未执行源数据库连接，则会生成错误并退出控制台应用程序。  
  
-   在执行命令期间，无法连接到源数据库服务器也会导致终止控制台应用程序。  
  
**脚本**  
  
-   `conversion-report-folder:` 指定可存储评估报表的文件夹。 (可选特性)   
  
-   `object-name:` 指定 () 为评估报表生成的对象 (它可以具有单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-summary-report-to:` 指定将在其中生成汇总报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **AssessmentReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建还有另外两个子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   assessment-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
或  
  
```xml  
<generate-assessment-report  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
>  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>迁移脚本文件命令  
迁移命令将目标数据库架构转换为源架构，并将数据迁移到目标服务器。  
  
迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
**命令**  
  
转换-架构  
  
-   执行从源到目标架构的架构转换。  
  
-   如果在执行此命令之前未执行源或目标数据库连接，或者与源或目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
**脚本**  
  
-   `conversion-report-folder:` 指定可存储评估报表的文件夹。 (可选特性)   
  
-   `object-name:` 指定 (s) 用于转换架构的源对象 (它可以具有单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-summary-report-to:` 指定将在其中生成汇总报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **SchemaConversionReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建还有另外两个子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<convert-schema  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
或  
  
```xml  
<convert-schema  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</convert-schema>  
```  
**命令**  
  
迁移-数据  
  
将源数据迁移到目标。  
  
**脚本**  
  
-   `conversion-report-folder:` 指定可存储评估报表的文件夹。 (可选特性)   
  
-   `object-name:` 指定) 用于迁移数据的源对象 ( (它可以具有单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-summary-report-to:` 指定将在其中生成汇总报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **DataMigrationReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建还有另外两个子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<migrate-data  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="<true/false>"  
  
   verbose="<true/false>">  
  
      <metabase-object object-name="<object-name>"/>  
  
      <metabase-object object-name="<object-name>"/>  
  
      <metabase-object object-name="<object-name>"/>  
  
      <data-migration-connection  
  
         source-use-last-used="true"/source-server="<server-unique-name>"  
  
         target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```  
或  
  
```xml  
<migrate-data  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="<true/false>"  
  
   verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>迁移准备脚本文件命令  
迁移准备命令启动源数据库和目标数据库之间的架构映射。  
  
**命令**  
  
映射架构  
  
将源数据库映射到目标架构的架构。  
  
将源数据迁移到目标。  
  
**脚本**  
  
-   `source-schema` 指定要迁移的源架构。  
  
-   `sql-server-schema` 指定要将其迁移到的目标架构。  
  
**语法示例：**  
  
```xml  
<map-schema  
  
   source-schema="<source-schema>"  
  
   sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-script-file-commands"></a>可管理性脚本文件命令  
可管理性命令有助于将目标数据库对象与源数据库同步。 迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
**命令**  
  
同步-目标  
  
-   将目标对象与目标数据库同步。  
  
-   如果对源数据库执行此命令，则会遇到错误。  
  
-   如果在执行此命令之前未执行目标数据库连接，或者与目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
**脚本**  
  
-   `object-name:` 指定要与目标数据库同步)  (的目标对象 (它可以具有单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `on-error:` 指定是否将同步错误指定为警告或错误。 针对出错的可用选项：  
  
    -   报表-总警告  
  
    -   报告-每个警告  
  
    -   fail-脚本  
  
-   `report-errors-to:` 指定同步操作的错误报告位置 (可选属性) 如果仅指定了文件夹路径，则创建按名称 **TargetSynchronizationReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name/folder-name>"   (optional)  
  
/>  
```  
或  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"/>  
```  
或  
  
```xml  
<synchronize-target>  
  
   <metabase-object object-name="<object-name>"/>  
  
   <metabase-object object-name="<object-name>"/>  
  
   <metabase-object object-name="<object-name>"/>  
  
</synchronize-target>  
```  
**命令**  
  
从数据库刷新  
  
-   刷新数据库中的源对象。  
  
-   如果对目标数据库执行此命令，则会生成错误。  
  
**脚本**  
  
需要一个或多个元数据库节点作为命令行参数。  
  
-   `object-name:` 为源对象指定 () 的源对象， (可以将单个对象名称或组对象名称) 。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `on-error:` 指定是否将刷新错误指定为警告或错误。 针对出错的可用选项：  
  
    -   报表-总警告  
  
    -   报告-每个警告  
  
    -   fail-脚本  
  
-   `report-errors-to:` 指定刷新操作的错误报告位置 (可选属性) 如果仅指定了文件夹路径，则创建按名称 **SourceDBRefreshReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name/folder-name>"   (optional)  
  
/>  
```  
或  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"/>  
```  
或  
  
```xml  
<refresh-from-database>  
  
   <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>脚本生成脚本文件命令  
脚本生成命令执行双重任务：它们有助于将控制台输出保存到脚本文件中;并根据指定的参数将 T-sql 输出记录到控制台或文件中。  
  
**命令**  
  
另存为脚本  
  
用于将对象的脚本保存到在元数据库 = 目标时提到的文件中，这是同步命令的替代方法，在其中获取脚本并在目标数据库上执行相同的。  
  
**脚本**  
  
需要一个或多个元数据库节点作为命令行参数。  
  
-   `object-name:` 指定要保存其脚本的对象 () 。  (可以有单个对象名或组对象名)   
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `metabase:` 指定是否 ithe 源或目标元数据库。  
  
-   `destination:` 指定要在其中保存脚本的路径或文件夹。如果未提供文件名，则格式 (object_name 属性值) 为 out。  
  
-   `overwrite:` 如果为 true，则它将覆盖相同的文件名。 它的值可以为 true/false)  (。  
  
**语法示例：**  
  
```xml  
<save-as-script  
  
   metabase="<source/target>"  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   destination="<file/folder>"  
  
   overwrite="<true/false>"   (optional)  
  
/>  
```  
或  
  
```xml  
<save-as-script  
  
   metabase="<source/target>"  
  
   destination="<file/folder>"  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</save-as-script>  
```  
**命令**  
  
convert-sql 语句  
  
-   `context` 指定架构名称。  
  
-   `destination` 指定是否应将输出存储在文件中。  
  
    如果未指定此属性，则转换后的 T-sql 语句将显示在控制台上。  (可选特性)   
  
-   `conversion-report-folder` 指定可存储评估报表的文件夹。 (可选特性)   
  
-   `conversion-report-overwrite` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-converted-sql-to` 指定文件 (或) 文件夹路径，在其中存储已转换的 T-sql。 如果文件夹路径与属性一起指定，则 `sql-files` 每个源文件都将具有在指定文件夹下创建的相应目标 t-sql 文件。 当文件夹路径与属性一起指定时 `sql` ，转换后的 t-sql 会写入到指定文件夹下名为 Result 的文件中 **。**  
  
-   `sql` 指定要转换的 Oracle sql 语句，可以使用 ";" 分隔一条或多条语句  
  
-   `sql-files` 指定必须转换为 T-sql 代码的 sql 文件的路径。  
  
-   `write-summary-report-to` 指定将在其中生成报表的路径。 如果仅提到文件夹路径，则创建按名称 **ConvertSQLReport.XML** 文件。  (可选特性)   
  
    报表创建还有另外两个子类别，即。：  
  
    -   报告错误 (= "true/false"，默认值为 "false" (可选属性) # A3。  
  
    -   详细 (= "true/false"，默认值为 "false" (可选属性) # A3。  
  
**脚本**  
  
需要一个或多个元数据库节点作为命令行参数。  
  
**语法示例：**  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="<stdout/file>"   (optional)  
  
   file-name="<file-name>"  
  
   sql="SELECT 1 FROM DUAL;">  
  
   <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
或  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>" (optional)  
  
   verbose="<true/false>" (optional)  
  
   report-errors="<true/false>"  
  
   destination="<stdout/file>"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql-files="<folder-name>\*.sql" />  
```  
或  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   sql-files="<folder-name>\*.sql" />  
```  
  
## <a name="next-step"></a>下一步  
有关命令行选项的信息，请参阅 [SSMA 控制台中的命令行选项 &#40;OracleToSQL&#41;](../../ssma/oracle/command-line-options-in-ssma-console-oracletosql.md) 。  
  
有关示例控制台脚本文件的信息，请参阅使用 [示例控制台脚本文件 &#40;OracleToSQL&#41;](../../ssma/oracle/working-with-the-sample-console-script-files-oracletosql.md)  
  
下一步取决于项目要求：  
  
-   若要指定密码或导出/导入密码，请参阅 [管理密码 &#40;OracleToSQL&#41;](../../ssma/oracle/managing-passwords-oracletosql.md)。  
  
-   有关生成报表的详细 &#40;，请参阅 [&#41;中生成报表 ](../../ssma/oracle/generating-reports-oracletosql.md)。  
  
-   有关控制台中问题的疑难解答，请参阅 [排查 &#40;OracleToSQL&#41;](../../ssma/oracle/troubleshooting-oracletosql.md)。  
  
