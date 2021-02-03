---
description: ASSEMBLYPROPERTY (Transact-SQL)
title: ASSEMBLYPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ASSEMBLYPROPERTY_TSQL
- ASSEMBLYPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- ASSEMBLYPROPERTY statement
- assemblies [CLR integration], properties
ms.assetid: cf03d1b1-724c-48bf-a8df-3fe2586b150a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a5acce18561975c29a3e5f22f74227e079f422f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210008"
---
# <a name="assemblyproperty-transact-sql"></a>ASSEMBLYPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

此函将返回有关程序集属性的信息。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ASSEMBLYPROPERTY('assembly_name', 'property_name')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
assembly_name  
程序集的名称。
  
property_name  
要检索其有关信息的属性的名称。 property_name 可以具有下列值之一：
  
|值|描述|  
|---|---|
|**CultureInfo**|程序集的区域设置。|  
|PublicKey|程序集的公钥或公钥令牌。|  
|**MvID**|由编译器生成的完整的程序集版本标识号。|  
|**VersionMajor**|由四部分组成的程序集版本标识号的主要组成部分（第一部分）。|  
|**VersionMinor**|由四部分组成的程序集版本标识号的次要组成部分（第二部分）。|  
|**VersionBuild**|由四部分组成的程序集版本标识号的内部版本组成部分（第三部分）。|  
|**VersionRevision**|由四部分组成的程序集版本标识号的修订版组成部分（第四部分）。|  
|**SimpleName**|程序集的简称。|  
|**体系结构**|程序集的处理器体系结构。|  
|**CLRName**|对程序集的简单名称、版本号、区域性、公钥以及体系结构进行编码的规范字符串。 该值唯一地标识公共语言运行时 (CLR) 端的程序集。|  
  
## <a name="return-type"></a>返回类型
**sql_variant**
  
## <a name="examples"></a>示例  
此示例假定在 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中注册了 `HelloWorld` 程序集。 有关详细信息，请参阅 [Hello World 示例](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130))。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ASSEMBLYPROPERTY ('HelloWorld' , 'PublicKey');  
```  
  
## <a name="see-also"></a>另请参阅
[CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)  
[DROP ASSEMBLY (Transact-SQL)](../../t-sql/statements/drop-assembly-transact-sql.md)
  
