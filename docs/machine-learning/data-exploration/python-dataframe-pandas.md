---
title: 将 SQL 表中的数据插入 Python pandas 数据框
titleSuffix: SQL machine learning
description: 了解如何读取 SQL 表中的数据并插入使用 Python 的 pandas 数据框。
author: dphansen
ms.author: davidph
ms.date: 07/23/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=azuresqldb-current'
ms.openlocfilehash: 7b26838dceee9b933b4f33621af3b32bdb0a0c67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349092"
---
# <a name="insert-data-from-a-sql-table-into-a-python-pandas-dataframe"></a>将 SQL 表中的数据插入 Python pandas 数据框
[!INCLUDE[SQL Server SQL DB SQL MI](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

本文介绍如何在 Python 中使用 [pyodbc](../../connect/python/pyodbc/python-sql-driver-pyodbc.md) 包将 SQL 数据插入 [pandas](https://pandas.pydata.org/) 数据框。 数据框中包含的数据的行和列可用于进一步的数据探索。

## <a name="prerequisites"></a>先决条件

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

* [还原示例数据库](../../samples/adventureworks-install-configure.md)以获取本文中使用的示例数据。

## <a name="verify-restored-database"></a>验证还原的数据库

可以通过查询 Person.CountryRegion 表来验证是否存在还原的数据库：

```sql
USE AdventureWorks;
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

## <a name="insert-data"></a>插入数据

使用以下脚本选择 Person.CountryRegion 表中的数据并插入数据框中。 编辑连接字符串变量“服务器”、“数据库”、“用户名”和“密码”以连接到 SQL。

创建新笔记本的步骤：

1. 在 Azure Data Studio 中，依次选择“文件”和”新建笔记本” 。
2. 在笔记本中，依次选择内核“Python3”和“+ 代码” 。
3. 在笔记本中粘贴代码，选择“全部运行”。

```python
import pyodbc
import pandas as pd
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'servername' 
database = 'AdventureWorks' 
username = 'yourusername' 
password = 'databasename'  
cnxn = pyodbc.connect('DRIVER={SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
# select 26 rows from SQL table to insert in dataframe.
query = "SELECT [CountryRegionCode], [Name] FROM Person.CountryRegion;"
df = pd.read_sql(query, cnxn)
print(df.head(26))
```

**输出**

上述脚本中的 `print` 命令显示 `pandas` 数据框 `df` 中的数据行。

```text
CountryRegionCode                 Name
0                 AF          Afghanistan
1                 AL              Albania
2                 DZ              Algeria
3                 AS       American Samoa
4                 AD              Andorra
5                 AO               Angola
6                 AI             Anguilla
7                 AQ           Antarctica
8                 AG  Antigua and Barbuda
9                 AR            Argentina
10                AM              Armenia
11                AW                Aruba
12                AU            Australia
13                AT              Austria
14                AZ           Azerbaijan
15                BS         Bahamas, The
16                BH              Bahrain
17                BD           Bangladesh
18                BB             Barbados
19                BY              Belarus
20                BE              Belgium
21                BZ               Belize
22                BJ                Benin
23                BM              Bermuda
24                BT               Bhutan
25                BO              Bolivia
```

## <a name="next-steps"></a>后续步骤

+ [将 Python 数据框插入 SQL](../data-exploration/python-dataframe-sql-server.md)