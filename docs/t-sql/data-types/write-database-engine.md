---
description: Write（数据库引擎）
title: Write（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Write_TSQL
- Write
dev_langs:
- TSQL
helpviewer_keywords:
- Write [Database Engine]
ms.assetid: 7c554334-d2d9-4eae-a4ae-097aa4020e1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: aba1271dd68224d237595a81d61b23d8db38b7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211484"
---
# <a name="write-database-engine"></a>Write（数据库引擎）
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Write 将 SqlHierarchyId 的二进制表示形式写出到传入的 BinaryWriter 中。 无法通过使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 来调用 Write。 请改为使用 CAST 或 CONVERT。
  
## <a name="syntax"></a>语法  
  
```csharp
void Write( BinaryWriter w )
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*w*  
一个 BinaryWriter 对象，此 hierarchyid 节点的二进制表示形式将写在该对象中。
  
## <a name="return-types"></a>返回类型  
CLR 返回类型：void 
  
## <a name="remarks"></a>备注  
必要时（例如，从 hierarchyid 列加载数据时），[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将在内部使用 Write。 在 hierarchyid 和 varbinary 之间进行转换时，也将在内部调用 Write。
  
## <a name="examples"></a>示例  
  
```csharp
MemoryStream stream = new MemoryStream();  
BinaryWriter bw = new BinaryWriter(stream);  
hid.Write(bw);  
byte[] encoding = stream.ToArray();  
```  
  
## <a name="see-also"></a>另请参阅
[Read（数据库引擎）](../../t-sql/data-types/read-database-engine.md)  
[ToString（数据库引擎）](../../t-sql/data-types/tostring-database-engine.md)  
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[hierarchyid 数据类型方法引用](./hierarchyid-data-type-method-reference.md)
  
