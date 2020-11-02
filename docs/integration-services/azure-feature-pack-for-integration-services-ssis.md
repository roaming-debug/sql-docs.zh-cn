---
description: 用于 Azure 的 Integration Services (SSIS) 功能包
title: 用于 Integration Services (SSIS) 的Azure 功能包 | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.SSIS.AZURE.F1
- SQL14.SSIS.AZURE.F1
ms.assetid: 31de555f-ae62-4f2f-a6a6-77fea1fa8189
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 63e40e66003737798c444f220058feceab69411a
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678925"
---
# <a name="azure-feature-pack-for-integration-services-ssis"></a>用于 Azure 的 Integration Services (SSIS) 功能包

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


用于 Azure 的 SQL Server Integration Services (SSIS) 功能包是一个扩展包，可为 SSIS 提供本页面上列出的组件，用于连接到 Azure 服务、在 Azure 与本地数据源之间传输数据以及处理 Azure 中存储的数据。

[![下载用于 Azure 的 SSIS 功能包](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/download/details.aspx?id=100430)下载

- 对于 SQL Server 2019 - [用于 Azure 的 Microsoft SQL Server 2019 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=100430)
- 对于 SQL Server 2017 - [用于 Azure 的 Microsoft SQL Server 2017 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=54798)
- 对于 SQL Server 2016 - [用于 Azure 的 Microsoft SQL Server 2016 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=49492)
- 对于 SQL Server 2014 - [用于 Azure 的 Microsoft SQL Server 2014 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=47366)
- 对于 SQL Server 2012 - [用于 Azure 的 Microsoft SQL Server 2012 Integration Services 功能包](https://www.microsoft.com/download/details.aspx?id=47367)

下载页面还包含有关先决条件的信息。 在将 Azure 功能包安装到服务器上之前，确保已安装 SQL Server，否则在将功能包部署到服务器上的 SSIS 目录数据库 SSISDB 时，功能包中的组件可能不可用。

## <a name="components-in-the-feature-pack"></a>功能包中的组件
-   连接管理器

    -   [Azure Data Lake Analytics Connection Manager](connection-manager/azure-data-lake-analytics-connection-manager.md)

    -   [Azure Data Lake Store 连接管理器](../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)
    
    -   [Azure HDInsight 连接管理器](../integration-services/connection-manager/azure-hdinsight-connection-manager.md)

    -   [Azure 资源管理器连接管理器](../integration-services/connection-manager/azure-resource-manager-connection-manager.md)
    
    -   [Azure 存储连接管理器](../integration-services/connection-manager/azure-storage-connection-manager.md)

    -   [Azure 订阅连接管理器](../integration-services/connection-manager/azure-subscription-connection-manager.md)
    
-   任务

    -   [Azure Blob 下载任务](../integration-services/control-flow/azure-blob-download-task.md)

    -   [Azure blob 上传任务](../integration-services/control-flow/azure-blob-upload-task.md)

    -   [Azure Data Lake Analytics 任务](control-flow/azure-data-lake-analytics-task.md)

    -   [Azure Data Lake Store 文件系统任务](../integration-services/control-flow/azure-data-lake-store-file-system-task.md)

    -   [Azure HDInsight 创建群集任务](../integration-services/control-flow/azure-hdinsight-create-cluster-task.md)

    -   [Azure HDInsight 删除群集任务](../integration-services/control-flow/azure-hdinsight-delete-cluster-task.md)
    
    -   [Azure HDInsight Hive 任务](../integration-services/control-flow/azure-hdinsight-hive-task.md)

    -   [Azure HDInsight Pig 任务](../integration-services/control-flow/azure-hdinsight-pig-task.md)

    -   [Azure SQL DW 上传任务](../integration-services/control-flow/azure-sql-dw-upload-task.md)

    -   [灵活的文件任务](../integration-services/control-flow/flexible-file-task.md)

-   数据流组件

    -   [Azure Blob 源](../integration-services/data-flow/azure-blob-source.md)

    -   [Azure Blob 目标](../integration-services/data-flow/azure-blob-destination.md)
    
    -   [Azure Data Lake Store 源](../integration-services/data-flow/azure-data-lake-store-source.md)
    
    -   [Azure Data Lake Store 目标](../integration-services/data-flow/azure-data-lake-store-destination.md)

    -   [灵活的文件源](../integration-services/data-flow/flexible-file-source.md)

    -   [灵活的文件目标](../integration-services/data-flow/flexible-file-destination.md)

-   Azure Blob、Azure Data Lake Store 和 Data Lake Storage Gen2 文件枚举器。 请参阅 [Foreach 循环容器](../integration-services/control-flow/foreach-loop-container.md)

## <a name="use-tls-12"></a>使用 TLS 1.2

Azure 功能包使用的 TLS 版本遵循系统 .NET Framework 设置。
若要使用 TLS 1.2，请使用以下两个注册表项下的数据 `1` 添加名称为 `SchUseStrongCrypto` 的 `REG_DWORD` 值。

1. `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319`
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319`

## <a name="dependency-on-java"></a>Java 上的依赖项

必须使用 Java，才能结合使用 ORC/Parquet 文件格式和 Azure Data Lake Store/灵活的文件连接器。  
Java 版本的体系结构（32/64 位）应与要使用的 SSIS 运行时的体系结构一致。
已测试以下 Java 版本。

- [Zulu OpenJDK 8u192](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Oracle Java SE 运行时环境 8u192](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

### <a name="set-up-zulus-openjdk"></a>安装 Zulu OpenJDK

1. 下载并提取安装 zip 包。
2. 从命令提示符处，运行 `sysdm.cpl`。
3. 在“高级”选项卡上，选择“环境变量”   。
4. 在“系统变量”部分中，选择“新建”   。
5. 输入变量名称 `JAVA_HOME` 。
6. 选择“浏览目录”，导航到已提取的文件夹，然后选择 `jre` 子文件夹  。
   然后选择“确定”，“变量值”将自动进行填充   。
7. 选择“确定”，关闭“新建系统变量”对话框   。
8. 选择“确定”，关闭“环境变量”对话框   。
9. 选择“确定”以关闭“系统属性”对话框   。

> [!TIP]
> 如果使用 Parquet 格式出现错误，指示“调用 java 时出现错误，消息：‘java.lang.OutOfMemoryError:Java 堆空间’”  ，则可以添加一个环境变量 `_JAVA_OPTIONS` 以调整 JVM 的最小/最大堆大小。
>
>![jvm 堆](media/azure-feature-pack-jvm-heap-size.png)
>
> 示例：将变量 `_JAVA_OPTIONS` 的值设置为 `-Xms256m -Xmx16g` 。 标志 Xms 指定 Java 虚拟机 (JVM) 的初始内存分配池，而 Xmx 指定最大内存分配池。 这意味着 JVM 初始内存为 `Xms` ，并且能够使用的最多内存为 `Xmx` 。 默认最小值为 64MB，最大值为 1G。

### <a name="set-up-zulus-openjdk-on-azure-ssis-integration-runtime"></a>在 Azure-SSIS Integration Runtime 上设置 Zulu 的 OpenJDK

这应该通过 Azure-SSIS Integration Runtime 的[自定义设置界面](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)完成。
假设使用 `zulu8.33.0.1-jdk8.0.192-win_x64.zip`。
可以按以下方式组织 blob 容器：

~~~
main.cmd
install_openjdk.ps1
zulu8.33.0.1-jdk8.0.192-win_x64.zip
~~~

作为入口点，`main.cmd` 触发 PowerShell 脚本 `install_openjdk.ps1` 的执行，后面的脚本进而会相应地提取 `zulu8.33.0.1-jdk8.0.192-win_x64.zip` 并设置 `JAVA_HOME`。

**main.cmd**

~~~
powershell.exe -file install_openjdk.ps1
~~~

> [!TIP]
> 如果使用 Parquet 格式并出现错误，指示“调用 java 时出现错误，消息：‘java.lang.OutOfMemoryError:Java 堆空间’”  ，则可以在 `main.cmd` 中添加命令以调整 JVM 的最小/最大堆大小。 示例：
> ~~~
> setx /M _JAVA_OPTIONS "-Xms256m -Xmx16g"
> ~~~
> 标志 Xms 指定 Java 虚拟机 (JVM) 的初始内存分配池，而 Xmx 指定最大内存分配池。 这意味着 JVM 初始内存为 `Xms` ，并且能够使用的最多内存为 `Xmx` 。 默认最小值为 64MB，最大值为 1G。

**install_openjdk.ps1**

~~~
Expand-Archive zulu8.33.0.1-jdk8.0.192-win_x64.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\zulu8.33.0.1-jdk8.0.192-win_x64\jre", "Machine")
~~~

### <a name="set-up-oracles-java-se-runtime-environment"></a>安装 Oracle Java SE 运行时环境

1. 下载并运行 exe 安装程序。
2. 按照安装程序说明完成安装。

## <a name="scenario-processing-big-data"></a>方案：处理大数据
 使用 Azure Connector 可完成以下大数据处理工作：

1.  使用 Azure Blob 上载任务可将输入数据上载到 Azure Blob 存储。

2.  使用 Azure HDInsight 创建群集任务可创建 Azure HDInsight 群集。 如果要使用自己的群集，则此步骤是可选的。

3.  使用 Azure HDInsight Hive 任务或 Azure HDInsight Pig 任务可在 Azure HDInsight 群集上调用 Pig 或 Hive 作业。

4.  使用 Azure HDInsight 删除群集任务可在使用之后删除 HDInsight 群集（如果在步骤 #2 中创建了按需 HDInsight 群集）。

5.  使用 Azure HDInsight Blob 下载任务可从 Azure Blob 存储下载 Pig/Hive 输出数据。

![显示 SSIS Azure 连接器大数据方案的屏幕截图。](../integration-services/media/ssis-azureconnector-bigdatascenario.png)
 
## <a name="scenario-managing-data-in-the-cloud"></a>方案：管理云中的数据
 在 SSIS 包中使用 Azure blob 目标，将输出数据写入 Azure blob 存储区；或使用 Azure blob 源，从 Azure blob 存储区中读取数据。

![显示从 OLE DB 源到 Azure Blob 目标的数据流的屏幕截图。](../integration-services/media/ssis-azureconnector-cloudarchive-1.png)
 
 ![显示从 Azure Blob 源到 OLE DB 目标的数据流的屏幕截图。](../integration-services/media/ssis-azureconnector-cloudarchive-2.png)

 结合使用 Foreach 循环容器和 Azure blob 枚举器，处理多个 blob 文件中的数据。

![显示控制流 Foreach 循环容器的屏幕截图。](../integration-services/media/ssis-azureconnector-cloudarchive-3.png)

## <a name="release-notes"></a>发行说明

### <a name="version-1190"></a>版本 1.19.0

#### <a name="improvements"></a>改进

1. 向 Azure 存储连接管理器添加了对共享访问签名身份验证的支持。

### <a name="version-1180"></a>版本 1.18.0

#### <a name="improvements"></a>改进

1. 对于灵活的文件任务，有三项改进：(1) 添加了对复制/删除操作的通配符支持；(2) 用户可以启用/禁用对删除操作的递归搜索；(3) 复制操作的目标文件名可以为空，以保留源文件名。

### <a name="version-1170"></a>版本 1.17.0

这是仅针对 SQL Server 2019 发布的修补程序版本。

#### <a name="bugfixes"></a>Bug 修复

1. 在 Visual Studio 2019 中执行修复且面向 SQL Server 2019 时，灵活文件任务/源/目标可能会失败，并显示错误消息：`Attempted to access an element as a type incompatible with the array.`
1. 在 Visual Studio 2019 中执行修复并面向 SQL Server 2019 时，使用 ORC/Parquet 格式的灵活的文件源/目标可能会失败，并显示错误消息：`Microsoft.DataTransfer.Common.Shared.HybridDeliveryException: An unknown error occurred. JNI.JavaExceptionCheckException.`

### <a name="version-1160"></a>版本 1.16.0

#### <a name="bugfixes"></a>Bug 修复

1. 在某些情况下，包执行报告“错误：无法加载文件或程序集‘Newtonsoft.Json, Version=11.0.0.0, Culture=neutral, PublicKeyToken=30ad4fe6b2a6aeed’ 或其依赖项之一。”

### <a name="version-1150"></a>版本 1.15.0

#### <a name="improvements"></a>改进

1. 向灵活的文件任务添加删除文件夹/文件操作
1. 在灵活的文件源中添加外部/输出数据类型转换函数

#### <a name="bugfixes"></a>Bug 修复

1. 在某些情况下，Data Lake Storage Gen2 的测试连接发生故障，并显示错误消息“尝试访问作为与数组不兼容的类型的元素”
1. 恢复对 Azure 存储仿真器的支持