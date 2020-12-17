---
title: 使用 T-SQL (CREATE EXTERNAL LIBRARY) 安装 R 包
description: 将新的 R 包添加到 SQL Server 2016 R Services 或 SQL Server 机器学习服务（数据库内）。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 61917bb0c62bdcbee114843bdd78bde80ae17619
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471008"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server"></a>使用 T-SQL (CREATE EXTERNAL LIBRARY) 将 R 包安装在 SQL Server 上
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

本文介绍如何在启用了机器学习的 SQL Server 实例上安装新的 R 包。 有多种方法可供选择。 使用 T-SQL 最适用于不熟悉 R 的服务器管理员。

使用 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) 语句，可以将包或包集添加到实例或特定数据库，而无需直接运行 R 或 Python 代码。 但是，此方法需要准备好包以及额外的数据库权限。

+ 所有包都必须作为本地压缩文件提供，而不是根据需要从 Internet 下载。

+ 必须按名称和版本标识所有依赖项，并将其包含在 zip 文件中。 如果所需的包不可用（包括下游包依赖项），则语句将失败。 

+ 你必须是 db_owner 或在数据库角色中享有 CREATE EXTERNAL LIBRARY 权限。 有关详细信息，请参阅 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)。

## <a name="download-packages-in-archive-format"></a>下载存档格式的包

如果要安装单个包，请下载压缩格式的包。

由于包依赖项，更常见的方法是安装多个包。 当包需要其他包时，必须在安装过程中验证是否所有包都可供访问。 建议使用 [miniCRAN](https://andrie.github.io/miniCRAN/) [创建本地存储库](create-a-local-package-repository-using-minicran.md)来组装一个完整的包集合，以及使用 [igraph](https://igraph.org/r/) 来分析包依赖项。 安装包的错误版本或省略包依赖项可能导致 CREATE EXTERNAL LIBRARY 语句失败。 

## <a name="copy-the-file-to-a-local-folder"></a>将文件复制到本地文件夹

将包含所有包的压缩文件复制到服务器上的本地文件夹。 如果无法访问服务器上的文件系统，还可以使用二进制格式将整个包作为变量传递。 有关详细信息，请参阅 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)。

## <a name="run-the-statement-to-upload-packages"></a>运行语句上传包

使用具有管理权限的帐户打开“查询”窗口。

运行 T-SQL 语句 `CREATE EXTERNAL LIBRARY` 将压缩包集合上传到数据库。

例如，下面的语句将 miniCRAN 存储库命名为包源，其中包含 randomForest 包及其依赖项。 

```sql
CREATE EXTERNAL LIBRARY [randomForest]
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

不能使用任意名称；外部库名称必须与加载或调用包时预期使用的名称相同。

## <a name="verify-package-installation"></a>验证包安装

如果成功创建库，则可以在 SQL Server 中运行包，方法是在存储过程中调用包。
    
```sql
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>另请参阅

+ [获取 R 包信息](r-package-information.md)
+ [R 教程](../tutorials/r-tutorials.md)