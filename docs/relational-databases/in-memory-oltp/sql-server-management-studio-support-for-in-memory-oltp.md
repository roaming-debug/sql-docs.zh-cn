---
title: SSMS 对内存中 OLTP 的支持
description: 使用 SQL Server Management Studio 来管理内存优化表、表索引、本机编译的存储过程和用户定义的表类型。
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ee847b5f-6a1a-448e-a746-d61a023881ff
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8684764c0386513c0890b10dee8bfa3582c1cf59
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485169"
---
# <a name="sql-server-management-studio-support-for-in-memory-oltp"></a>对内存中 OLTP 的 SQL Server Management Studio 支持
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 是用于管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 基础结构的集成环境。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供用于配置、监视和管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的工具。 有关详细信息，请参阅 [SQL Server Management Studio](../../ssms/sql-server-management-studio-ssms.md)  
  
 本主题中的任务说明如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 来管理内存优化的表、内存优化的表的索引、本机编译的存储过程以及用户定义的内存优化的表类型。  
  
 有关如何以编程方式创建内存优化表的信息，请参阅 [创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。  
  
### <a name="to-create-a-database-with-a-memory-optimized-data-filegroup"></a>使用内存优化的数据文件组创建数据库  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库引擎实例，然后展开该实例。  
  
2.  右键单击“数据库”，然后单击“新建数据库”。  
  
3.  若要添加新内存优化的数据文件组，请单击“文件组”页。 在“内存优化数据”下，单击“添加文件组”，然后输入内存优化数据文件组的名称。  标有 **“FILESTREAM 文件”** 的列表示文件组中的容器数。 容器是在 **“常规”** 页上添加的。  
  
4.  要向文件组添加文件（容器），请单击“常规”页。 在 **“数据库文件”** 下，单击 **“添加”** 。 在“文件类型”中选择“FILESTREAM 数据”，指定容器的逻辑名称，选择内存优化文件组，并确保“自动增长/最大大小”设置为“无限制”。  

     有关如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 创建新数据库的更多信息，请参阅[创建数据库](../../relational-databases/databases/create-a-database.md)。  
  
### <a name="to-create-a-memory-optimized-table"></a>创建内存优化的表  
  
1.  在“对象资源管理器”中，右键单击数据库的 **Tables**“表”节点，单击“新建”，然后单击“内存优化的表”。  
  
     此时会显示用于创建内存优化表的模板。  
  
2.  若要替换模板参数，请单击 **“指定模板参数的值”** （在 **“查询”** 菜单上）。  
  
     有关如何使用模板的更多信息，请参阅 [Template Explorer](../../ssms/template/template-explorer.md)。  
  
3.  在“对象资源管理器”中，表先按基于磁盘的表，然后按内存优化表的顺序排序。 使用 **“对象资源管理器详细信息”** 可以查看按名称排序的所有表。  
  
### <a name="to-create-a-natively-compiled-stored-procedure"></a>创建本机编译的存储过程  
  
1.  在“对象资源管理器”中，右键单击数据库的“存储过程”节点，单击“新建”，然后单击“本机编译的存储过程”。  
  
     此时会显示用于创建本机编译存储过程的模板。  
  
2.  若要替换模板参数，请单击 **“指定模板参数的值”** （在 **“查询”** 菜单上）。  
  
     有关如何创建新存储过程的更多信息，请参阅 [Create a Stored Procedure](../../relational-databases/stored-procedures/create-a-stored-procedure.md)。  
  
### <a name="to-create-a-user-defined-memory-optimized-table-type"></a>创建用户定义的内存优化表类型  
  
1.  在“对象资源管理器”中，展开数据库的“类型”节点，右键单击“用户定义的表类型”节点，单击“新建”，然后单击“用户定义的内存优化表类型”。  
  
     将显示创建用户定义的内存优化表类型所用的模板。  
  
2.  若要替换模板参数，请单击 **“指定模板参数的值”** （在 **“查询”** 菜单上）。  
  
     有关如何创建新存储过程的更多信息，请参阅 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)。  
  
## <a name="memory-monitoring"></a>内存监视  
  
#### <a name="view-memory-usage-by-memory-optimized-objects-report"></a>查看内存优化对象的内存使用情况报表  
  
-   在“对象资源管理器”中，右键单击数据库，然后依次单击“报表”、“标准报表”、“内存优化对象的内存使用情况”。  
  
     该报表提供数据库中的内存优化对象占用的内存空间详细数据。  
  
#### <a name="view-properties-for-allocated-and-used-memory-for-a-table-database"></a>查看为表、数据库分配和使用的内存的属性  
  
