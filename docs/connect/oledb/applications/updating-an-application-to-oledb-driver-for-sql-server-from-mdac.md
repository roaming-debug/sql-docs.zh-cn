---
title: 将应用程序从 MDAC 更新到适用于 SQL Server 的 OLE DB 驱动程序
description: 了解旧的 OLE DB Provider for SQL Server 与新的 OLE DB Driver for SQL Server 之间的变化，以及更新到新驱动器的须知事项。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, vs. MDAC
- OLE DB Driver for SQL Server, vs. MDAC
- data access [OLE DB Driver for SQL Server], vs. MDAC
- OLE DB Driver for SQL Server, updating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a6acf12dd61b4d772139811b9ead823ded0b635c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751207"
---
# <a name="updating-an-application-to-ole-db-driver-for-sql-server-from-mdac"></a>将应用程序从 MDAC 更新到适用于 SQL Server 的 OLE DB 驱动程序
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序和 Microsoft 数据访问组件 (MDAC) 之间存在很多不同；从 Windows Vista 开始，数据访问组件改名为 Windows 数据访问组件（或 Windows DAC）。 虽然两者都可提供对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的本机数据访问权限，但适用于 SQL Server 的 OLE DB 驱动程序经过专门地设计以公开 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的新功能，且同时保持了与早期版本的后向兼容性。   

 此外，MDAC 包含使用 OLE DB、ODBC 和 ActiveX 数据对象 (ADO) 的组件，而 OLE DB Driver for SQL Server 仅能实现 OLE DB（尽管 ADO 可以访问 OLE DB Driver for SQL Server 的功能）。  

 OLE DB Driver for SQL Server 和 MDAC 在以下其他方面还存在区别：  

-   与访问 SQL OLE DB 提供程序相比，使用 ADO 访问 OLE DB Driver for SQL Server 的用户可能会发现其筛选功能较少。  

-   如果 ADO 应用程序使用 OLE DB Driver for SQL Server 并尝试更新计算列，将会报告错误。 使用 MDAC，会接受但忽略此更新。  

-   OLE DB Driver for SQL Server 是一个独立的动态链接库 (DLL) 文件。 公开的接口已经保持为最低限度，这样既是为了减轻分发工作，也是为了降低安全风险。  

-   仅支持 OLE DB 接口。  

-   OLE DB Driver for SQL Server 名称与用于 MDAC 的名称不同。  

-   使用 OLE DB Driver for SQL Server 时，可以使用由 MDAC 组件提供的用户可访问功能。 这包括但不限于以下功能：连接池、ADO 支持以及客户端游标支持。 使用这些功能中的任意功能时，OLE DB Driver for SQL Server 仅提供数据库连接。 MDAC 可提供诸如跟踪、管理控件和性能计数器等功能。  

-   应用程序可将 OLE DB 核心服务与适用于 SQL Server 的 OLE DB 驱动程序配合使用，但如果使用 OLE DB 游标引擎，则应使用数据类型兼容性选项，从而避免可能由于游标引擎不能识别新的 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 数据类型而引起的任何潜在问题。  

-   OLE DB Driver for SQL Server 支持访问以前的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库。  

-   OLE DB Driver for SQL Server 不包含 XML 集成。 OLE DB Driver for SQL Server 支持 SELECT ...FOR XML 查询，但不支持任何其他 XML 功能。 但是，OLE DB Driver for SQL Server 支持 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的 xml  数据类型。  

-   适用于 SQL Server 的 OLE DB 驱动程序支持仅使用连接字符串属性来配置客户端网络库。 如果需要更完整的网络库配置，您必须使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器。  

-   MDAC 连接字符串允许将布尔值 (true  ) 用于 Trusted_Connection  关键字。 OLE DB Driver for SQL Server 连接字符串必须使用 yes  或 no  。  

-   警告和错误已发生小的改动。 现在，服务器返回的警告和错误在传递给适用于 SQL Server 的 OLE DB 驱动程序时会保留相同的严重性。 如果要依赖于捕获特定的警告和错误，则应当确保已经全面测试了应用程序。  

-   适用于 SQL Server 的 OLE DB 驱动程序的错误检查比 MDAC 更为严格，也就是说，某些不严格遵从 OLE DB 规范的应用程序可能会有不同的表现。 例如，SQLOLEDB 访问接口不强制执行结果参数的参数名称必须以“\@”开头这一规则，而适用于 SQL Server 的 OLE DB 驱动程序会强制执行此规则。  

