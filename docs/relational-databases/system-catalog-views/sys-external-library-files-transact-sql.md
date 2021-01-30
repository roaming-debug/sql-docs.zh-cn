---
description: sys.external_library_files (Transact-SQL)
title: sys.external_library_files (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- external_library_files
- external_library_files_TSQL
- sys.external_library_files
- sys.external_library_files_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_library_files catalog view
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 139843ab9a4c4c6a398c96cc3bcc3f7b50b297f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191546"
---
# <a name="sysexternal_library_files-transact-sql"></a>sys.external_library_files (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

列出组成外部库的每个文件对应一行。

|列名称 |数据类型 |说明|
|------|------|-----|
|external_library_id | int |外部库对象的 ID。 |
|内容 |varbinary(max) |外部库文件项目的内容。 |
|平台 |tinyint |安装 SQL Server 的主机平台的 ID。 |
|platform_desc | nvarchar(60) |主机平台的名称。 有效值为 "WINDOWS"、"LINUX"。 |

### <a name="see-also"></a>请参阅  

[sys.external_libraries](sys-external-libraries-transact-sql.md)  
[创建外部库](../../t-sql/statements/create-external-library-transact-sql.md)  
