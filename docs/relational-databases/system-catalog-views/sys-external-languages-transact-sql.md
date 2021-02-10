---
description: sys.external_languages (Transact-sql) -SQL Server
title: sys.external_languages (Transact-sql) -SQL Server |Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- external_languages
- external_languages_TSQL
- sys.external_languages
- sys.external_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_languages catalog view
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 425971304e516670bd15bfcdcc0a3b5777091cc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061583"
---
# <a name="sysexternal_languages-transact-sql"></a>sys.external_languages (Transact-sql) 
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

此目录视图提供数据库中的外部语言列表。 “R”和“Python”是保留名称，不能使用这些特定名称创建外部语言   。

## <a name="sysexternal_languages"></a>sys.external_languages

目录视图 sys.external_languages 为数据库中的每个外部语言列出一行。

|列名称 |数据类型 | 说明|
|------|------|------|
|external_language_id |int | 外部语言的 ID|
|语言 |sysname |外部语言的名称。 在该数据库中是唯一的。 R 和 Python 是每个实例的保留名称|
|create_date |datetime2 |创建日期和时间|
|principal_id |int |拥有此外部库的主体的 ID|

## <a name="see-also"></a>另请参阅  

+ [sys.external_language_files](sys-external-language-files-transact-sql.md)  
+ [创建外部语言](../../t-sql/statements/create-external-language-transact-sql.md) 
