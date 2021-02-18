---
title: 使用 Python 绘制直方图进行数据探索
titleSuffix: SQL machine learning
description: 了解如何使用 Python 创建直方图来可视化数据。
author: dphansen
ms.author: davidph
ms.date: 07/14/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: f9c803760a3ee098e841cbc716cde8ac8a2fd648
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349106"
---
# <a name="plot-histograms-in-python"></a>在 Python 中绘制直方图 
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

本文介绍如何使用 Python 包 [pandas'.hist()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.hist.html) 来绘制数据。 SQL 数据库提供一种源可用于可视化具有连续的非重叠值的直方图数据间隔。

## <a name="prerequisites"></a>先决条件：

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
* [适用于 Windows 的 SQL Server](../../database-engine/install-windows/install-sql-server.md) 或 [适用于 Linux 的 SQL Server](../../linux/sql-server-linux-overview.md)
::: moniker-end

::: moniker range="=azuresqldb-current"
* [Azure SQL 数据库](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
* [Azure SQL 托管实例](/azure/azure-sql/managed-instance/instance-create-quickstart)

* 用于将示例数据库还原到 Azure SQL 托管实例的 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
::: moniker-end

* Azure Data Studio。 如需安装，请参阅 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。

* [还原示例 DW 数据库](../../samples/adventureworks-install-configure.md)以获取本文中使用的示例数据。

## <a name="verify-restored-database"></a>验证还原的数据库

可以通过查询 Person.CountryRegion 表来验证是否存在还原的数据库：
```sql
USE AdventureWorksDW;
SELECT * FROM Person.CountryRegion;
```
  
## <a name="install-python-packages"></a>安装 Python 包

[下载并安装 Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)。

安装以下 Python 包：
  * pyodbc
  * pandas

  若要安装这些包：

  1. 在 Azure Data Studio 笔记本中，选择“管理包”。
  2. 在“管理包”窗格中，选择“添加新包”选项卡。
  3. 对于以下每个包，输入包名称，单击“搜索”，然后单击“安装”。

## <a name="plot-histogram"></a>绘制直方图

直方图中显示的分布式数据基于 AdventureWorksDW 中的 SQL 查询。 直方图直观显示数据以及数据值的频率。 编辑连接字符串变量“服务器”、“数据库”、“用户名”和“密码”以连接到 SQL 数据库。

创建新笔记本的步骤：

1. 在 Azure Data Studio 中，依次选择“文件”和”新建笔记本” 。
2. 在笔记本中，依次选择内核“Python3”和“+ 代码” 。
3. 在笔记本中粘贴代码，选择“全部运行”。

```python
import pyodbc 
import pandas as plt
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorksDW' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
sql = "SELECT DATEDIFF(year, c.BirthDate, GETDATE()) AS Age FROM [dbo].[FactInternetSales] s INNER JOIN dbo.DimCustomer c ON s.CustomerKey = c.CustomerKey"
df = pd.read_sql(sql, cnxn)
df.hist(bins=10)
```

将显示 FactInternetSales 表中客户的年龄分布。

![Pandas 直方图](./media/python-histogram.png)