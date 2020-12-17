---
title: 使用 dta 命令提示实用工具
description: 了解 dta 命令提示实用工具除了包含 SQL Server 数据库引擎优化顾问提供的功能之外，还包含哪些功能。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 30f27f4d-8852-4b12-ba62-57f63e496f1d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 53eacd0177862b81d32b44616dc67cc0beb90f79
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489147"
---
# <a name="lesson-3-using-the-dta-command-prompt-utility"></a>第 3 课：使用 dta 命令提示实用工具
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**dta** 命令提示实用工具除了包含数据库引擎优化顾问提供的功能之外，还包含其他功能。  
  
通过数据库引擎优化顾问 XML 架构，您可以使用自己喜爱的 XML 工具创建实用工具的输入文件。 该架构随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 一起安装，可在 C:\Program Files (x86)\Microsoft SQL Server\110\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd 中找到。  
  
数据库引擎优化顾问 XML 架构也可通过 [此 Microsoft 网站](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)在线获得。  
  
数据库引擎优化顾问 XML 架构在设置优化选项时可提供更大的灵活性。 例如，可通过它执行“假设分析”。 “假设分析”包括：为要优化的数据库指定一组现有的假设物理设计结构，然后使用数据库引擎优化顾问对该数据库进行分析，以判定此假设物理设计是否能改善查询处理性能。 此类分析具有的优点是，在评估新配置时不会引起实际实施它的开销。 如果假设物理设计不能提供预期的性能改善，则可以方便地进行更改和重新分析，直到获得能够满足需要的配置为止。  
  
此外，通过结合使用数据库引擎优化顾问 XML 架构和 **dta** 命令提示实用工具，可以将数据库引擎优化顾问功能加入到脚本中，并可将其与其他数据库设计工具一起使用。  
  
使用数据库引擎优化顾问的 XML 输入功能不在本课程的讨论范围之内。  
  
此任务指导你如何从命令提示符下启动 **dta** 实用工具，查看其帮助，以及使用它优化工作负荷。 此任务将使用在数据库引擎优化顾问图形用户界面 (GUI) 练习[优化工作负载](lesson-2-using-database-engine-tuning-advisor.md#tuning-a-workload)中创建的工作负载 MyScript.sql  
  
本教程使用 AdventureWorks2017 示例数据库。 出于安全原因，默认情况下不安装该示例数据库。 若要安装示例数据库，请参阅 [安装 SQL Server 示例和示例数据库](../../samples/adventureworks-install-configure.md)。  
  
以下任务将指导你打开命令提示符，启动 **dta** 命令提示实用工具，查看其语法帮助，并优化在 [优化工作负荷](./lesson-2-using-database-engine-tuning-advisor.md?view=sql-server-ver15)中创建的一个简单工作负荷 MyScript.sql。  

## <a name="prerequisites"></a>先决条件 

若要完成本教程，需要 SQL Server Management Studio、针对运行 SQL Server 的服务器的访问权限以及 AdventureWorks 数据库。

- 安装 [SQL Server 2017 Developer Edition。](https://www.microsoft.com/sql-server/sql-server-downloads)
- 下载 [AdventureWorks2017 示例数据库。](../../samples/adventureworks-install-configure.md)


此处提供在 SSMS 中还原数据库的说明：[还原数据库。](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)

  >[!NOTE]
  > 本教程适用于熟悉如何使用 SQL Server Management Studio 和基本数据库管理任务的用户。 

## <a name="access-dta-command-prompt-utility-help-menu"></a>访问 DTA 命令提示实用工具的“帮助”菜单
  
  
1.  在“开始”菜单中，依次指向“所有程序”、“附件”，再单击“命令提示符”。  
  
2.  在命令提示符下，键入以下命令，再按 Enter 键：  
  
    ```  
    dta -? | more  
    ```  
  
    该命令的 `| more` 部分是可选的。 但是，使用该选项可以逐页查看实用工具的语法帮助。 按 Enter 键可以按行翻阅帮助文本，按空格键可按页翻阅。  

  ![使用 DTA cmd 实用工具的帮助](media/dta-tutorials/dta-cmd-help.png)

## <a name="tune-simple-workload-using-the-dta-command-prompt-utility"></a>使用 DTA 命令提示实用工具优化简单的工作负载  


  
1.  在命令提示符下，导航到存储 MyScript.sql 文件的目录。  
  
2.  在命令提示符下，键入以下命令，然后按 Enter 键运行该命令并启动优化会话（请注意，实用工具分析命令时区分大小写）：  
  
    ```  
    dta -S YourServerName\YourSQLServerInstanceName -E -D AdventureWorks2012 -if MyScript.sql -s MySession2 -of MySession2OutputScript.sql -ox MySession2Output.xml -fa IDX_IV -fp NONE -fk NONE  
    ```  
  
    其中 `-S` 指定安装了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的服务器和 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 实例的名称。 `-E` 设置指定要使用可信连接来连接实例。使用 Windows 域帐户连接时可使用该设置。 `-D` 设置指定要优化的数据库， `-if` 指定工作负荷文件， `-s` 指定会话名称， `-of` 指定该工具要将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 建议脚本写入其中的文件， `-ox` 指定该工具要将 XML 格式的建议脚本写入其中的文件。 最后三个开关指定如下优化选项： `-fa IDX_IV` 指定数据库引擎优化顾问应该只考虑添加索引（包括聚集和非聚集索引）和索引视图； `-fp NONE` 指定分析时不考虑分区策略； `-fk NONE` 指定数据库引擎优化顾问进行建议时不必保留数据库中的现有物理设计结构。  

  ![结合使用 CMD 和 DTA](media/dta-tutorials/dta-cmd.png)
  
3.  数据库引擎优化顾问完成了优化工作负荷后，将显示一个消息指示优化会话已成功完成。 若要查看优化结果，可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 打开 MySession2OutputScript.sql 和 MySession2Output.xml 文件。 此外，也可以在数据库引擎优化顾问 GUI 中打开 MySession2 优化会话并查看其建议和报告，执行的方式与 [查看优化建议](./lesson-2-using-database-engine-tuning-advisor.md?view=sql-server-ver15) 和 [查看优化报告](./lesson-2-using-database-engine-tuning-advisor.md?view=sql-server-ver15)中执行的方式相同。  
  
 
## <a name="after-you-finish-this-tutorial"></a>学完本教程后  
学完本教程中的课程后，请参考以下主题来了解有关数据库引擎优化顾问的详细信息：  
  
-   [数据库引擎优化顾问](../../relational-databases/performance/database-engine-tuning-advisor.md) 提供有关如何使用此工具执行任务的说明。 
-   [dta 实用工具](../../tools/dta/dta-utility.md) 提供有关此命令提示实用工具的参考材料和可用于控制此实用工具的操作的可选 XML 文件。  
  
若要返回到教程的起始位置，请参阅[教程：数据库引擎优化顾问](../../tools/dta/tutorial-database-engine-tuning-advisor.md)。  
  
## <a name="see-also"></a>另请参阅  
[数据库引擎教程](../../relational-databases/database-engine-tutorials.md)  
