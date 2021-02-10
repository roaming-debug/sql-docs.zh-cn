---
description: '执行 SSMA 控制台 (AccessToSQL) '
title: 执行 SSMA 控制台 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aa1bf665-8dc0-4259-b36f-46ae67197a43
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cf369ee1104f4b686dc0e7b6ab4bbde7bab458de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044697"
---
# <a name="executing-the-ssma-console-accesstosql"></a>执行 SSMA 控制台 (AccessToSQL) 
Microsoft 为你提供了一组可靠的脚本文件命令和命令行选项来执行和控制 SSMA 活动。 后续部分将详细介绍相同的内容。  
  
## <a name="project--script-file-commands"></a>项目脚本文件命令  
项目命令用于处理创建项目、打开、保存和退出项目。  
  
**命令**  
  
创建-新建-项目：创建新的 SSMA 项目。  
  
**脚本**  
  
-   `project-folder` 指示已创建项目的文件夹。  
  
-   `project-name` 指示项目的名称。 {string}  
  
-   `overwrite-if-exists`可选属性指示是否应覆盖现有项目。 变量  
  
-   `project-type` 是一个可选属性。  以下选项适用于项目类型：  
  
    -   sql-服务器-2005  
  
    -   sql-服务器-2008  
  
    -   sql-服务器-2012  
  
    -   sql-服务器-2014  
  
    -   sql-服务器-2016  
  
    -   sql-azure  
  
    默认值为 "sql-server-2008"。  
  
**示例：**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>"  
  
  project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"  
  
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
**注意：** SSMA For Access Console 应用程序支持向后兼容性。 可以打开 SSMA 以前版本创建的项目。  
  
**命令**  
  
保存-项目：保存迁移项目。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<save-project/>  
```  
**命令**  
  
关闭项目：关闭迁移项目。  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<close-project  
  
  if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
特性 "if-modified" 是可选的，默认情况下 **忽略** 。  
  
## <a name="database-connection-script-file-commands"></a>数据库连接脚本文件命令  
数据库连接命令有助于连接到数据库。  
  
控制台中不支持 UI 的 **浏览** 功能。  
  
连接到 SQL Azure 时， **windows 身份验证** 和 **端口** 参数不适用。  
  
有关 "创建脚本文件" 的详细信息，请参阅 [创建脚本文件 &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md)。  
  
**命令**
  
连接-源-数据库  
  
- 执行与源数据库的连接，并加载源数据库的高级元数据，而不是所有元数据。  
  
- 如果无法建立与源的连接，则会生成错误，并使控制台应用程序停止执行  
  
**脚本**
  
服务器定义是从为服务器连接文件或脚本文件的 server 部分中的每个连接定义的名称属性中检索的。  
  
**语法示例：**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**命令**  
  
负载访问-数据库：用于加载 access 数据库文件  
  
**脚本**  
  
**语法示例：**  
  
```xml  
<load-access-database  database-file="<Access-database>"/>  
```  
或  
  
```xml  
<load-access-database>  
  
  <access-database database-file="<Access-database1>"/>  
  
  <access-database database-file="<Access-database2>"/>  
  
</load-access-database>  
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
<force-load  
  
  object-name="<object-name>"  
  
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
  
- 重新连接到源数据库，但不会加载任何元数据，而与 "连接源-数据库" 命令不同。  
  
- 如果 (无法建立与源) 的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**
  
