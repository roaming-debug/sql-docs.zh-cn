---
description: 对象层次结构语法 (Transact-SQL)
title: 对象层次结构语法 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], hierarchy syntax
ms.assetid: 7ed8df86-9fd2-4e09-96bc-5381fec85f65
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6ebfc2afdcbaa3ca5eddd5d311262ba19780e332
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205258"
---
# <a name="object-hierarchy-syntax-transact-sql"></a>对象层次结构语法 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Sp_OAGetProperty 和 sp_OASetProperty 的 *propertyname* 参数和 sp_OAMethod 的 *方法名称* 参数支持对象层次结构语法，该语法与类似 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 。 当使用该特殊语法时，这些参数具有以下通用格式。  
  
## <a name="syntax"></a>语法  
  
```  
  
'TraversedObject.PropertyOrMethod'  
```  
  
## <a name="arguments"></a>参数  
 *TraversedObject*  
 是层次结构中的一个 OLE 对象，该对象位于存储过程中指定的 *objecttoken* 下。 使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 语法指定一系列集合、对象属性和返回对象的方法。 该系列中的每个对象说明符必须用句号 (.) 分隔。  
  
 系列中的项可以是集合名。 使用下面的语法指定集合：  
  
 集合 ( "*item*" )   
  
 要求使用双引号 (")。 不支持用于集合的 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 惊叹号 (!) 语法。  
  
 *PropertyOrMethod*  
 *TraversedObject* 的属性或方法的名称。  
  
 若要通过使用 sp_OAGetProperty、sp_OASetProperty 或 sp_OAMethod 参数（包括对 sp_OAMethod 输出参数的支持）指定所有索引或方法参数，请使用下面的语法：  
  
 *PropertyOrMethod*  
  
 若要在圆括号内指定所有索引或方法参数（将导致忽略 sp_OAGetProperty、sp_OASetProperty 或 sp_OAMethod 的所有索引或方法参数），请使用下面的语法：  
  
 *PropertyOrMethod* ( [ *ParameterName*： =] "*parameter*" [，...] )   
  
 要求使用双引号 (")。 指定了所有位置参数后，才能指定命名参数。  
  
## <a name="remarks"></a>备注  
 如果未指定 *TraversedObject* ，则必须指定 *PropertyOrMethod* 。  
  
 如果未指定 *PropertyOrMethod* ，则 *TRAVERSEDOBJECT* 将作为 OLE 自动化存储过程中的对象标记输出参数返回。 如果指定了 *PropertyOrMethod* ，则将调用 *TraversedObject* 的属性或方法，并将属性值或方法返回值作为输出参数从 OLE 自动化存储过程返回。  
  
 如果 *TraversedObject* 列表中的任何项不返回 OLE 对象，则会引发错误。  
  
 有关 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] OLE 对象语法的详细信息，请参阅 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 文档。  
  
 有关 HRESULT 返回代码的详细信息，请参阅 [&#40;transact-sql&#41;sp_OACreate ](../../relational-databases/system-stored-procedures/sp-oacreate-transact-sql.md)。  
  
## <a name="examples"></a>示例  
 下面是使用 SQL-DMO SQLServer 对象的对象层次结构语法示例。  
  
```  
-- Get the AdventureWorks2012 Person.Address Table object.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address")',  
   @table OUT  
  
-- Get the Rows property of the AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAGetProperty @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").Rows',  
   @rows OUT  
  
-- Call the CheckTable method to validate the   
-- AdventureWorks2012 Person.Address table.  
EXEC @hr = sp_OAMethod @object,  
   'Databases("AdventureWorks2012").Tables("Person.Address").CheckTable',  
   @checkoutput OUT  
```  
  
## <a name="see-also"></a>另请参阅  
 [OLE 自动化示例脚本](../../relational-databases/stored-procedures/ole-automation-sample-script.md)   
 [OLE 自动存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
  
  