-   对于失败的连接，OLE DB Driver for SQL Server 的行为不同于 MDAC。 例如连接失败的情况，MDAC 会返回缓存的属性值，而适用于 SQL Server 的 OLE DB 驱动程序会向调用应用程序报告错误。  

-   适用于 SQL Server 的 OLE DB 驱动程序不会生成 Visual Studio Analyzer 事件，而是生成 Windows 跟踪事件。  

-   OLE DB Driver for SQL Server 不能与 Perfmon 一起使用。 Perfmon 是一种 Windows 工具，它仅可与使用 Windows 附带的 MDAC SQLODBC 驱动程序的 DSN 一起使用。  

-   当 OLE DB Driver for SQL Server 连接到 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 和更高版本时，服务器错误 16947 会作为 SQL_ERROR 返回。 定位更新无法更新行或定位删除无法删除行时，会出现此错误。 当 MDAC 连接到任何版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，服务器错误 16947 会作为警告 (SQL_SUCCESS_WITH_INFO) 返回。  

-   适用于 SQL Server 的 OLE DB 驱动程序实现 IDBDataSourceAdmin 接口（是一个以前未实现的可选 OLE DB 接口），但只实现了此可选接口的 CreateDataSource 方法   。 [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  

-   在 TABLE_TYPE 设置为 SYNONYM 的情况下，适用于 SQL Server 的 OLE DB 驱动程序返回 TABLES 和 TABLE_INFO 架构行集中的同义词。  

-   数据类型 varchar(max)、nvarchar(max)、varbinary(max)、xml、udt 或其他大型对象类型的返回值不能返回到早于 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 的客户端版本      。 如果需要将这些类型用作返回值，必须使用 OLE DB Driver for SQL Server。  

-   MDAC 允许在手动事务和隐式事务启动时执行以下语句，但适用于 SQL Server 的 OLE DB 驱动程序不允许。 这些语句必须以自动提交模式执行。  

    -   所有全文操作（索引和目录 DDL）  

    -   所有数据库操作（创建数据库、更改数据库以及删除数据库）  

    -   重新配置  

    -   关机  

    -   终止  

    -   备份  

-   MDAC 应用程序连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时，[!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的数据类型将显示为与 [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] 兼容的数据类型，如下表所示。  

    |SQL Server 2005 类型|SQL Server 2000 类型|  
    |--------------------------|--------------------------|  
    |**varchar(max)**|**text**|  
    |**nvarchar(max)**|**ntext**|  
    |**varbinary(max)**|**图像**|  
    |**udt**|**varbinary**|  
    |**xml**|**ntext**|  

     此类型映射会影响为列元数据返回的值。 例如，text  列的最大大小为 2,147,483,647，但 OLE DB Driver for SQL Server 将 varchar(max)  列的最大大小报告为 2,147,483,647 或 -1，具体根据平台而定。  

-   考虑到后向兼容性，适用于 SQL Server 的 OLE DB 驱动程序允许连接字符串中存在多义性（例如，可能多次指定某些关键字，并且可能允许基于位置或优先级对发生冲突的关键字进行解析）。 OLE DB Driver for SQL Server 的未来版本可能不允许连接字符串中存在多义性。 在修改要使用适用于 SQL Server 的 OLE DB 驱动程序的应用程序时，最好消除连接字符串多义性上的任何依赖项。  

-   如果使用 OLE DB 调用来启动事务，则 OLE DB Driver for SQL Server 和 MDAC 的行为会有所不同；使用 OLE DB Driver for SQL Server 时将立即启动事务，但使用 MDAC 时，事务将在第一次数据库访问后开始启动。 这可能会影响存储过程和批处理的行为，因为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 要求不管是在批处理或存储过程执行结束后还是在该批处理或存储过程启动时 @@TRANCOUNT 均应相同。  

-   使用 OLE DB Driver for SQL Server 时，ITransactionLocal::BeginTransaction 将导致事务立即启动。 如果使用 MDAC，则事务会延迟，直到应用程序已执行要求事务处于隐式事务模式的语句后才启动。 有关详细信息，请参阅 [SET IMPLICIT_TRANSACTIONS (Transact-SQL)](../../../t-sql/statements/set-implicit-transactions-transact-sql.md)。  


 OLE DB Driver for SQL Server 和 MDAC 都支持使用行版本控制的已提交读事务隔离，但只有 OLE DB Driver for SQL Server 支持快照事务隔离。 （就编程而言，使用行版本控制的已提交读事务隔离等同于已提交读事务。）  

## <a name="see-also"></a>另请参阅  
 [使用适用于 SQL Server 的 OLE DB 驱动程序生成应用程序](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
