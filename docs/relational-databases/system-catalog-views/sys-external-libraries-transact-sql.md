---
description: sys.external_libraries (Transact-SQL)
title: sys.external_libraries (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_libraries
- external_libraries_TSQL
- sys.external_libraries
- sys.external_libraries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_libraries catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 6370cfdd213c7670b22066395534928f980766c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354205"
---
# <a name="sysexternal_libraries-transact-sql"></a>sys.external_libraries (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

支持管理与外部运行时（如 R、Python 和 Java）相关的包库。

> [!NOTE]
> 在 SQL Server 2017 中，支持 R 语言和 Windows 平台。 SQL Server 2019 及更高版本支持 Windows 和 Linux 平台上的 R、Python 和 Java。 在 Azure SQL 托管实例上，支持 R 和 Python。

## <a name="sysexternal_libraries"></a>sys.external_libraries

目录视图 sys.external_libraries 为已上传到数据库的每个外部库列出一行。

|列名称 |数据类型 | 说明|
|------|------|------|
|external_library_id |int | 外部库对象的 ID。 |
|name |sysname |外部库的名称。 在数据库中每个所有者都是唯一的。|
|principal_id |int |拥有此外部库的主体的 ID。 |
|语言 | sysname | 支持外部库的语言或运行时的名称。 有效值为 "R"、"Python" 和 "Java"。 将来可能会添加其他运行时。|
|scope |int |0表示公共作用域;1用于专用范围 |  
|scope_desc |varchar (7)  |指示包是公共包还是私有包|

## <a name="see-also"></a>请参阅  

+ [sys.external_library_files](sys-external-library-files-transact-sql.md)  
+ [创建外部库](../../t-sql/statements/create-external-library-transact-sql.md)  
+ [安装新 R 包](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md)  
+ [安装新 Python 包](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md)  
