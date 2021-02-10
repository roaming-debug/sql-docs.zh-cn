---
description: '转换 DB2 架构 (DB2ToSQL) '
title: 转换 DB2 架构 (DB2ToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7947efc3-ca86-4ec5-87ce-7603059c75a0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: af91274d4e48ce99894316c730a58d68fd633242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062862"
---
# <a name="converting-db2-schemas-db2tosql"></a>转换 DB2 架构 (DB2ToSQL) 
连接到 DB2、连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并设置项目和数据映射选项后，可以将 DB2 数据库对象转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库对象。  
  
## <a name="the-conversion-process"></a>转换过程  
转换数据库对象将从 DB2 获取对象定义，并将其转换为类似 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象，然后将此信息加载到 SSMA 元数据。 它不会将信息加载到的实例中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 然后，您可以使用 " [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 元数据资源管理器" 查看对象及其属性。  
  
在转换过程中，SSMA 会将输出消息打印到 "输出" 窗格，并将错误消息打印到 "错误列表" 窗格。 使用输出和错误信息来确定是否必须修改 DB2 数据库或转换过程以获取所需的转换结果。  
  
## <a name="setting-conversion-options"></a>设置转换选项  
转换对象之前，请在 " **项目设置** " 对话框中查看项目转换选项。 使用此对话框，可以设置 SSMA 如何转换函数和全局变量。 有关详细信息，请参阅 [DB2ToSQL&#41;&#40;转换&#41; &#40;项目设置 ](../../ssma/db2/project-settings-conversion-db2tosql.md)。  
  
## <a name="conversion-results"></a>转换结果  
下表显示转换了哪些 DB2 对象以及生成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象：  
  
|DB2 对象|生成的 SQL Server 对象|  
|-----------|----------------------------|  
|数据类型|**SSMA 映射除以下列出的类型之外的每种类型：**<br /><br />CLOB：不支持使用此类型工作的某些本机函数 (例如 CLOB_EMPTY ( # A2 # A3<br /><br />BLOB：不支持使用此类型工作的某些本机函数 (例如 BLOB_EMPTY ( # A2 # A3<br /><br />DBLOB：不支持使用此类型工作的某些本机函数 (例如 DBLOB_EMPTY ( # A2 # A3|  
|用户定义类型|**SSMA 映射以下用户定义的：**<br /><br />Distinct 类型<br /><br />结构化类型<br /><br />SQL PL 数据类型-注意：不支持弱游标类型。|  
|特殊寄存器|**SSMA 仅映射下面列出的注册：**<br /><br />当前时间戳<br /><br />当前日期<br /><br />当前时间<br /><br />当前时区<br /><br />当前用户<br /><br />SESSION_USER 和用户<br /><br />SYSTEM_USER<br /><br />当前 CLIENT_APPLNAME<br /><br />当前 CLIENT_WRKSTNNAME<br /><br />当前锁超时<br /><br />当前架构<br /><br />当前服务器<br /><br />当前隔离<br /><br />其他特殊寄存器不会映射到 SQL server 语义。|  
|CREATE TABLE|**SSMA 映射 CREATE TABLE 具有以下例外：**<br /><br />多维群集 (MDC) 表<br /><br />范围-聚集表 (.RCT) <br /><br />分区表<br /><br />分离的表<br /><br />数据捕获子句<br /><br />隐式隐藏选项<br /><br />VOLATILE 选项|  
|CREATE VIEW|SSMA maps CREATE VIEW with "WITH LOCAL CHECK OPTION" 但其他选项未映射到 SQL server 语义|  
|CREATE INDEX|**SSMA 映射创建索引时出现以下异常：**<br /><br />XML 索引<br /><br />无重叠选项 BUSINESS_TIME<br /><br />分区子句<br /><br />仅规范选项<br /><br />使用扩展选项<br /><br />MINPCTUSED 选项<br /><br />PAGE SPLIT 选项|  
|触发器|**SSMA 映射以下触发器语义：**<br /><br />每行触发器之后/<br /><br />在/FOR 每个语句后触发<br /><br />在每行之前/之前，而不是每行触发器|  
|序列|已映射。|  
|SELECT 语句|**SSMA 映射选择但具有以下例外：**<br /><br />数据更改表引用子句-部分映射，但不支持最终表<br /><br />表引用子句-部分映射但仅限表引用、外部表引用 analyze_table 表达式、集合派生表、xmltable 表达式未映射到 SQL server 语义<br /><br />Period-规范子句-未映射。<br /><br />Continue-处理程序子句-未映射。<br /><br />未映射类型化的相关子句。<br /><br />并发访问解析子句-未映射。|  
|VALUES 语句|已映射。|  
|INSERT 语句|已映射。|  
|UPDATE 语句|S **SMA MAPS 更新，但存在以下例外：**<br /><br />表引用子句的唯一表引用未映射到 SQL server 语义<br /><br />Period 子句-未映射。|  
|MERGE 语句|**SSMA 映射与以下例外合并：**<br /><br />每个子句的单个和多个匹配项-映射为每个子句的有限出现次数的 SQL server 语义<br /><br />信子句-不映射到 SQL Server 语义<br /><br />混合更新和删除子句-不映射到 SQL Server 语义<br /><br />Period-子句-不映射到 SQL Server 语义|  
|DELETE 语句|**SSMA 映射删除但出现以下异常：**<br /><br />表引用子句的唯一表引用未映射到 SQL server 语义<br /><br />Period 子句-不映射到 SQL Server 语义|  
|隔离级别和锁定类型|已映射。|  
| (SQL) 的过程|已映射。|  
|过程 (外部) |需要手动更新。|  
| (的过程) |不要映射到 SQL Server 语义。|  
|赋值语句|已映射。|  
|过程的 CALL 语句|已映射。|  
|CASE 语句|已映射。|  
|FOR 语句|已映射。|  
|GOTO 语句|已映射。|  
|IF 语句|已映射。|  
|循环访问语句|已映射。|  
|LEAVE 语句|已映射。|  
|LOOP 语句|已映射。|  
|REPEAT 语句|已映射。|  
|RESIGNAL 语句|不支持条件。 消息可以是可选的。|  
|RETURN 语句|已映射。|  
|信号语句|不支持条件。 消息可以是可选的。|  
|WHILE 语句|已映射。|  
|获取诊断语句|**SSMA maps 获取诊断，但有以下例外：**<br /><br />ROW_COUNT-已映射。<br /><br />DB2_RETURN_STATUS-已映射。<br /><br />MESSAGE_TEXT-已映射。<br /><br />DB2_SQL_NESTING_LEVEL-不映射到 SQL Server 语义<br /><br />DB2_TOKEN_STRING-不映射到 SQL Server 语义|  
|游标|**SSMA 映射游标，但有以下例外：**<br /><br />ALLOCATE CURSOR 语句-不映射到 SQL Server 语义<br /><br />关联定位器语句-不映射到 SQL Server 语义<br /><br />DECLARE CURSOR Returnability 子句未映射到 SQL server 语义<br /><br />FETCH 语句-部分映射。 仅支持作为目标的变量。 SQLDA 描述符未映射到 SQL server 语义|  
|变量|已映射。|  
|异常、处理程序和条件|**SSMA 映射 "异常处理"，但以下情况例外：**<br /><br />退出处理程序-已映射。<br /><br />撤消处理程序-已映射。<br /><br />CONTINUE 处理程序-不映射。<br /><br />条件-它不映射到 SQL server 语义。|  
|动态 SQL|未映射。|  
|别名|已映射。|  
|昵称|部分映射。 基础对象需要手动处理|  
|同义词|已映射。|  
|DB2 中的标准函数|SQL Server 中的等效函数可用时，SSMA 映射 DB2 标准函数：|  
|授权|未映射。|  
|谓词|已映射。|  
|SELECT INTO 语句|未映射。|  
|值 INTO 语句|未映射。|  
|事务控制|未映射。|  
  
## <a name="converting-db2-database-objects"></a>转换 DB2 数据库对象  
若要转换 DB2 数据库对象，请首先选择要转换的对象，然后让 SSMA 执行转换。 若要在转换过程中查看输出消息，请在 " **视图** " 菜单上选择 " **输出**"。  
  
**将 DB2 对象转换为 SQL Server 语法**  
  
1.  在 DB2 元数据资源管理器中，展开 DB2 服务器，然后展开 " **架构**"。  
  
2.  选择要转换的对象：  
  
    -   若要转换所有架构，请选中 " **架构**" 旁边的复选框。  
  
    -   若要转换或省略数据库，请选中该架构名称旁边的复选框。  
  
    -   若要转换或省略对象的类别，请展开一个架构，然后选中或清除该类别旁边的复选框。  
  
    -   若要转换或省略单个对象，请展开 category 文件夹，然后选中或清除该对象旁边的复选框。  
  
3.  若要转换所有选定的对象，请右键单击 " **架构** "，然后选择 " **转换架构**"。  
  
    您还可以通过右键单击对象或其父文件夹，然后选择 " **转换架构**"，来转换各个对象或对象类别。  
  
## <a name="viewing-conversion-problems"></a>查看转换问题  
某些 DB2 对象可能不会转换。 您可以通过查看摘要转换报告来确定转换成功率。  
  
**查看摘要报表**  
  
1.  在 DB2 元数据资源管理器中，选择 " **架构**"。  
  
2.  在右侧窗格中，选择 " **报表** " 选项卡。  
  
    此报表显示已评估或转换的所有数据库对象的摘要评估报告。 您还可以查看单个对象的摘要报表：  
  
    -   若要查看单个架构的报表，请在 DB2 元数据资源管理器中选择该架构。  
  
    -   若要查看单个对象的报表，请在 DB2 元数据资源管理器中选择该对象。 具有转换问题的对象具有红色错误图标。  
  
对于失败转换的对象，可以查看导致转换失败的语法。  
  
**查看各个转换问题**  
  
1.  在 DB2 元数据资源管理器中，展开 " **架构**"。  
  
2.  展开显示红色错误图标的架构。  
  
3.  在该架构下，展开一个包含红色错误图标的文件夹。  
  
4.  选择包含红色错误图标的对象。  
  
5.  在右侧窗格中，单击 " **报表** " 选项卡。  
  
6.  在 " **报表** " 选项卡的顶部是一个下拉列表。 如果列表显示 " **统计信息**"，请将所选内容更改为 " **源**"。  
  
    SSMA 将显示源代码，并将多个按钮直接显示在代码上方。  
  
7.  单击 " **下一问题** " 按钮。 这是一个红色的错误图标，其中有一个指向右侧的箭头。  
  
    SSMA 将突出显示在当前对象中找到的第一个有问题的源代码。  
  
对于无法转换的每个项，必须确定要对该对象执行哪些操作：  
  
-   您可以在 " **SQL** " 选项卡上修改过程的源代码。  
  
-   您可以修改 DB2 数据库中的对象，以删除或修改有问题的代码。 若要将更新的代码加载到 SSMA 中，必须更新元数据。 有关详细信息，请参阅 [连接到 DB2 数据库 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)。  
  
-   可以从迁移中排除对象。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] "元数据资源管理器" 和 "DB2 元数据资源管理器" 中，清除项旁的复选框，然后将对象加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] db2 并从 DB2 迁移数据。  
  
## <a name="next-step"></a>下一步  
迁移过程的下一步是将已 [转换的对象加载到 SQL Server 中](./loading-converted-database-objects-into-sql-server-db2tosql.md)。  
  
## <a name="see-also"></a>另请参阅  
[将 DB2 数据迁移到 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