1.  获取有关内存使用情况的信息：  
  
    -   在“对象资源管理器”中，右键单击内存优化表，然后依次单击“属性”、“存储”页。 **“数据空间”** 属性的值指示表中数据占用的内存。 **“索引空间”** 属性的值指示表中索引占用的内存。  
  
    -   在“对象资源管理器”中，右键单击数据库，单击“属性”，然后单击“常规”页。 “分配给内存优化对象的内存”属性的值指示分配给数据库中内存优化对象的内存。 “内存优化对象使用的内存”属性的值指示数据库中内存优化对象使用的内存。  
  
## <a name="supported-features-in-ssmanstudiofull"></a>中支持的功能 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 支持具有内存优化的数据文件组、内存优化的表、索引和本机编译的存储过程的数据库上数据库引擎所支持的功能和操作。  
  
 对于数据库、表、存储过程、用户定义的表类型或索引对象，以下 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 功能已得到更新或扩展，以便支持内存中 OLTP。  
  
-   “对象资源管理器”  
  
    -   上下文菜单  
  
    -   筛选器设置  
  
    -   编写脚本为  
  
    -   任务  
  
    -   报表  
  
    -   属性  
  
    -   数据库任务：  
  
        -   附加和分离包含内存优化表的数据库。  
  
             “附加数据库”用户界面不显示内存优化数据文件组。 但是，您可以继续附加数据库并且数据库将正确附加。  
  
            > [!NOTE]  
            >  如果要使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 附加具有内存优化数据文件组容器的数据库，并且该数据库的内存优化数据文件组容器是在另一台计算机上创建的，则该内存优化数据文件组容器在两台计算机上的位置必须相同。 如果希望该数据库的内存优化数据文件组容器在新计算机上的位置不同，则可使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 附加该数据库。 在下例中，内存优化数据文件组容器在新计算机上的位置为 C:\Folder2。 但是，在第一台计算机上创建内存优化数据文件组容器时，该位置为 C:\Folder1。  
            >   
            >  `CREATE DATABASE[imoltp] ON`  
            >   
            >  `(NAME =N'imoltp',FILENAME=N'C:\Folder2\imoltp.mdf'),`  
            >   
            >  `(NAME =N'imoltp_mod1',FILENAME=N'C:\Folder2\imoltp_mod1'),`  
            >   
            >  `(NAME =N'imoltp_log',FILENAME=N'C:\Folder2\imoltp_log.ldf')`  
            >   
            >  `FOR ATTACH`  
            >   
            >  `GO`  
  
        -   生成脚本。  
  
             在 **“生成和发布脚本向导”** 中， **“检查对象是否存在”** 脚本编写选项的默认值是 FALSE。 如果“检查对象是否存在”脚本编写选项的值在向导的“设置脚本编写选项”屏幕中设置为 TRUE，则生成的脚本将包含“CREATE PROCEDURE <过程名称> AS”和“ALTER PROCEDURE <过程名称> <过程定义>”。 在执行时，生成的脚本将返回错误，因为在本机编译的存储过程中不支持 ALTER PROCEDURE。  
  
             更改每个本机编译的存储过程的生成的脚本：  
  
            1.  在“CREATE PROCEDURE <过程名称> AS”中，使用“<过程定义>”替换“AS”。  
  
            2.  删除“ALTER PROCEDURE <过程名称> <过程定义>”。  
  
        -   复制数据库。 对于具有内存优化的对象的数据库，将不在事务内执行在目标服务器上创建数据库以及传输数据。  
  
        -   导入和导出数据。 使用“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 从一个或多个表或视图中导入和导出向导复制数据”选项。 如果目标表是目标数据库中不存在的内存优化表：  
  
            1.  在 **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入与导出向导”** 中的 **“指定表复制或查询”** 屏幕上，选择 **“复制一个或多个表或视图的数据”** 。 然后单击“下一步”。  
  
            2.  单击 **“编辑映射”** 。 然后选择 **“创建目标表”** 并单击 **“编辑 SQL”** 。 在目标数据库上输入用于创建内存优化表的 CREATE TABLE 语法。 单击 **“确定”** 并完成向导中的剩余步骤。  
  
        -   维护计划。 不支持对内存优化的表及其索引执行重新组织索引和重建索引的维护任务。 因此，在执行重建索引和重新组织索引的维护计划时，将忽略选定数据库中的内存优化表及其索引。  
  
             对内存优化的表及其索引的抽样扫描不支持维护任务更新统计信息。 因此，在执行更新统计信息的维护计划时，始终将内存优化的表及其索引的统计信息更新到 **WITH FULLSCAN, NORECOMPUTE**。  
  
-   “对象资源管理器详细信息”窗格  
  
-   模板资源管理器  
  
## <a name="unsupported-features-in-ssmanstudiofull"></a>中不支持的功能 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
 对于内存中 OLTP 对象，数据库引擎不支持的功能和操作， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 同样不支持。  
  
 有关不支持的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能的详细信息，请参阅 [内存中 OLTP 不支持的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 对内存中 OLTP 的支持](./transact-sql-support-for-in-memory-oltp.md)  
  
