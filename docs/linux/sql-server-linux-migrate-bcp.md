---
title: 将数据批量复制到 Linux 上的 SQL Server
description: 本文介绍 bcp 实用工具。 使用 bcp 将许多行导入 SQL Server 表中，或将数据从 SQL Server 表导出到数据文件。
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 7b93d0d7-7946-4b78-b33a-57d6307cdfa9
ms.openlocfilehash: c27e4095854abd409336c97ac5b8458bf6f62894
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345612"
---
# <a name="bulk-copy-data-with-bcp-to-sql-server-on-linux"></a>使用 bcp 将数据批量复制到 Linux 上的 SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文介绍如何使用 [bcp](../tools/bcp-utility.md) 命令行实用工具，在 Linux 上的 SQL Server 实例和采用用户指定格式的数据文件之间批量复制数据。

可以使用 `bcp` 将许多行导入 SQL Server 表中，或将数据从 SQL Server 表导出到数据文件。 除非与 queryout 选项配合使用，否则使用 `bcp` 不需要了解 Transact-SQL 知识。 `bcp` 命令行实用工具可用于在本地或云中、在 Linux、Windows 或 Docker 上以及在 Azure SQL 数据库和 Azure Synapse Analytics 中运行的 Microsoft SQL Server。

本文介绍如何：
- 使用 `bcp in` 命令将数据导入表
- 使用 `bcp out` 命令从表中导出数据

## <a name="install-the-sql-server-command-line-tools"></a>安装 SQL Server 命令行工具

`bcp` 是 SQL Server 命令行工具的一部分，不会随 Linux 上的 SQL Server 自动安装。 如果尚未在 Linux 计算机上安装 SQL Server 命令行工具，则必须先安装它们。 有关如何安装这些工具的详细信息，请从以下列表中选择 Linux 分发版：

- [Red Hat Enterprise Linux (RHEL)](sql-server-linux-setup-tools.md#RHEL)
- [Ubuntu](sql-server-linux-setup-tools.md#ubuntu)
- [SUSE Linux Enterprise Server (SLES)](sql-server-linux-setup-tools.md#SLES)

## <a name="import-data-with-bcp"></a>使用 bcp 导入数据

在本教程中，将在本地 SQL Server 实例 (**localhost**) 上创建示例数据库和表，然后使用 `bcp` 从磁盘上的文本文件加载到示例表。

### <a name="create-a-sample-database-and-table"></a>创建示例数据库和表

首先创建一个具有简单表的示例数据库，本教程接下来会使用该数据库。

1. 在 Linux 框中，打开命令终端。

2. 将以下命令复制并粘贴到终端窗口中。 这些命令使用 **sqlcmd** 命令行实用工具在本地 SQL Server 实例 (**localhost**) 上创建示例数据库 (**BcpSampleDB**) 和表 (**TestEmployees**)。 运行命令前，请记得根据需要替换 `username` 和 `<your_password>`。

创建数据库 **BcpSampleDB**：
```bash 
sqlcmd -S localhost -U sa -P <your_password> -Q "CREATE DATABASE BcpSampleDB;"
```
在数据库 **BcpSampleDB** 中创建表 **TestEmployees**：
```bash 
sqlcmd -S localhost -U sa -P <your_password> -d BcpSampleDB -Q "CREATE TABLE TestEmployees (Id INT IDENTITY(1,1) NOT NULL PRIMARY KEY, Name NVARCHAR(50), Location NVARCHAR(50));"
```
### <a name="create-the-source-data-file"></a>创建源数据文件
将以下命令复制并粘贴到终端窗口中。 我们使用内置的 `cat` 命令创建包含三条记录的示例文本数据文件，并将文件作为 **~/test_data.txt** 保存在主目录中。 记录中的字段用逗号分隔。

```bash
cat > ~/test_data.txt << EOF
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
EOF
```

可通过在终端窗口中运行以下命令，验证是否已正确创建数据文件：
```bash 
cat ~/test_data.txt
```

终端窗口中应该会显示以下内容：
```bash
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

### <a name="import-data-from-the-source-data-file"></a>从源数据文件导入数据
将以下命令复制并粘贴到终端窗口中。 此命令使用 `bcp` 连接到本地 SQL Server 实例 (**localhost**)，并将数据文件 ( **~/test_data.txt**) 中的数据导入数据库 (**BcpSampleDB**) 中的表 (**TestEmployees**)。 运行命令前，请记得根据需要替换用户名和 `<your_password>`。

```bash 
bcp TestEmployees in ~/test_data.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t  ','
```

以下是此示例中与 `bcp` 配合使用的命令行参数的简要概述：
- `-S`：指定要连接到的 SQL Server 实例
- `-U`：指定用于连接到 SQL Server 的登录 ID
- `-P`：指定登录 ID 的密码
- `-d`：指定要连接到的数据库
- `-c`：使用字符数据类型执行操作
- `-t`：指定字段终止符。 我们在数据文件中使用 `comma` 作为记录的字段终止符

> [!NOTE]
> 本示例中不指定自定义行终止符。 先前使用 `cat` 命令创建数据文件时，文本数据文件中的行已使用 `newline` 正确终止。

可通过在终端窗口中运行以下命令，验证是否已成功导入数据。 运行命令前，请记得根据需要替换 `username` 和 `<your_password>`。
```bash 
sqlcmd -S localhost -d BcpSampleDB -U sa -P <your_password> -I -Q "SELECT * FROM TestEmployees;"
```

应该会显示以下结果：
```bash
Id          Name                Location
----------- ------------------- -------------------
          1 Jared               Australia
          2 Nikita              India
          3 Tom                 Germany

(3 rows affected)
```

## <a name="export-data-with-bcp"></a>使用 bcp 导出数据

在本教程中，将使用 `bcp` 把先前创建的示例表中的数据导出到新数据文件。

将以下命令复制并粘贴到终端窗口中。 这些命令使用 `bcp` 命令行实用工具，将数据库 **BcpSampleDB** 中表 **TestEmployees** 的数据导出到名为 **~/test_export.txt** 的新数据文件。  运行命令前，请记得根据需要替换用户名和 `<your_password>`。

```bash 
bcp TestEmployees out ~/test_export.txt -S localhost -U sa -P <your_password> -d BcpSampleDB -c -t ','
```

可通过在终端窗口中运行以下命令，验证是否已正确导出数据：
```bash 
cat ~/test_export.txt
```

终端窗口中应该会显示以下内容：
```
1,Jared,Australia
2,Nikita,India
3,Tom,Germany
```

## <a name="see-also"></a>另请参阅
- [bcp 实用工具](../tools/bcp-utility.md)
- [使用 bcp 时用以获得兼容性的数据格式](../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)
- [使用 BULK INSERT 导入批量数据](../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md)
- [BULK INSERT (Transact-SQL)](../t-sql/statements/bulk-insert-transact-sql.md)
