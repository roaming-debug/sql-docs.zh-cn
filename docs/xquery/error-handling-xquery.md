---
title: )  (XQuery 处理错误Microsoft Docs
description: 了解 XQuery 中的错误处理，并查看处理动态错误的示例。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- static errors
- errors [XQuery]
- XQuery, error handling
- dynamic errors [XQuery]
ms.assetid: 7dee3c11-aea0-4d10-9126-d54db19448f2
author: rothja
ms.author: jroth
ms.openlocfilehash: 61e6b8b53e4519eb88f94f9521434b2e48aac40d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335856"
---
# <a name="error-handling-xquery"></a>错误处理 (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  W3C 规范允许静态或动态引发类型错误，并定义了静态错误、动态错误和类型错误。  
  
## <a name="compilation-and-error-handling"></a>编译和错误处理  
 语法不正确的 Xquery 表达式和 XML DML 语句会返回编译错误。 编译阶段会检查 XQuery 表达式和 DML 语句的静态类型正确性，并针对类型化的 XML 使用 XML 架构进行类型推理。 如果表达式在运行时由于类型安全冲突而失败，会引起静态类型错误。 静态错误的示例包括将字符串添加到整数，以及在不存在的节点中查询类型化的数据。  
  
 与 W3C 标准有所不同的是，XQuery 运行时错误被转换为空序列。 这些序列根据调用上下文，可以作为空 XML 或 NULL 传播到查询结果。  
  
 通过显式转换为正确的类型，用户可以解决静态错误的问题，尽管运行时转换错误将被转换为空序列。  
  
## <a name="static-errors"></a>静态错误  
 静态错误是通过使用 [!INCLUDE[tsql](../includes/tsql-md.md)] 错误机制返回的。 在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中，XQuery 类型错误是静态返回的。 有关详细信息，请参阅 [XQuery 和静态类型](../xquery/xquery-and-static-typing.md)化。  
  
## <a name="dynamic-errors"></a>动态错误  
 在 XQuery 中，大部分动态错误都映射到一个空序列（即“()”）。 不过，有两个例外：XQuery 聚合函数中的溢出条件和 XML-DML 验证错误。 请注意，大部分动态错误都映射到一个空序列。 另外，使用 XML 索引的查询执行可能引发错误。 因此，为了能够有效地执行索引而不生成意外错误，[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]会将动态错误映射到 ()。  
  
 通常，在谓词内出现动态错误的情况下，不引发错误就不会更改语义，因为 () 映射到 False。 但是，在某些情况下，返回 () 而不返回动态错误可能导致意外结果。 下列示例说明了这一点。  
  
### <a name="example-using-the-avg-function-with-a-string"></a>示例：使用带字符串的 avg() 函数  
 在下面的示例中，调用 [avg 函数](../xquery/aggregate-functions-avg.md) 来计算三个值的平均值。 这些值中的一个是字符串。 由于此情况下的 XML 实例是非类型化的，因此其中的所有数据都是非类型化的原子类型。 **Avg ()** 函数先将这些值转换为 **xs： double** ，然后再计算平均值。 但是， `"Hello"` 不能将值强制转换为 **xs： double** ，并创建动态错误。 在这种情况下，将转换 `"Hello"` 为 **xs： double** 会导致空序列，而不是返回动态错误。 **Avg ()** 函数忽略此值，计算两个值的平均值，并返回150。  
  
```  
DECLARE @x xml  
SET @x=N'<root xmlns:myNS="test">  
 <a>100</a>  
 <b>200</b>  
 <c>Hello</c>  
</root>'  
SELECT @x.query('avg(//*)')  
```  
  
### <a name="example-using-the-not-function"></a>示例：使用 not 函数  
 如果在谓词中使用 [not 函数](../xquery/functions-on-boolean-values-not-function.md) （例如）， `/SomeNode[not(Expression)]` 并且表达式导致动态错误，则将返回空序列而不是错误。 将 **not ()** 应用于空序列将返回 True，而不是错误。  
  
### <a name="example-casting-a-string"></a>示例：转换字符串  
 在下面的示例中，文字字符串“NaN”先转换为 xs:string，再转换为 xs:double。 结果是一个空行集。 虽然字符串“NaN”无法成功转换为 xs:double，但直到运行时才能确定，因为该字符串首先转换为 xs:string。  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double(xs:string("NaN")) ')  
GO  
```  
  
 不过，在此示例中，生成一个静态类型错误。  
  
```  
DECLARE @x XML  
SET @x = ''  
SELECT @x.query(' xs:double("NaN") ')  
GO  
```  
  
#### <a name="implementation-limitations"></a>实现限制  
 不支持 **fn： error ()** 函数。  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 语言参考 (SQL Server)](../xquery/xquery-language-reference-sql-server.md)   
 [XQuery 基础知识](../xquery/xquery-basics.md)  
  
  
