---
title: 步骤 3：使用 pyodbc 连接到 SQL
description: 第 3 步是概念证明，展示了如何使用 Python 和 pyODBC 连接到 SQL Server。 基本示例展示了如何选择和插入数据。
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4bfd6e52-817d-4f0a-a33d-11466e3f0484
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 808cb17d29362e76fdcc69171b21c9ffffd5c1ed
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837124"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-pyodbc"></a>步骤 3：使用 pyodbc 连接到 SQL 的概念证明

此示例是一个概念证明。 为了清楚起见，此示例代码已经过简化，并不一定代表 Microsoft 建议的最佳做法。  

若要开始操作，请运行以下示例脚本。 创建一个名为“test.py”的文件，并根据需要添加每个代码片段。

```
> python test.py
```
  
## <a name="connect"></a>连接  
  
```python
import pyodbc 
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'tcp:myserver.database.windows.net' 
database = 'mydb' 
username = 'myusername' 
password = 'mypassword' 
cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()

```  

## <a name="run-query"></a>运行查询  
  
cursor.execute 函数可用于从针对 SQL 数据库的查询中检索结果集。 此函数可接受查询，并返回可使用 cursor.fetchone() 循环访问的结果集

```python
#Sample select query
cursor.execute("SELECT @@version;") 
row = cursor.fetchone() 
while row: 
    print(row[0])
    row = cursor.fetchone()

```  
  
## <a name="insert-a-row"></a>插入行  
  
此示例展示了如何安全地运行 [INSERT](../../../t-sql/statements/insert-transact-sql.md) 语句并传递参数。 参数保护应用程序不进行 [SQL 注入](../../../relational-databases/tables/primary-and-foreign-key-constraints.md)。

```python
#Sample insert query
count = cursor.execute("""
INSERT INTO SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) 
VALUES (?,?,?,?,?)""",
'SQL Server Express New 20', 'SQLEXPRESS New 20', 0, 0, CURRENT_TIMESTAMP).rowcount
cnxn.commit()
print('Rows inserted: ' + str(count))
```  

## <a name="azure-active-directory-and-the-connection-string"></a>Azure Active Directory 和连接字符串

pyODBC 使用 Microsoft ODBC Driver for SQL Server。
如果 ODBC 驱动程序的版本为 17.1 或更高版本，则可以通过 pyODBC 使用 ODBC 驱动程序的 Azure Active Directory 交互模式。
如果 Python 和 pyODBC 允许 ODBC 驱动程序显示对话框，则此交互选项有效。 仅在 Windows 操作系统上提供此选项。

### <a name="example-connection-string-for-azure-active-directory-interactive-authentication"></a>用于 Azure Active Directory 交互式身份验证的连接字符串示例

以下示例提供指定 Azure Active Directory 交互式身份验证的 ODBC 连接字符串：

`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

有关 ODBC 驱动程序身份验证选项的详细信息，请参阅[结合使用 Azure Active Directory 和 ODBC 驱动程序](../../odbc/using-azure-active-directory.md#new-andor-modified-dsn-and-connection-string-keywords)。

## <a name="next-steps"></a>后续步骤
  
有关详细信息，请参阅 [Python 开发人员中心](https://azure.microsoft.com/develop/python/)。
