---
title: 将 Python 数据帧插入 SQL 表
titleSuffix: SQL machine learning
description: 如何将数据从数据帧插入 SQL 表。
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current||=sqlallproducts-allversions'
ms.openlocfilehash: 85980bf7bc69190a0e7ae75ee74336a62afd12c6
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870210"
---
# <a name="insert-python-dataframe-into-sql-table"></a>将 Python 数据帧插入 SQL 表
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

本文介绍如何使用 Python 中的 [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) 包在 SQL 数据库中插入 [pandas](https://pandas.pydata.org/) 数据帧。

## <a name="prerequisites"></a>先决条件

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions"
* [适用于 Windows 的 SQL Server](../../database-engine/install-windows/install-sql-server.md) 或 [适用于 Linux 的 SQL Server](../../linux/sql-server-linux-overview.md)
::: moniker-end

::: moniker range="=azuresqldb-current||=sqlallproducts-allversions"
* [Azure SQL 数据库](/azure/sql-database/sql-database-get-started-portal)
::: moniker-end

::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
* [Azure SQL 托管实例](/azure/azure-sql/managed-instance/instance-create-quickstart)

* 用于将示例数据库还原到 Azure SQL 托管实例的 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)。
::: moniker-end

* Azure Data Studio。 如需安装，请参阅 [Azure Data Studio](../../azure-data-studio/what-is.md)。

* [还原示例数据库](../../samples/adventureworks-install-configure.md)以获取本文中使用的示例数据。

## <a name="verify-restored-database"></a>验证还原的数据库

可以通过查询“HumanResources.Department”表来验证是否存在还原的数据库：

```sql
USE AdventureWorks;
SELECT * FROM HumanResources.Department;
```

## <a name="install-python-packages"></a>安装 Python 包

* [下载并安装 Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)

安装以下 Python 包：
  * pyodbc
  * pandas

  若要安装这些包：

  1. 在 Azure Data Studio 笔记本中，选择“管理包”。
  2. 在“管理包”窗格中，选择“添加新包”选项卡。
  3. 对于以下每个包，输入包名称，单击“搜索”，然后单击“安装”。

## <a name="connect-to-sql-server-using-azure-data-studio"></a>使用 Azure Data Studio 连接到 SQL Server

[使用 Azure Data Studio 连接](../../azure-data-studio/quickstart-sql-server.md)。

1. 连接到 Adventureworks 数据库以创建新表“HumanResources.DepartmentTest”。 SQL 表将用于插入数据帧。

```sql
CREATE TABLE [HumanResources].[DepartmentTest](
[DepartmentID] [smallint] NOT NULL,
[Name] [dbo].[Name] NOT NULL,
[GroupName] [dbo].[Name] NOT NULL
)
GO
```

## <a name="create-csv-file"></a>创建 CSV 文件

复制文本，并将文件另存为数据帧的 department.csv。

```text
DepartmentID,Name,GroupName,
1,Engineering,Research and Development,
2,Tool Design,Research and Development,
3,Sales,Sales and Marketing,
4,Marketing,Sales and Marketing,
5,Purchasing,Inventory Management,
6,Research and Development,Research and Development,
7,Production,Manufacturing,
8,Production Control,Manufacturing,
9,Human Resources,Executive General and Administration,
10,Finance,Executive General and Administration,
11,Information Services,Executive General and Administration,
12,Document Control,Quality Assurance,
13,Quality Assurance,Quality Assurance,
14,Facilities and Maintenance,Executive General and Administration,
15,Shipping and Receiving,Inventory Management,
16,Executive,Executive General and Administration
```

## <a name="connect-to-sql-using-python"></a>使用 Python 连接到 SQL

1. 编辑连接字符串变量“server”、“database”、“username”和“password”，以连接到 SQL 数据库。

2. 编辑 CSV 文件的路径。

## <a name="load-dataframe-from-csv-file"></a>从 CSV 文件加载数据帧

使用 Python `pandas` 包创建数据帧并加载 CSV 文件。 连接到 SQL 以将数据帧加载到新的 SQL 表“HumanResources.DepartmentTest”。

创建新笔记本的步骤：

1. 在 Azure Data Studio 中，依次选择“文件”和”新建笔记本” 。
2. 在笔记本中，依次选择内核“Python3”和“+ 代码” 。
3. 在笔记本中粘贴代码，选择“全部运行”。

 ```Python
import pyodbc
import pandas as pd
# insert data from csv file into dataframe.
# working directory for csv file: type "pwd" in Azure Data Studio or Linux
# working directory in Windows c:\users\username
df = pd.read_csv("c:\\user\\username\department.csv")
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'yourservername' 
database = 'AdventureWorks' 
username = 'username' 
password = 'yourpassword' 
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# Insert Dataframe into SQL Server:
for index, row in df.iterrows():
     cursor.execute("INSERT INTO HumanResources.DepartmentTest (DepartmentID,Name,GroupName) values(?,?,?)", row.DepartmentID, row.Name, row.GroupName)
cnxn.commit()
cursor.close()
```

## <a name="confirm-row-count-in-sql"></a>在 SQL 中确认行计数

执行 SQL 语句，确认表是否已成功从数据帧中加载数据。

```sql
SELECT count(*) from HumanResources.DepartmentTest;
```

结果

```bash
(No column name)
16
```

## <a name="next-steps"></a>后续步骤

+ [使用 Python 绘制用于数据探索的直方图](../data-exploration/python-plot-histogram.md)