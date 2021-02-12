---
description: 执行 SSMA 控制台 (SybaseToSQL)
title: 执行 SSMA 控制台 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Database Connection Commands
- Sybase Console,Manageability Commands
- Sybase Console,Migration Commands
- Sybase Console,Migration Preparation Command
- Sybase Console,Project Commands
- Sybase Console,Report Commands
- Sybase Console,Script File Commands
- Sybase Console,Script Generation Commands
ms.assetid: ea8950b7-fabc-4aa4-89f8-9573a2617d70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7eed4ea61ada5979dbecdb106e5954620ae9c81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079368"
---
# <a name="executing-the-ssma-console-sybasetosql"></a>执行 SSMA 控制台 (SybaseToSQL)
Microsoft 为你提供了一组可靠的脚本文件命令来执行和控制 SSMA 活动。 后续部分将详细介绍相同的内容。  
  
## <a name="script-file-commands"></a>脚本文件命令  
控制台应用程序使用本部分中所列举的某些标准脚本文件命令。  
  
## <a name="project-commands"></a>项目命令  
项目命令用于处理创建项目、打开、保存和退出项目。  
  
### <a name="create-new-project"></a>create-new-project  
此命令创建新的 SSMA 项目。  
  
-   `project-folder` 指示已创建项目的文件夹。  
  
-   `project-name` 指示项目的名称。 {string}  
  
-   `overwrite-if-exists`可选属性指示是否应覆盖现有项目。 变量  
  
-   `project-type:`可选的特性。 指示项目类型，即 "sql-server-2005" 项目或 "sql-server-2008" 项目或 "sql-server 2012" 项目或 "sql-2014" 项目或 "sql-azure" 项目。 默认值为 "sql-server-2008"。  
  
**语法示例：**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>" (optional)  
  
   project-type="<sql-server-2008/sql-server-2005/sql-server-2012/sql-server-2014/sql-azure>"  
/>  
```  
默认情况下，属性 "覆盖-exists" 为 **false** 。  
  
默认情况下，属性 "项目类型" 为 **sql-server-2008** 。  
  
### <a name="open-project"></a>打开-项目  
此命令打开项目。

-   `project-folder` 指示已创建项目的文件夹。 如果指定的文件夹不存在，则该命令将失败。  {string}  
  
-   `project-name` 指示项目的名称。 如果指定的项目不存在，则该命令将失败。  {string}  
  
**语法示例：**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
> [!NOTE]  
> SSMA for SAP ASE 控制台应用程序支持向后兼容性。 可以使用它打开 SSMA 的早期版本创建的项目。  
  
### <a name="save-project"></a>save-project  
此命令保存迁移项目。  
  
**语法示例：**  
  
```xml  
<save-project/>  
```  
  
### <a name="close-project"></a>关闭项目  
此命令关闭迁移项目。  
  
**语法示例：**  
  
```xml  
<close-project   
  if-modified="<save/error/ignore>"   (optional)  
/>  
```  
特性 "if-modified" 是可选的，默认情况下 **忽略** 。  
  
## <a name="database-connection-commands"></a>数据库连接命令  
数据库连接命令有助于连接到数据库。  
  
> [!NOTE]  
> - 控制台中不支持 UI 的 **浏览** 功能。  
> - 有关 "创建脚本文件" 的详细信息，请参阅 [创建脚本文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md)。  
  
### <a name="connect-source-database"></a>连接-源-数据库  
此命令执行与源数据库的连接，并加载源数据库的高级元数据，而不是所有元数据。
  
如果无法建立与源的连接，则会生成错误，并且控制台应用程序将停止执行。
  
服务器定义是从为服务器连接文件或脚本文件的 server 部分中的每个连接定义的 name 属性中检索的。  
  
**语法示例：**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="force-load-sourcetarget-database"></a>强制加载-源/目标-数据库  
此命令加载源元数据，它可用于脱机处理迁移项目。  
  
如果无法建立与源/目标的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
此命令需要一个或多个元数据库节点作为命令行参数。  
  
**语法示例：**  
  
```xml  
<force-load metabase="<source/target>" >  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
  
### <a name="reconnect-source-database"></a>重新连接-源-数据库  
此命令重新连接到源数据库，但不会加载任何元数据，而与 "连接源-数据库" 命令不同。  
  
如果 (无法建立与源) 的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**语法示例：**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="connect-target-database"></a>连接目标-数据库  
此命令连接到目标 SQL Server 数据库，并加载目标数据库的高级元数据，而不是完全加载元数据。  
  
如果无法建立与目标的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
服务器定义是从为服务器连接文件或脚本文件的 server 部分中的每个连接定义的 name 属性中检索的。  
  
**语法示例：**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
  
### <a name="reconnect-target-database"></a>重新连接-目标-数据库  
  
