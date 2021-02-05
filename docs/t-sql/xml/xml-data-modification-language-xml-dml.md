---
description: XML 数据修改语言 (XML DML)
title: XML 数据修改语言 (XML DML)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- modifying data [SQL Server], XML DML
- XML [SQL Server], DML
- XML DML [SQL Server]
- data modification language [XML DML]
- data modifications [XML DML]
- DML [XML in SQL Server]
- XQuery, XML DML
- xml data type [SQL Server], XML DML
ms.assetid: 20ce50d2-c07b-4e41-93a7-1380d2cd49cb
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b9296671957cfb5d8ee0bcd41ae94362a85f42c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181685"
---
# <a name="xml-data-modification-language-xml-dml"></a>XML 数据修改语言 (XML DML)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  XML 数据修改语言 (XML DML) 是 XQuery 语言的扩展。 根据 W3C 的定义，XQuery 语言缺少数据操作 (DML) 部分。 本主题所介绍的 XML DML 以及 XQuery 语言，提供完整的功能查询和数据修改语言，你可以使用它们对 xml 数据类型进行操作。  
  
 XML DML 将下列区分大小写的关键字添加到 XQuery 中：  
  
-   **insert**  
  
-   **delete**  
  
-   **replace value of**  
  
 如 [XML 数据类型和列 &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md) 中所述，可以创建 xml 类型的变量和列，并向它们分配 XML 文档或片断。 若要修改或更新这些 XML 实例，请执行下列操作：  
  
-   使用 xml 数据类型的 [modify() 方法（xml 数据类型）](../../t-sql/xml/modify-method-xml-data-type.md)。  
  
-   指定 modify() 方法中相应的 XML DML 语句。  
  
 请注意，存在某些无法插入、删除或修改其值的属性。 例如：  
  
-   对于类型化或非类型化的 xml 而言，这样的属性有 xmlns、xmlns:\* 和 xml:base  。  
  
-   仅对于类型化的 xml 而言，这样的属性有 xsi:nil 和 xsi:type  。  
  
 下面列出了其他限制：  
  
-   对于类型化或非类型化的 xml，插入 xml:base 属性将失败 。  
  
-   对于类型化的 xml，删除和修改 xsi:nil 属性将失败 。 对于非类型化的 xml，则可以删除此属性或修改此属性的值。  
  
-   对于类型化的 xml，修改 xs:type 属性值将失败 。 对于非类型化的 xml，则可以修改此属性值。  
  
 修改类型化的 XML 实例时，最终格式必须是该类型的有效实例。 否则，将返回一个验证错误。  
  
## <a name="see-also"></a>另请参阅  
 [insert &#40;XML DML&#41;](../../t-sql/xml/insert-xml-dml.md)   
 [delete &#40;XML DML&#41;](../../t-sql/xml/delete-xml-dml.md)   
 [replace value of &#40;XML DML&#41;](../../t-sql/xml/replace-value-of-xml-dml.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [创建 XML 数据的实例](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [xml 数据类型方法](../../t-sql/xml/xml-data-type-methods.md)  
  
  
