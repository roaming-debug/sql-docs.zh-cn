---
title: 使用 SSMS 的提示和技巧
description: 了解如何使用 SQL Server Management Studio 注释和取消注释代码、缩进文本、筛选对象、访问错误日志，以及查找 SQL Server 实例名称。
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
helpviewer_keywords:
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- Find SQL Server Instance
- find instance name
- find sql server instance name
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: 4e8e26ce2aa9edc696bf801293213ed720c7c65e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063622"
---
# <a name="tips-and-tricks-for-using-sql-server-management-studio-ssms"></a>使用 SQL Server Management Studio (SSMS) 的提示和技巧

本文介绍了一些使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS) 的提示和技巧。 本文介绍如何： 

> [!div class="checklist"]
> * 注释/取消注释 Transact-SQL (T-SQL) 文本
> * 缩进文本
> * 在对象资源管理器中筛选对象
> * 访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志
> * 查找 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称

## <a name="prerequisites"></a>先决条件

若要测试本文提供的步骤，必须有 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、对 SQL Server 的访问权限，以及 AdventureWorks 数据库。 

* 安装 [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md)。
* 安装 [[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads)。
* 下载 [AdventureWorks 示例数据库](https://github.com/Microsoft/sql-server-samples/releases)。 有关在 SSMS 中还原数据库的说明，请参阅[还原数据库](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)。 

## <a name="commentuncomment-your-t-sql-code"></a>注释/取消注释 T-SQL 代码

可使用工具栏中的“注释”按钮注释和取消注释部分文本  。 系统不会执行已注释掉的文本。

1. 打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。

2. 连接到 SQL Server。

3. 打开“新建查询”窗口。

4. 将以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码粘贴到文本窗口。

    ```sql
    USE master
        GO

        -- Drop the database if it already exists
        IF  EXISTS (
            SELECT name 
                FROM sys.databases 
                WHERE name = N'TutorialDB'
                )

        DROP DATABASE TutorialDB
        GO

        CREATE DATABASE TutorialDB
        GO

        ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
        GO
   ```

5. 突出显示文本的“更改数据库”部分，然后选择工具栏中的“注释”按钮   ： 

    ![“注释”按钮](media/ssms-tricks/comment.png)
6. 选择“执行”运行取消注释的文本部分  。 

7. 突出显示除“更改数据库”命令之外的所有内容，然后选择“注释”按钮   ：

    ![注释全部内容](media/ssms-tricks/commenteverything.png)

    > [!NOTE]
    > 注释的文本的键盘快捷方式是 CTRL + K，CTRL + C  。

8. 突出显示文本的“更改数据库”部分，然后选择工具栏中的“取消注释”按钮以取消注释   ：

    ![取消注释文本](media/ssms-tricks/uncomment.png)

    > [!NOTE]
    > 取消注释的文本的键盘快捷方式是 CTRL + K，CTRL + U  。

9. 选择“执行”运行取消注释的文本部分  。

## <a name="indent-your-text"></a>缩进文本

可使用工具栏上的缩进按钮增加或减少文本的缩进。

1. 打开“新建查询”窗口。

2. 将以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码粘贴到文本窗口：

    ```sql
    USE master
      GO

      --Drop the database if it already exists
      IF  EXISTS (
            SELECT name
                FROM sys.databases
                WHERE name = N'TutorialDB'
              )

      DROP DATABASE TutorialDB
      GO

      CREATE DATABASE TutorialDB
      GO

      ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
      GO
    ```

3. 突出显示文本的“更改数据库”部分，然后选择工具栏上的“增加缩进”按钮以向前移动此文本   ：

    ![增加缩进](media/ssms-tricks/increaseindent.png)

4. 再次突出显示文本的“更改数据库”部分，然后选择“减少缩进”按钮以向后移动此文本   。

    ![减少缩进](media/ssms-tricks/decreaseindent.png)

## <a name="filter-objects-in-object-explorer"></a>在对象资源管理器中筛选对象

在具有多个对象的数据库中，可以使用筛选功能来搜索特定表、视图等。本节介绍如何筛选表，但可在对象资源管理器中的任何其他节点中使用以下步骤：

1. 连接到 SQL Server。

2. 展开“数据库” > “AdventureWorks” > “表”    。 此时将显示数据库中的所有表。

3. 右键单击“表”，然后选择“筛选器” > “筛选器设置”    ：

    ![筛选器设置](media/ssms-tricks/filtersettings.png)

4. 在“筛选器设置”窗口中，可以修改以下某些筛选器设置  ：
    * 按名称筛选： 

      ![按名称筛选](media/ssms-tricks/filterbyname.png)

    * 按架构筛选： 

      ![按架构筛选](media/ssms-tricks/filterbyschema.png)

5. 若要清除筛选器，请右键单击“表”，然后选择“删除筛选器”   。

    ![删除筛选器](media/ssms-tricks/removefilter.png)

## <a name="access-your-sql-server-error-log"></a>访问 SQL Server 错误日志

错误日志是一个文件，其中包含 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中所发生操作的相关详细信息。 可浏览和查询 SSMS 中的错误日志。 错误日志是位于磁盘上的日志文件。

### <a name="open-the-error-log-in-ssms"></a>在 SSMS 中打开错误日志

1. 连接到你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  

2. 展开“管理” > “SQL Server 日志”   。 

3. 右键单击“当前”错误日志，然后选择“查看 SQL Server 日志”   ：

    ![在 SSMS 中查看错误日志](media/ssms-tricks/viewerrorloginssms.png)

### <a name="query-the-error-log-in-ssms"></a>在 SSMS 中查看查询日志

1. 连接到 SQL Server。

2. 打开“新建查询”窗口。

3. 将以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码粘贴到查询窗口：

     ```sql
       sp_readerrorlog 0,1,'Server process ID'
      ```

4. 将单引号中的文本修改为要搜索的文本。

5. 执行查询然后查看结果：

    ![查询错误日志](media/ssms-tricks/queryerrorlog.png)

### <a name="find-the-error-log-location-if-youre-connected-to-sql-server"></a>如果连接到 SQL Server，请查找错误日志位置

1. 连接到你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。

2. 打开“新建查询”窗口。

3. 将以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码粘贴到查询窗口，然后选择“执行”：

     ```sql
        SELECT SERVERPROPERTY('ErrorLogFileName') AS 'Error log file location'  
      ```

4. 结果将显示文件系统中错误日志的位置： 

    ![通过查询查找错误日志](media/ssms-tricks/finderrorlogquery.png)

### <a name="find-the-error-log-location-if-you-cant-connect-to-sql-server"></a>如果无法连接到 SQL Server，请查找错误日志位置

你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志的路径可能有所不同，具体取决于你的配置设置。 可以在 SQL Server 配置管理器内的启动参数中找到错误日志位置的路径。 请按照以下步骤来找到标识 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志位置的相关启动参数。 你的路径可能与以下指示的路径有所不同  。

1. 打开“SQL Server 配置管理器”。

2. 展开“服务”  。

3. 右键单击你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，然后选择“属性”：

    ![Configuration Manager 服务器属性](media/ssms-tricks/serverproperties.PNG)

4. 选择“启动参数”选项卡  。

5. 在“现有参数”区域中，“-e”后面的路径是错误日志的位置  ： 

    ![错误日志](media/ssms-tricks/errorlog.png)

    此位置中包含多个错误日志文件。 当前错误日志的文件名以 *.log 结尾。 以前的日志文件的文件名以数字结尾。 每次重新启动 SQL Server 时都会创建一个新日志。

6. 在记事本中打开 errorlog.log 文件。

## <a name="find-sql-server-instance-name"></a>查找 SQL Server 实例名称

在连接到 SQL Server 之前和之后，有几个选项可用于查找 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的名称。  

### <a name="before-you-connect-to-sql-server"></a>连接到 SQL Server 之前

1. 按照步骤查找[磁盘上的 SQL Server 错误日志](#find-the-error-log-location-if-you-cant-connect-to-sql-server)。 你的路径可能与下图中的路径有所不同。

2. 在记事本中打开 errorlog.log 文件。

3. 搜索文本“服务器名称是”  。

    单引号中列出的所有内容都是将连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称：

    ![在错误日志中查找服务器名称](media/ssms-tricks/servernameinlog.png)

    名称的格式为 HOSTNAME\INSTANCENAME。 如果只看到了主机名，然后已安装了默认实例，则实例名称是 MSSQLSERVER。 连接到默认实例时，只需输入主机名以连接到 SQL Server。  

### <a name="when-youre-connected-to-sql-server"></a>连接到 SQL Server 时

连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，可在三个位置找到服务器名称： 

1. 服务器名称将在“对象资源管理器”中列出：

    ![对象资源管理器中的 SQL Server 实例名称](media/ssms-tricks/nameinobjectexplorer.png)
2. 服务器名称将在查询窗口中列出：

    ![查询窗口中的 SQL Server 实例名称](media/ssms-tricks/nameinquerywindow.png)
3. 服务器名称将在“属性”中列出  。
    * 在“视图”菜单上，选择“属性窗口”   ：

      ![属性窗口中的 SQL Server 实例名称](media/ssms-tricks/nameinproperties.png)

### <a name="if-youre-connected-to-an-alias-or-availability-group-listener"></a>如果连接到别名或可用性组侦听程序

如果连接到别名或可用性组侦听程序，则将在“对象资源管理器”和“属性”中显示该信息。 在这种情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 名称可能不是显而易见的，并且必须进行查询：

1. 连接到 SQL Server。

2. 打开“新建查询”窗口。

3. 将以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码粘贴到窗口中：

      ```sql
       select @@Servername
     ```

4. 查看查询结果，确定连接到的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称： 

    ![查询 SQL Server 名称](media/ssms-tricks/queryservername.png)

## <a name="next-steps"></a>后续步骤

熟悉 SSMS 的最好方式是进行实践演练。 这些教程  和操作说明  文章可帮助你使用 SSMS 的各种功能。  这些文章教你如何管理 SSMS 组件，以及如何查找常用功能。

* [连接到实例和查询实例](../quickstarts/ssms-connect-query-sql-server.md)
* [脚本](scripting-ssms.md)
* [在 SSMS 中使用模板](../template/templates-ssms.md)
* [SSMS 配置](ssms-configuration.md)