**语法示例：**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```

**命令**
  
连接目标-数据库  
  
- 连接到目标 SQL Server 或 SQL Azure 数据库，并加载目标数据库的高级元数据，而不是完全加载元数据。  
  
- 如果无法建立与目标的连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**
  
服务器定义从为服务器连接文件或脚本文件的服务器部分中的每个连接定义的名称属性中检索  
  
**语法示例：**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  

**命令**
  
重新连接-目标-数据库  
  
- 重新连接到目标数据库，但不加载任何元数据，这与连接目标数据库命令不同。  
  
- 如果无法建立与目标) 的 (连接，则会生成错误，并且控制台应用程序将停止执行。  
  
**脚本**
  
**语法示例：**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  

## <a name="report-script-file-commands"></a>报表脚本文件命令

报表命令生成各种 SSMA 控制台活动的性能报告

**命令**
  
生成-评估-报表  
  
- 在源数据库上生成评估报告。  
  
- 如果在执行此命令之前未执行源数据库连接，则会生成错误并退出控制台应用程序。  
  
- 在执行命令期间，无法连接到源数据库服务器也会导致控制台应用程序结束。  
  
**脚本**

- `assessment-report-folder:` 指定可存储评估报表的文件夹。 (可选属性)   
  
- `object-name:` 指定 () 用于评估报表生成的对象， (可以将单个对象名称或组对象名称) 。  
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `assessment-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
- `write-summary-report-to:` 指定将在其中生成报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **AssessmentReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    - `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    - `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<generate-assessment-report  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  
  write-summary-report-to="<file>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  conversion-report-folder="<folder>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```

或
  
```xml  
<generate-assessment-report  
  
  conversion-report-folder="<folder>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
    <metabase-object object-name="ssma.Procedures"  
  
        object-type="category"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>迁移脚本文件命令

迁移命令将目标数据库架构转换为源架构，并将数据迁移到目标服务器。  
  
迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
**命令**

转换-架构  
  
- 执行从源到目标架构的架构转换。  
  
- 如果在执行此命令之前未执行源或目标数据库连接，或者与源或目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
**脚本**

- `conversion-report-folder:` 指定可存储评估报表的文件夹。 (可选属性)   
  
- `object-name:` 指定 (s) 用于转换架构的源对象 (它可以有单个对象名或) 的组对象名称。  
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `conversion-report-overwrite:` 指定是否覆盖评估报告文件夹（如果已存在）。  
  
    **默认值：** false。  (可选特性)   
  
- `write-summary-report-to:` 指定将在其中生成报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **SchemaConversionReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    - `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    - `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<convert-schema  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  write-summary-report-to="<filepath/folder>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```

或  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder>"         (optional)  
  
  conversion-report-overwrite="<true/false>"  (optional)  
  
  <metabase-object object-name="ssma.Procedures"  
  
    object-type="category"/>  
  
</convert-schema>  
```

**命令**
  
迁移-数据  
  
- 将源数据迁移到目标。  
  
**脚本**
  
- `object-name:` 指定) 用于迁移数据的源对象 ( (它可以有单个对象名或) 的组对象名称。  
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `write-summary-report-to:` 指定将在其中生成报表的路径。  
  
    如果仅提到文件夹路径，则按名称 **DataMigrationReport &lt; n &gt; 。创建 XML** 。  (可选特性)   
  
    报表创建包含两个进一步的子类别：  
  
    - `report-errors` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
    - `verbose` (= "true/false"，默认值为 "false" (可选属性) # A3  
  
**语法示例：**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true">  
  
    <metabase-object object-name="ssma.TT1"/>  
  
    <metabase-object object-name="ssma.TT2"/>  
  
    <metabase-object object-name="ssma.TT3"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```

或  
  
```xml  
<migrate-data  
  
  object-name="ssma.Tables"  
  
  object-type="category"  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true"/>  
```

**命令**
  
链接表：此命令将源 (访问) 表链接到目标表。  
  
**脚本**

**语法示例：**  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</link-tables>  
```

或  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</link-tables>  
```

**命令**
  
取消链接表：此命令从目标表中将源 (访问) 表取消链接。  
  
**脚本**
  
**语法示例：**  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</unlink-tables>  
```

或  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</unlink-tables>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>迁移准备脚本文件命令

迁移准备命令启动源数据库和目标数据库之间的架构映射。  
  
**命令**
  
映射架构：从源数据库到目标架构的架构映射。  
  
**脚本**
  
- `source-schema` 指定要迁移的源架构。  
  
- `sql-server-schema` 指定要将其迁移到的目标架构。  
  
**语法示例：**  
  
```xml  
<map-schema source-schema="source-schema"  
  
            sql-server-schema="target-schema"/>  
