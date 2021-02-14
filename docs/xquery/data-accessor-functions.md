---
title: 数据访问器函数 |Microsoft Docs
description: 了解如何使用 XQuery 数据访问器函数 fn： data () 、fn： string () 和 text () 。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- data-accessor functions [XQuery]
ms.assetid: 31bad04f-7c74-4773-9f83-612704fdd21c
author: rothja
ms.author: jroth
ms.openlocfilehash: 53ce941c88e2ab551ac0787126e2cf981d353819
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351071"
---
# <a name="data-accessor-functions"></a>数据取值函数
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  本节中的主题介绍数据取值函数并提供相应的示例代码。  
  
## <a name="understanding-fndata-fnstring-and-text"></a>了解 fn:data()、fn:string() 和 text()  
 XQuery 具有一个函数 **fn： data ()** 以便从节点提取标量、类型化的值、节点测试 **文本 ()** 返回文本节点，以及返回节点的字符串值的函数 **fn： string ()** 。 它们的用法容易混淆。 以下是在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 中正确使用它们的准则。 XML 实例 \<age> 12 \</age> 用于说明目的。  
  
-   非类型化的 XML：路径表达式 /age/text() 返回文本节点“12”。 函数 fn:data(/age) 返回字符串值“12”，fn:string(/age) 也是如此。  
  
-   类型化的 XML： expression/age/text () 为任何简单类型化元素返回一个静态错误 \<age> 。 另一方面，fn:data(/age) 返回整数 12。 fn:string(/age) 产生字符串“12”。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [&#40;XQuery 的字符串函数&#41;](../xquery/data-accessor-functions-string-xquery.md)  
  
-   [数据函数 &#40;XQuery&#41;](../xquery/data-accessor-functions-data-xquery.md)  
  
## <a name="see-also"></a>另请参阅  
 [&#41;&#40;XQuery 的路径表达式 ](../xquery/path-expressions-xquery.md)  
  
  
