---
description: sp_addlinkedserver (Transact-SQL)
title: sp_addlinkedserver (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedserver_TSQL
- sp_addlinkedserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedserver
ms.assetid: fed3adb0-4c15-4a1a-8acd-1b184aff558f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c894bbf5bdb3e523901fc4f1200d146d0a569449
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171790"
---
# <a name="sp_addlinkedserver-transact-sql"></a>sp_addlinkedserver (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建链接服务器。 链接服务器让用户可以对 OLE DB 数据源进行分布式异类查询。 使用 **sp_addlinkedserver** 创建链接服务器后，可对该服务器运行分布式查询。 如果链接服务器定义为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例，则可执行远程存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_addlinkedserver [ @server= ] 'server' [ , [ @srvproduct= ] 'product_name' ]   
     [ , [ @provider= ] 'provider_name' ]  
     [ , [ @datasrc= ] 'data_source' ]   
     [ , [ @location= ] 'location' ]   
     [ , [ @provstr= ] 'provider_string' ]   
     [ , [ @catalog= ] 'catalog' ]   
```  
  
## <a name="arguments"></a>参数  
[ @server =] *\' 服务器 \'*          
要创建的链接服务器的名称。 *server* 的数据类型为 **sysname**，无默认值。  
  
[ @srvproduct =] *\' product_name \'*          
要添加为链接服务器的 OLE DB 数据源的产品名称。 *product_name* 为 **nvarchar (** 128 **)**，默认值为 NULL。 如果不需要指定 **SQL Server**、 *provider_name*、 *data_source*、 *位置*、 *provider_string* 和 *目录* 。  
  
[ @provider =] *\' provider_name \'*          
与此数据源对应的 OLE DB 访问接口的唯一编程标识符 (PROGID)。 对于当前计算机上安装的指定 OLE DB 提供程序， *provider_name* 必须是唯一的。 *provider_name* 为 **nvarchar (128)**，默认值为 NULL;但是，如果省略 *provider_name* ，则使用 sqlncli.msi。 

> [!NOTE]
> 使用 SQLNCLI.MSI 将重定向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序的最新版本。 OLE DB 提供程序应以指定的 PROGID 在注册表中注册。

> [!IMPORTANT] 
> 以前的 Microsoft OLE DB Provider for SQL Server (SQLOLEDB) 和 SQL Server Native Client OLEDB 提供程序 (SQLNCLI) 仍然不推荐使用，不建议在新的开发工作中使用它们。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。
  
[ @datasrc =] *\' data_source \'*          
 由 OLE DB 访问接口解释的数据源的名称。 *data_source* (4000 **)** 为 **nvarchar**。 *data_source* 作为 DBPROP_INIT_DATASOURCE 属性传递以初始化 OLE DB 提供程序。  
  
[ @location =] *\' 位置 \'*          
 由 OLE DB 访问接口解释的数据库的位置。 *location* (4000 **)** 为 **NVARCHAR**，默认值为 NULL。 *location* 作为 DBPROP_INIT_LOCATION 属性传递以初始化 OLE DB 提供程序。  
  
[ @provstr =] *\' provider_string \'*          
 OLE DB 访问接口特定的连接字符串，它可标识唯一的数据源。 *provider_string* 为 **nvarchar (** 4000 **)**，默认值为 NULL。 *provstr* 传递给 IDataInitialize，或设置为 DBPROP_INIT_PROVIDERSTRING 属性以初始化 OLE DB 提供程序。  
  
 当针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序创建链接服务器时，可以通过使用 server 关键字 as server =*servername* \\ *instancename* 指定的特定实例来指定实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *servername* 是运行的计算机的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， *instancename* 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户将连接到的特定实例的名称。  
  
> [!NOTE]
> 若要访问镜像数据库，则连接字符串必须包含数据库名称。 该名称是数据访问接口启用故障转移尝试所必需的。 可以在 **\@ provstr** 或 **\@ catalog** 参数中指定数据库。 此外，连接字符串还可以提供故障转移伙伴名称。  
  
[ @catalog =] *\' 目录 \'*       
 与 OLE DB 访问接口建立连接时所使用的目录。 *目录* 的值为 **sysname**，默认值为 NULL。 *目录* 作为 DBPROP_INIT_CATALOG 属性传递以初始化 OLE DB 提供程序。 在针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例定义链接服务器时，目录指向链接服务器映射到的默认数据库。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 下表显示为能通过 OLE DB 访问数据源而建立链接服务器的方法。 对于特定的数据源，可以使用多种方法为其设置链接服务器；该表中可能有多行适用于一种数据源类型。 此表还显示了用于设置链接服务器的 **sp_addlinkedserver** 参数值。  
  
|远程 OLE DB 数据源|OLE DB 访问接口|product_name|provider_name|data_source|location|provider_string|目录|  
|-------------------------------|---------------------|-------------------|--------------------|------------------|--------------|----------------------|-------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] <sup>1</sup> （默认值）||||||  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序||**SQLNCLI**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的网络名称（用于默认实例）|||数据库名称（可选）|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序||**SQLNCLI**|*servername* \\特定实例的 *instancename* () |||数据库名称（可选）|  
|Oracle，版本 8 及更高版本|Oracle Provider for OLE DB|Any|**OraOLEDB.Oracle**|用于 Oracle 数据库的别名||||  
|Access/Jet|Microsoft OLE DB Provider for Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Jet 数据库文件的完整路径||||  
|ODBC 数据源|Microsoft OLE DB Provider for ODBC|Any|**MSDASQL**|ODBC 数据源的系统 DSN||||  
|ODBC 数据源|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for ODBC|Any|**MSDASQL**|||ODBC 连接字符串||  
|文件系统|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Indexing Service|Any|**MSIDXS**|索引服务目录名称||||  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 电子表格|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for Jet|Any|**Microsoft.Jet.OLEDB.4.0**|Excel 文件的完整路径||Excel 5。0||  
|IBM DB2 数据库|[!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB Provider for DB2|Any|**DB2OLEDB**|||请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] DB2 文档 OLE DB 提供程序。|DB2 数据库的目录名称|  
  
 <sup>1</sup> 通过这种设置链接服务器的方式，可以强制链接服务器的名称与的远程实例的网络名称相同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用 *data_source* 指定服务器。  
  
 <sup>2</sup> "任何" 表示产品名称可以是任何内容。  
  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果未指定提供程序名称，或者 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指定为产品名称，则 Native Client OLE DB 提供程序是与一起使用的提供程序。 即使指定了较早版本的访问接口名称 SQLOLEDB，在保存到目录时该名称也将改为 SQLNCLI。  
  
 *Data_source*、*位置*、 *provider_string* 和 *目录* 参数标识链接服务器指向的数据库或数据库。 如果其中任一参数为 NULL，则不设置相应的 OLE DB 初始化属性。  
  
 在群集环境中，当指定指向 OLE DB 数据源的文件名时，应使用通用命名规则 (UNC) 名称或共享驱动器指定位置。  
  
 不能在用户定义的事务中执行 **sp_addlinkedserver** 。  
  
> [!IMPORTANT]
> 使用 **sp_addlinkedserver** 创建链接服务器时，将为所有本地登录名添加默认自映射。 对于非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供程序， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 经过身份验证的登录名可能能够获取服务帐户下的访问接口的访问权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 管理员应考虑使用 `sp_droplinkedsrvlogin <linkedserver_name>, NULL` 删除全局映射。  
  
## <a name="permissions"></a>权限  
 `sp_addlinkedserver`语句需要 `ALTER ANY LINKED SERVER` 权限。  (" [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **新建链接服务器** " 对话框的实现方式是需要 `sysadmin` 固定服务器角色的成员身份。 )   
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-microsoft-sql-server-ole-db-provider"></a>A. 使用 Microsoft SQL Server OLE DB 提供程序  
 下面的示例将创建一个名为 `SEATTLESales` 的链接服务器。 产品名称为 `SQL Server`，未使用访问接口名称。  
  
```sql  
USE master;  
GO  
EXEC sp_addlinkedserver   
   N'SEATTLESales',  
   N'SQL Server';  
GO  
```  

 下面的示例 `S1_instance1` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE DB 驱动程序在实例上创建链接服务器。  

```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'MSOLEDBSQL',   
   @datasrc=N'S1\instance1';  
```  

 下面的示例 `S1_instance1` [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序在实例上创建链接服务器。  
 
> [!IMPORTANT] 
> SQL Server Native Client OLE DB 提供程序 (SQLNCLI) 仍然不推荐使用，不建议在新的开发工作中使用它。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。
  
```sql  
EXEC sp_addlinkedserver     
   @server=N'S1_instance1',   
   @srvproduct=N'',  
   @provider=N'SQLNCLI',   
   @datasrc=N'S1\instance1';  
```  
  
### <a name="b-using-the-microsoft-ole-db-provider-for-microsoft-access"></a>B. 使用 Microsoft OLE DB Provider for Microsoft Access  
 Microsoft.Jet.OLEDB.4.0 访问接口连接到使用 2002-2003 格式的 Microsoft Access 数据库。 下面的示例将创建一个名为 `SEATTLE Mktg` 的链接服务器。  
  
> [!NOTE]  
> 此示例假定 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 已安装访问和示例 **northwind** 数据库，并且 **northwind** 数据库位于 C:\Msoffice\Access\Samples. 中。  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.Jet.OLEDB.4.0',   
   @srvproduct = N'OLE DB Provider for Jet',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.mdb';  
GO  
```  
  
 Microsoft.ACE.OLEDB.12.0 访问接口连接到使用 2007 格式的 Microsoft Access 数据库。 下面的示例将创建一个名为 `SEATTLE Mktg` 的链接服务器。  
  
> [!NOTE]  
> 此示例假定 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 已安装访问和示例 **northwind** 数据库，并且 **northwind** 数据库位于 C:\Msoffice\Access\Samples. 中。  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Mktg',   
   @provider = N'Microsoft.ACE.OLEDB.12.0',   
   @srvproduct = N'OLE DB Provider for ACE',  
   @datasrc = N'C:\MSOffice\Access\Samples\Northwind.accdb';  
GO  
```  
  
### <a name="c-using-the-microsoft-ole-db-provider-for-odbc-with-the-data_source-parameter"></a>C. 将 Microsoft OLE DB Provider for ODBC 与 data_source 参数一起使用  
 下面的示例创建一个名为的链接服务器 `SEATTLE Payroll` ，该服务器使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] ODBC (`MSDASQL`) 和 *Data_source* 参数的 OLE DB 提供程序。  
  
> [!NOTE]  
> 在使用该链接服务器之前，必须在该服务器中将指定的 ODBC 数据源名称定义为系统 DSN。  
  
```sql  
EXEC sp_addlinkedserver   
   @server = N'SEATTLE Payroll',   
   @srvproduct = N'',  
   @provider = N'MSDASQL',   
   @datasrc = N'LocalServer';  
GO  
```  
  
### <a name="d-using-the-microsoft-ole-db-provider-for-excel-spreadsheet"></a>D. 将 Microsoft OLE DB Provider 用于 Excel 电子表格  
 若要使用 Jet 的 OLE DB 提供程序创建链接服务器定义 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 来访问 1997-2003 格式的 Excel 电子表格，请先指定要选择的 excel 工作表的列和行，以在 excel 中创建命名范围。 这样，可以在分布式查询中将此范围的名称引用为表名称。  
  
```sql  
EXEC sp_addlinkedserver 'ExcelSource',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   'c:\MyData\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
GO  
```  
  
 若要访问 Excel 电子表格中的数据，请将单元范围与名称相关联。 以下查询通过使用先前设置的链接服务器，将指定的命名范围 `SalesData` 作为表来访问。  
  
```sql  
SELECT *  
   FROM ExcelSource...SalesData;  
GO  
```  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在可以访问远程共享的域帐户下运行，则可以使用 UNC 路径来代替映射驱动器。  
  
```sql  
EXEC sp_addlinkedserver 'ExcelShare',  
   'Jet 4.0',  
   'Microsoft.Jet.OLEDB.4.0',  
   '\\MyServer\MyShare\Spreadsheets\DistExcl.xls',  
   NULL,  
   'Excel 5.0';  
```  
  
 若要连接到 Excel 2007 格式的 Excel 电子表格，请使用 ACE 访问接口。  
  
```sql  
EXEC sp_addlinkedserver @server = N'ExcelDataSource',   
@srvproduct=N'ExcelData', @provider=N'Microsoft.ACE.OLEDB.12.0',   
@datasrc=N'C:\DataFolder\People.xlsx',  
@provstr=N'EXCEL 12.0' ;  
  
```  
  
### <a name="e-using-the-microsoft-ole-db-provider-for-jet-to-access-a-text-file"></a>E. 使用 Microsoft OLE DB Provider for Jet 访问文本文件  
 以下示例创建直接访问文本文件的链接服务器，而没有将这些文件链接为 Access .mdb 文件中的表。 访问接口为 `Microsoft.Jet.OLEDB.4.0`，访问接口字符串为 `Text`。  
  
 数据源是包含文本文件的目录的完整路径。 schema.ini 文件（描述文本文件的结构）必须与此文本文件存在于相同的目录中。 有关如何创建 Schema.ini 文件的详细信息，请参阅 Jet 数据库引擎文档。  
  
```sql  
--Create a linked server.  
EXEC sp_addlinkedserver txtsrv, N'Jet 4.0',   
   N'Microsoft.Jet.OLEDB.4.0',  
   N'c:\data\distqry',  
   NULL,  
   N'Text';  
GO  
  
--Set up login mappings.  
EXEC sp_addlinkedsrvlogin txtsrv, FALSE, Admin, NULL;  
GO  
  
--List the tables in the linked server.  
EXEC sp_tables_ex txtsrv;  
GO  
  
--Query one of the tables: file1#txt  
--using a four-part name.   
SELECT *   
FROM txtsrv...[file1#txt];  
```  
  
### <a name="f-using-the-microsoft-ole-db-provider-for-db2"></a>F. 使用 Microsoft OLE DB Provider for DB2  
 以下示例创建名为 `DB2` 的链接服务器，该服务器使用 `Microsoft OLE DB Provider for DB2`。  
  
```sql  
EXEC sp_addlinkedserver  
   @server=N'DB2',  
   @srvproduct=N'Microsoft OLE DB Provider for DB2',  
   @catalog=N'DB2',  
   @provider=N'DB2OLEDB',  
   @provstr=N'Initial Catalog=PUBS;  
       Data Source=DB2;  
       HostCCSID=1252;  
       Network Address=XYZ;  
       Network Port=50000;  
       Package Collection=admin;  
       Default Schema=admin;';  
```  
  
### <a name="g-add-a-sssdsfull-as-a-linked-server-for-use-with-distributed-queries-on-cloud-and-on-premises-databases"></a>G. 添加 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 作为链接服务器，以便用于云和本地数据库上的分布式查询  
 您可以添加一个 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 作为链接服务器，然后将它用于跨本地数据库和云数据库的分布式查询。 这是跨本地企业网络和 Azure 云的数据库混合解决方案的组件。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Box 产品包含 "分布式查询" 功能，通过该功能，您可以编写查询，以便将来自本地数据源和数据的数据与远程源中的数据合并， (包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 定义为链接服务器的非数据源的数据。 每个 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]（虚拟 master 除外）可以添加为单个链接服务器，然后在您的数据库应用程序中像任何其他数据库一样直接使用它。  
  
 使用 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的好处包括便于管理、高可用性和可伸缩性、使用熟悉的开发模型以及采用关系数据模型。 您的数据库应用程序要求决定了它将在云中如何使用 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。 您可以立即将所有数据移到 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]，或逐渐迁移一部分数据而将其余数据保留在本地。 对于此类混合数据库应用程序， [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 现在可以添加为链接服务器，并且数据库应用程序可以发出分布式查询，以便将数据与 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 本地数据源组合在一起。  
  
 下面是一个简单示例，说明如何 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 使用分布式查询连接到：  
  
```sql  
-- Configure the linked server  
-- Add one Azure SQL Database as Linked Server  
EXEC sp_addlinkedserver  
  @server='myLinkedServer', -- here you can specify the name of the linked server  
  @srvproduct='',       
  @provider='sqlncli', -- using SQL Server Native Client  
  @datasrc='myServer.database.windows.net',   -- add here your server name  
  @location='',  
  @provstr='',  
  @catalog='myDatabase'  -- add here your database name as initial catalog (you cannot connect to the master database)  

-- Add credentials and options to this linked server  
EXEC sp_addlinkedsrvlogin  
  @rmtsrvname = 'myLinkedServer',  
  @useself = 'false',  
  @rmtuser = 'myLogin',             -- add here your login on Azure DB  
  @rmtpassword = 'myPassword' -- add here your password on Azure DB  

EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;  

-- Now you can use the linked server to execute 4-part queries  
-- You can create a new table in the Azure DB  
EXEC ('CREATE TABLE t1tutut2(col1 int not null CONSTRAINT PK_col1 PRIMARY KEY CLUSTERED (col1) )') at myLinkedServer  
-- Insert data from your local SQL Server  
EXEC ('INSERT INTO t1tutut2 VALUES(1),(2),(3)') at myLinkedServer  
  
-- Query the data using 4-part names  
SELECT * FROM myLinkedServer.myDatabase.dbo.myTable  
```  
  
## <a name="see-also"></a>另请参阅  
 [分布式查询存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)   
 [sp_serveroption (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [sp_setnetname &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