```  
  
## <a name="manageability-commands"></a>可管理性命令

可管理性命令有助于将目标数据库对象与源数据库同步。  
  
迁移命令的默认控制台输出设置为 "完全" 输出报告，没有详细的错误报告：仅限源对象树根节点上的 "摘要"。  
  
**命令**

同步-目标  
  
- 将目标对象与目标数据库同步。  
  
- 如果对源数据库执行此命令，则会出现错误。  
  
- 如果在执行此命令之前未执行目标数据库连接，或者与目标数据库服务器之间的连接在命令执行过程中失败，则会生成错误并退出控制台应用程序。  
  
**脚本**
  
- `object-name:` 指定要与目标数据库同步)  (的目标对象 (它可以有单个对象名或) 的组对象名称。  
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `on-error:` 指定是否将同步错误指定为警告或错误。 针对出错的可用选项：  
  
    - 报表-总警告  
  
    - 报告-每个警告  
  
    - fail-脚本  
  
- `report-errors-to:` 指定同步操作的错误报告位置 (可选属性) 如果仅指定了文件夹路径，则创建按名称 **TargetSynchronizationReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<synchronize-target  
  
  object-name="ots_triggers.dbo"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

或  
  
```xml  
<synchronize-target  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"/>  
```

或  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="ssma.dbo.TT1"/>  
  
  <metabase-object object-name="ssma.dbo.TT2"/>  
  
  <metabase-object object-name="ssma.dbo.TT3"/>  
  
</synchronize-target>  
```

**命令**
  
从数据库刷新  
  
- 刷新数据库中的源对象。  
  
- 如果对目标数据库执行此命令，则会生成错误。  
  
**脚本**
  
需要一个或多个元数据库节点作为命令行参数。  
  
- `object-name:` 为源对象指定 () 的源对象， (可以将单个对象名称或组对象名称) 。  
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `on-error:` 指定是否将刷新错误指定为警告或错误。 针对出错的可用选项：  
  
    - 报表-总警告  
  
    - 报告-每个警告  
  
    - fail-脚本  
  
- `report-errors-to:` 指定刷新操作的错误报告位置 (可选属性) 如果仅指定了文件夹路径，则创建按名称 **SourceDBRefreshReport.XML** 文件。  
  
**语法示例：**  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.TT1"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

或  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.Procedures"  
  
  object-type="category"/>  
```

或  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="ssma.TT_1"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>脚本生成脚本文件命令

脚本生成命令有助于将控制台输出保存到脚本文件中。  
  
**命令**
  
另存为脚本  
  
用于将对象的脚本保存到在元数据库 = 目标时提到的文件中，这是同步命令的替代方法，在其中获取脚本并在目标数据库上执行相同的。  
  
**脚本**
  
需要一个或多个元数据库节点作为命令行参数。  
  
- `object-name:` 指定要保存其脚本的对象 () 。  (可以有单个对象名或组对象名)   
  
- `object-type:` 指定对象名属性中指定的对象的类型 (如果指定了对象类别，则对象类型将为 "category" ) 。  
  
- `metabase:` 指定是源元数据库还是目标元数据库。  
  
- `destination:` 指定要在其中保存脚本的路径或文件夹;如果未提供文件名，则格式为的文件名 (object_name 属性值) 为 out。  
  
- `overwrite:` 如果为 true，则它将覆盖相同的文件名。 它的值可以为 true/false)  (。  
  
**语法示例：**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"  
  
  destination="<file/folder>"  
  
  overwrite="<true/false>"             (optional)  
  
/>  
```

或  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file/folder>"  
  
    <metabase-object object-name="ssma.dbo.Procedures"  
  
                     object-type="category"/>  
  
</save-as-script>  
```  
  
## <a name="next-steps"></a>后续步骤

有关命令行选项的信息，请参阅 [SSMA 控制台中的命令行选项 &#40;AccessToSQL&#41;](../../ssma/access/command-line-options-in-ssma-console-accesstosql.md) 。  
  
有关示例控制台脚本文件的信息，请参阅使用 [示例控制台脚本 FILESEXECUTING SSMA 控制台 &#40;AccessToSQL&#41;](../../ssma/access/working-sample-console-script-filesexecuting-ssma-console-accesstosql.md)  
  
下一步取决于项目要求：  
  
- 若要指定密码或导出/导入密码，请参阅 [管理密码 &#40;AccessToSQL&#41;](../../ssma/access/managing-passwords-accesstosql.md)。  
  
- 有关生成报表的详细 &#40;，请参阅 [&#41;中生成报表 ](../../ssma/access/generating-reports-accesstosql.md)。  
  
- 有关控制台中问题的疑难解答，请参阅 [排查 &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md)。  
