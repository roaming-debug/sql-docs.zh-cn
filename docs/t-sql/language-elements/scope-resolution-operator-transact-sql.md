---
description: ::（作用域解析）(Transact-SQL)
title: ::（作用域解析）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 764d8f91-957b-4037-997b-a9b6b533c504
author: cawrites
ms.author: chadam
ms.openlocfilehash: b2a8c285533f7feac07941360bc8d7a85db74f6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194253"
---
# <a name="-scope-resolution-transact-sql"></a>::（作用域解析）(Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  作用域解析运算符 :: 提供对复合数据类型的静态成员的访问。 复合数据类型是指包含多个简单数据类型和方法的数据类型。 复合数据类型包括内置的 CLR 类型和自定义 SQLCLR 用户定义类型 (UDT)。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何使用作用域解析运算符访问 `GetRoot()` 类型的 `hierarchyid` 成员。  
  
```sql  
DECLARE @hid hierarchyid;  
SELECT @hid = hierarchyid::GetRoot();  
PRINT @hid.ToString();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `/`  
  
## <a name="see-also"></a>另请参阅  
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
 
