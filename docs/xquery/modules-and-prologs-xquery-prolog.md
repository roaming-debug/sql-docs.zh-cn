---
title: XQuery 序言 |Microsoft Docs
description: 了解 XQuery prolog，其中包含一系列声明和定义，这些声明和定义创建查询处理所需的环境。
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
- namespaces [XQuery]
- default namespace declarations
ms.assetid: 03924684-c5fd-44dc-8d73-c6ab90f5e069
author: rothja
ms.author: jroth
ms.openlocfilehash: 2b460f6da448e45d688ebcc08f418a9acbae85b8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341838"
---
# <a name="modules-and-prologs---xquery-prolog"></a>模块和 Prolog - XQuery Prolog
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery 查询由一个 prolog 和一个主体组成。 XQuery prolog 是一系列声明和定义，它们共同创建所需的查询处理环境。 在 SQL Server 中，XQuery prolog 可以包含命名空间声明。 XQuery 主体由指定预期查询结果的一些表达式组成。  
  
 例如，对 **xml** 类型的说明列指定了以下 XQuery，该指令将生产说明存储为 xml。 该查询将检索生产车间 `10` 的生产说明。 `query()` **Xml** 数据类型的方法用于指定 XQuery。  
  
```  
SELECT Instructions.query('declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";           
    /AWMI:root/AWMI:Location[@LocationID=10]  
') AS Result   
FROM  Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 请注意上述查询的以下方面：  
  
-   XQuery 序言 (AWMI) 声明中包含命名空间前缀 `(namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";` 。  
  
-   `declare namespace` 关键字定义查询主体随后使用的命名空间前缀。  
  
-   `/AWMI:root/AWMI:Location[@LocationID="10"]` 是查询主体。  
  
## <a name="namespace-declarations"></a>命名空间声明  
 命名空间声明定义前缀并将其与命名空间 URI 相关联，如下面的查询所示。 在查询中， `CatalogDescription` 是一个 **xml** 类型列。  
  
 对此列指定 XQuery 时，查询 prolog 将指定 `declare namespace` 声明以便将前缀 `PD`（即产品说明）与命名空间 URI 相关联。 查询主体中随后将使用此前缀代替命名空间 URI。 产生的 XML 中的节点属于与命名空间 URI 相关联的命名空间。  
  
```  
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 若要提高查询可读性，可以使用 WITH XMLNAMESPACES 声明命名空间，而不是使用 `declare namespace` 在查询 prolog 中声明前缀并进行命名空间绑定。  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
  
SELECT CatalogDescription.query('  
         /PD:ProductDescription/PD:Summary   
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 有关详细信息，请参阅通过 [WITH XMLNAMESPACES 将命名空间添加到查询](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)。  
  
### <a name="default-namespace-declaration"></a>默认命名空间声明  
 如果不使用 `declare namespace` 声明来声明一个命名空间前缀，您可以使用 `declare default element namespace` 声明为元素名称绑定一个默认命名空间。 在这种情况下，并不需要指定任何前缀。  
  
 在下面的示例中，查询主体中的路径表达式没有指定命名空间前缀。 默认情况下，所有元素名称都属于 prolog 中指定的默认命名空间。  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace  "https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
 可以使用 WITH XMLNAMESPACES 来声明默认命名空间：  
  
```  
WITH XMLNAMESPACES (DEFAULT 'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription')  
SELECT CatalogDescription.query('  
        /ProductDescription/Summary   
    ') as Result  
FROM  Production.ProductModel  
WHERE ProductModelID=19   
```  
  
## <a name="see-also"></a>另请参阅  
 [使用 WITH XMLNAMESPACES 将命名空间添加到查询](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)  
  
  