此命令将重新连接到目标数据库，但不会加载任何元数据，这与连接目标数据库命令不同。  
  
如果无法建立与目标) 的 (连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**语法示例：**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-commands"></a>报告命令  
报表命令生成各种 SSMA 控制台活动的性能报告。  
  
### <a name="generate-assessment-report"></a>生成-评估-报表  
  
此命令生成源数据库的评估报告。  
  
如果在执行此命令之前未执行源数据库连接，则会生成错误并退出控制台应用程序。  
  
在执行命令期间，无法连接到源数据库服务器也会导致终止控制台应用程序。  
  
-   `conversion-report-folder:` 指定可在其中存储评估报表的文件夹。  (可选特性)   
  
-   `object-name:` 指定 () 为评估报表生成的对象 (支持单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名称属性中的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-summary-report-to:` 指定将生成报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **AssessmentReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<generate-assessment-report  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  assessment-report-folder="<folder-name>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
或  
  
```xml  
<generate-assessment-report  
  
  assessment-report-folder="<folder-name>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
<metabase-object object-name="<object-name>"  
  
        object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-commands"></a>迁移命令  
迁移命令将目标数据库架构转换为源架构，并将数据迁移到目标服务器。  
  
### <a name="convert-schema"></a>转换-架构  
此命令执行从源到目标架构的架构转换。  
  
如果在执行此命令之前未执行源或目标数据库连接，或者与源或目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
-   `conversion-report-folder:` 指定可在其中存储评估报表的文件夹。  (可选特性)   
  
-   `object-name:` 指定 (s) 的源对象，该对象用于转换架构 (支持单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名称属性中的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-summary-report-to:` 指定将生成汇总报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **SchemaConversionReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<convert-schema  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  write-summary-report-to="<file-name/folder-name>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder-name>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```  
或  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder-name>"         (optional)  
  
  conversion-report-overwrite="<true/false>"> (optional)  
  
  <metabase-object object-name="<object-name>"  
  
    object-type="<object-category>"/>  
  
</convert-schema>  
```  
  
### <a name="migrate-data"></a>迁移-数据  
此命令将源数据迁移到目标。  
  
-   `object-name:` 指定 () 的源对象，该对象用于迁移数据 (支持单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名称属性中的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `write-summary-report-to:` 指定将生成报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **DataMigrationReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    -   `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    -   `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>">  
  
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
  
  report-errors="<true/false>" verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-command"></a>迁移准备命令  
迁移准备命令启动源数据库和目标数据库之间的架构映射。  
  
> [!NOTE]  
> 迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
### <a name="map-schema"></a>映射架构  
此命令提供源数据库到目标架构的架构映射。  
  
-   `source-schema` 指定要迁移的源架构。  
  
-   `sql-server-schema` 指定源架构将迁移到的目标架构。  
  
**语法示例：**  
  
```xml  
<map-schema source-schema="<source-schema>"  
  
sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-commands"></a>可管理性命令  
可管理性命令有助于将目标数据库对象与源数据库同步。  
  
> [!NOTE]  
> 迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
### <a name="synchronize-target"></a>同步-目标  
此命令将目标对象与目标数据库进行同步。  
 
如果对源数据库执行此命令，则会遇到错误。  
  
如果在执行此命令之前未执行目标数据库连接，或者与目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
-   `object-name:` 指定要与目标数据库同步)  (的目标对象， (支持单个对象名或) 的组对象名称。  
  
-   `object-type:` 指定对象名称属性中的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `on-error:` 指定是否将同步错误指定为警告或错误。 针对出错的可用选项：  
  
    -   报表-总警告  
  
    -   报告-每个警告  
  
    -   fail-脚本  
  
-   `report-errors-to:` 指定同步操作的错误报告位置 (可选属性) 。 如果仅指定了文件夹路径，则创建按名称 **TargetSynchronizationReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<synchronize-target  
  
object-name="<object-name>"  
  
on-error="<report-total-as-warning/  
  
report-each-as-warning/  
  
fail-script>" (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
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
  
### <a name="refresh-from-database"></a>从数据库刷新  
此命令刷新数据库中的源对象。  
  
如果对目标数据库执行此命令，则会生成错误。  
  
此命令需要一个或多个元数据库节点作为命令行参数。  
  
-   `object-name:` 指定 (s) 从源数据库中进行刷新的源对象， (支持单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `on-error:` 指定是否将刷新错误作为警告或错误调用。 针对出错的可用选项：  
  
    -   报表-总警告  
  
    -   报告-每个警告  
  
    -   fail-脚本  
  
-   `report-errors-to:` 指定刷新操作的错误报告位置 (可选属性) 。 如果仅指定了文件夹路径，则创建按名称 **SourceDBRefreshReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  on-error="<report-total-as-warning/  
  
             report-each-as-warning/  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
或  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  object-type="<object-category>" />  
```  
或  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-commands"></a>脚本生成命令  
脚本生成命令执行双重任务：它们可帮助将控制台输出保存到脚本文件中，并根据指定的参数将 T-sql 输出记录到控制台或文件中。  
  
### <a name="save-as-script"></a>另存为脚本  
此命令用于将对象的脚本保存到元数据库 = 目标时提到的文件中。 这是同步命令的替代方法，因为我们获取了脚本并在目标数据库上执行了相同的命令。  
  
此命令需要一个或多个元数据库节点作为命令行参数。  
  
-   `object-name:` 指定要保存其脚本 ( (s) 对象，支持单个对象名称或) 的组对象名称。  
  
-   `object-type:` 指定对象名称属性中的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
-   `metabase:` 指定它是源元数据库还是目标元数据库。  
  
-   `destination:` 指定必须在其中保存脚本的路径或文件夹。 如果未指定文件名，则将提供格式 (object_name 属性值) 的文件名。
  
-   `overwrite:` 如果为 true，则它将覆盖相同的文件名（如果存在）。 它的值可以为 true/false)  (。  
  
**语法示例：**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  destination="<file-name/folder-name>"  
  
  overwrite="<true/false>"   (optional)  
  
/>  
```  
或  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file-name/folder-name>"  
  
    <metabase-object object-name="<object-name>"  
  
                     object-type="<object-category>"/>  
  
</save-as-script>  
```  
  
### <a name="convert-sql-statement"></a>convert-sql 语句
此命令转换 SQL 语句。  
  
-   `context` 指定架构名称。  
  
-   `destination` 指定是否应将输出存储在文件中。  
  
    如果未指定此属性，则转换后的 T-sql 语句将显示在控制台上。  (可选特性)   
  
-   `conversion-report-folder` 指定可在其中存储评估报表的文件夹。  (可选特性)   
  
-   `conversion-report-overwrite` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
-   `write-converted-sql-to` 指定文件 (或应存储已转换 T-sql 的) 文件夹路径。 如果文件夹路径与属性一起指定，则 `sql-files` 每个源文件都具有在指定文件夹下创建的相应目标 t-sql 文件。 当文件夹路径与属性一起指定时 `sql` ，转换后的 t-sql 会写入到指定文件夹下名为 Result 的文件中。  
  
-   `sql` 指定要转换的 Sybase sql 语句，可以使用 ";" 分隔一条或多条语句  
  
-   `sql-files` 指定必须转换为 T-sql 代码的 sql 文件的路径。  
  
-   `write-summary-report-to` 指定将在其中生成汇总报表的路径。 如果仅提到文件夹路径，则创建按名称 **ConvertSQLReport.XML** 文件。  (可选特性)   
  
    创建摘要报表包含两个进一步的子类别，即：  
  
    -   报告错误 (= "true/false"，默认值为 "false" (可选属性) # A3。  
  
    -   详细 (= "true/false"，默认值为 "false" (可选属性) # A3。  
  
此命令需要一个或多个元数据库节点作为命令行参数。  
  
**语法示例：**  
  
```  
<convert-sql-statement  
  
       context="<database-name>.<schema-name>"  
  
        conversion-report-folder="<folder-name>"  
  
        conversion-report-overwrite="<true/false>"  
  
        write-summary-report-to="<file-name/folder-name>"   (optional)  
  
        verbose="<true/false>"   (optional)  
  
        report-errors="<true/false>"   (optional)  
  
        destination="<stdout/file>"   (optional)  
  
        write-converted-sql-to ="<file-name/folder-name>"  
  
        sql="SELECT 1 FROM DUAL;">  
  
    <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
或  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         write-summary-report-to="<file-name/folder-name>"   (optional)  
  
         verbose="<true/false>"   (optional)  
  
         report-errors="<true/false>"   (optional)  
  
         destination="<stdout/file>"   (optional)  
  
         write-converted-sql-to ="<file-name/folder-name>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
或  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-steps"></a>后续步骤  
有关命令行选项的信息，请参阅 [SSMA 控制台中的命令行选项 (AccessToSQL) ](../access/command-line-options-in-ssma-console-accesstosql.md)。  
  
有关示例控制台脚本文件的信息，请参阅使用 [示例控制台脚本文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/working-with-the-sample-console-script-files-sybasetosql.md)  
  
下一步取决于项目要求：  
  
-   若要指定密码或导出/导入密码，请参阅 [管理密码 &#40;SybaseToSQL&#41;](../../ssma/sybase/managing-passwords-sybasetosql.md)。  
  
-   有关生成报表的详细 &#40;，请参阅 [&#41;中生成报表 ](../../ssma/sybase/generating-reports-sybasetosql.md)。  
  
-   有关控制台中问题的疑难解答，请参阅 [排查 &#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md)。  
  
