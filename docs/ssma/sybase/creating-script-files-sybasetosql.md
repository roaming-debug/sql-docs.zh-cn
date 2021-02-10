---
description: 创建脚本文件 (SybaseToSQL)
title: 创建脚本文件 (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Configuring Settings
- Sybase Console,Script Commands
- Sybase Console,Script File Validation
- Sybase Console,Server Connection Parameters
ms.assetid: e6baf106-abbd-4200-b3de-33b4b4f1b294
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b3f417752ef7de6ab5c4fef077eb9771d7454996
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057192"
---
# <a name="creating-script-files-sybasetosql"></a>创建脚本文件 (SybaseToSQL)
启动 SSMA 控制台应用程序之前的第一个步骤是创建脚本文件，并在需要时创建变量值文件和服务器连接文件。  
  
脚本文件可以分为三个部分，即即：  
  
1.  **config：** 允许用户为控制台应用程序设置配置参数。  
  
2.  **服务器：** 允许用户设置源/目标服务器定义。 也可以在单独的服务器连接文件中使用。  
  
3.  **脚本-命令：** 允许用户执行 SSMA 工作流命令。  
  
下面将详细介绍每个部分：  
  
## <a name="configuring-sybase-console-settings"></a>配置 Sybase 控制台设置  
脚本的配置显示在控制台脚本文件中。  
  
如果在配置节点中指定了任一元素，则这些元素将设置为全局设置，即它们适用于所有脚本命令。 如果用户想要替代全局设置，还可以在脚本命令部分的每个命令中设置这些配置元素。  
  
用户可配置的选项包括：  
  
1.  **输出窗口提供程序：** 如果将 "取消消息" 特性设置为 "true"，则命令特定消息不会显示在控制台上。 属性说明如下所示：  
  
    -   destination：指定是否需要将输出打印到文件或 stdout。 默认情况下，此值为 false。  
  
    -   文件名：文件路径 (可选) 。  
  
    -   抑制消息：抑制控制台上的消息。 默认情况下，此值为 "false"。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <output-window  
  
        suppress-messages="<true/false>"   (optional)  
  
        destination="<file/stdout>"        (optional)  
  
        file-name="<file-name>"            (optional)  
  
       />  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <output-window  
  
         suppress-messages="<true/false>"   (optional)  
  
         destination="<file/stdout>"        (optional)  
  
         file-name="<file-name>"            (optional)  
  
       />  
  
    </...All commands...>  
    ```  
  
2.  **数据迁移连接提供程序：** 此项指定要将哪个源/目标服务器视为数据迁移。  源-使用情况-上次使用指示上次使用的源服务器用于数据迁移。 类似目标-使用-上次使用指示上次使用的目标服务器用于数据迁移。 用户还可以通过使用属性 "源-服务器" 或 "目标服务器" 指定服务器 (源或目标) 。  
  
    只能使用一个或另一个指定的属性，例如：  
  
    -   源-使用-上次使用 = "true" (默认) 或源-server = "source_servername"  
  
    -   目标-使用-上次使用 = "true" (默认) 或目标-server = "target_servername"  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <data-migration-connection   source-use-last-used="true"  
  
                                   target-server="<target-server-unique-name>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <migrate-data>  
  
      <data-migration-connection   source-server="<source-server-unique-name>"  
  
                                   target-use-last-used="true"/>  
  
    </migrate-data>  
    ```  
  
3.  **用户输入弹出窗口：** 这允许在从数据库加载对象时处理错误。 用户提供输入模式，并在发生错误时，控制台将在用户指定的情况下继续。  
  
    模式包括：  
  
    -   **ask-用户-** 提示用户继续 ( "是" ) 或 ( "否" ) 时出现错误。  
  
    -   **错误-** 控制台将显示错误并停止执行。  
  
    -   **继续-** 控制台将继续执行。  
  
    默认模式为 " **错误**"。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!-- Connect to target database -->  
  
    <connect-target-database server="<target-server-unique-name>">  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </connect-target-database>  
    ```  
  
4.  **重新连接提供程序：** 这允许用户设置连接失败的重新连接设置大。 这可为源服务器和目标服务器设置此项。  
  
    重新连接模式如下：  
  
    -   重新连接到上次使用的-服务器：如果连接未处于活动状态，则它会尝试重新连接到最近5次使用的最后一个服务器。  
  
    -   生成-错误：如果连接未处于活动状态，则会生成错误。  
  
    默认模式为 " **生成-错误**"。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <reconnect-manager  on-source-reconnect="<reconnect-to-last-used-server/generate-an-error>"  
  
                          on-target-reconnect="<reconnect-to-last-used-server/generate-an-error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!--synchronization-->  
  
    <synchronize-target>  
  
      <reconnect-manager on-target-reconnect="reconnect-to-last-used-server"/>  
  
    </synchronize-target>  
    ```  
    *or*  
  
    ```xml  
    <!--data migration-->  
  
    <migrate-data server="<target-server-unique-name>">  
  
      <reconnect-manager  
  
        on-source-reconnect="reconnect-to-last-used-server"  
  
        on-target-reconnect="generate-an-error"/>  
  
    </migrate-data>  
    ```  
  
5.  **转换器覆盖提供程序：** 这使用户能够处理目标元数据库上已存在的对象。 可能的操作包括：  
  
    -   错误：控制台显示错误并停止执行。  
  
    -   overwrite：覆盖现有对象值。 默认情况下，此操作已完成。  
  
    -   skip：控制台将跳过数据库中已存在的对象  
  
    -   ask-user：提示用户输入 ( "是"/"否" )   
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <convert-schema object-name="<object-name>">  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </convert-schema>  
    ```  
  
6.  **必备组件提供程序：** 这使用户能够处理处理命令所需的任何先决条件。 默认情况下，严格模式为 "false"。 如果设置为 "true"，则会生成异常以满足先决条件。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <prerequisites strict-mode="<true/false>"/>  
  
    </output-providers>  
    ```  
  
7.  **停止操作：** 在中间操作期间，如果用户想要停止操作，则可以使用 **"Ctrl + C"** 热键。 用于 Sybase 控制台的 SSMA 将等待操作完成，并终止控制台执行。  
  
    如果用户想立即停止执行，则可以再次按 **"Ctrl + C"** 热键来 SSMA 控制台应用程序的突然终止  
  
8.  **进度提供程序：** 通知每个控制台命令的进度。 此项已默认禁用。 进度报告属性包括：  
  
    -   关闭  
  
    -   每隔1%  
  
    -   每隔2%  
  
    -   每5%  
  
    -   每隔10%  
  
    -   每隔20%  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <progress-reporting enable="<true/false>"           (optional)  
  
                          report-messages="<true/false>"  (optional)  
  
                          report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off" (optional)/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <progress-reporting  
  
        enable="<true/false>"          (optional)  
  
        report-messages="<true/false>" (optional)  
  
        report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off (optional)/>  
  
    </...All commands...>  
    ```  
  
9. **记录器详细级别：** 设置日志详细级别。 这对应于 UI 中的 "所有类别" 选项。 默认情况下，日志详细级别为 "错误"。  
  
    记录器级别选项包括：  
  
    -   严重错误：只记录致命错误消息。  
  
    -   错误：只记录错误和严重错误消息。  
  
    -   警告：记录除 "调试" 和 "信息" 消息以外的所有级别。  
  
    -   信息：记录除调试消息以外的所有级别。  
  
    -   调试：记录的所有消息级别。  
  
    > [!NOTE]  
    > 必需的消息记录在任何级别。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </...All commands...>  
    ```  
  
10. **替代加密密码：** 如果为 "true"，则在服务器连接文件或脚本文件的 "服务器定义" 部分中指定的明文密码将覆盖受保护存储区中存储的加密密码（如果存在）。 如果未指定明文形式的密码，则系统会提示用户输入密码。  
  
    这里出现两种情况：  
  
    1.  如果 override 选项为 **false**，则搜索顺序为 "受保护的存储- &gt; 脚本文件- &gt; 服务器连接文件- &gt; 提示用户"。  
  
    2.  如果 override 选项为 **true**，则搜索顺序为 "脚本文件- &gt; 服务器连接文件- &gt; 提示用户"。  
  
    **示例：**  
  
    ```xml  
    <output-providers>  
  
      <encrypted-password override="<true/false>"/>  
  
    </output-providers>  
    ```  
  
不可配置的选项是：  
  
-   **最大重新连接尝试：** 如果已建立的连接超时或由于网络故障而中断，则需要重新连接服务器。 允许重新连接尝试最多执行 **5** 次重试，此时，控制台将自动执行重新连接。 自动重新连接的功能可以减少重新运行脚本的工作量。  
  
## <a name="server-connection-parameters"></a>服务器连接参数  
服务器连接参数可以在脚本文件中或服务器连接文件中定义。 有关更多详细信息，请参阅 [创建服务器连接文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md) 部分  
  
## <a name="script-commands"></a>脚本命令  
脚本文件包含一系列 XML 格式的迁移工作流命令。 SSMA 控制台应用程序按脚本文件中显示的命令顺序处理迁移。  
  
例如，Sybase 数据库中特定表的典型数据迁移遵循数据库架构表的层次结构 &gt; &gt; 。  
  
成功执行脚本文件中的所有命令后，SSMA 控制台应用程序将退出并将控件返回给用户。 脚本文件的内容具有更多或更少的静态，其中的变量信息包含在 [变量值文件](creating-variable-value-files-sybasetosql.md) 中，或在脚本文件的单独节中用于变量值。  
  
**示例：**  
  
```xml  
<!--Sample of script file commands -->  
  
<ssma-script-file>  
  
  <script-commands>  
  
    <create-new-project project-folder="<project-folder>"  
  
                        project-name="<project-name>"  
  
                        overwrite-if-exists="<true/false>"/>  
  
    <connect-source-database server="<source-server-unique-name>"/>  
  
    <save-project/>  
  
    <close-project/>  
  
  </script-commands>  
  
</ssma-script-file>  
```  
产品目录的 "示例控制台脚本" 文件夹中提供了包含3个脚本文件的模板 (用于执行各种方案) 、变量值文件和服务器连接文件：  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   VariableValueFileSample.xml  
  
-   ServersConnectionFileSample.xml  
  
更改中显示的用于关联性的参数后，可以 (文件) 执行模板。  
  
脚本命令的完整列表，可在[执行 SSMA 控制台 &#40;SybaseToSQL](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md)中找到&#41;  
  
## <a name="script-file-validation"></a>脚本文件验证  
用户可以根据 "架构" 文件夹中提供的架构定义文件 **的 "2ssconsolescriptschema"** ，轻松验证其脚本文件  
  
## <a name="next-step"></a>下一步  
操作控制台的下一步是 [&#40;SybaseToSQL&#41;创建变量值文件 ](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[创建变量值文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
  
