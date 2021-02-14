---
title: 在路径表达式中使用缩写语法 |Microsoft Docs
description: 了解如何在 XQuery 路径表达式中使用缩写语法。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- axis step [XQuery]
- abbreviated syntax [XQuery]
ms.assetid: f83c2e41-5722-47c3-b5b8-bf0f8cbe05d3
author: rothja
ms.author: jroth
ms.openlocfilehash: 959d5498011984fc1f400567517d3b5b07727fcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339936"
---
# <a name="path-expressions---using-abbreviated-syntax"></a>路径表达式 - 使用缩写语法
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  [了解 XQuery 中的路径表达式](../xquery/path-expressions-xquery.md)的所有示例均使用不缩写的路径表达式语法。 路径表达式中用于轴步骤的未缩写语法包括轴名称和节点测试（用双冒号分隔），后跟零或更多个步骤限定符。  
  
 例如：  
  
```  
child::ProductDescription[attribute::ProductModelID=19]  
```  
  
 XQuery 支持在路径表达式中使用下列缩写：  
  
-   **子** 轴为默认轴。 因此，可以在表达式中的步骤中省略 **child：：** 轴。 例如，`/child::ProductDescription/child::Summary` 可以写为 `/ProductDescription/Summary`。  
  
-   **特性** 轴可以缩写为 @ 。 例如，`/child::ProductDescription[attribute::ProductModelID=10]` 可以写为 `/ProudctDescription[@ProductModelID=10]`。  
  
-   **/Descendant-or-self：： node () /** 可以缩写为//。 例如，`/descendant-or-self::node()/child::act:telephoneNumber` 可以写为 `//act:telephoneNumber`。  
  
     上一个查询检索了存储在 Contact 表的 AdditionalContactInfo 列中的所有电话号码。 AdditionalContactInfo 的架构的定义方式是 \<telephoneNumber> 元素可以出现在文档中的任何位置。 因此，若要检索所有电话号码，必须搜索文档中的每一个节点。 从文档的根节点开始搜索，接下来搜索所有后代节点。  
  
     以下查询将检索某一特定客户的联系人的所有电话号码：  
  
    ```  
    SELECT AdditionalContactInfo.query('             
                declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";             
                declare namespace crm="https://schemas.adventure-works.com/Contact/Record";             
                declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";             
                /descendant-or-self::node()/child::act:telephoneNumber             
                ') as result             
    FROM Person.Contact             
    WHERE ContactID=1             
    ```  
  
     如果将路径表达式替换为缩写语法 `//act:telephoneNumber`，会收到相同的结果。  
  
-   步骤中的 **self：： node ()** 可以缩写为单点 ( ) 。 但是，点与 **self：： node ()** 不是等效的，也不能互换。  
  
     例如，在以下查询中，使用点表示的是一个值而不是节点：  
  
    ```  
    ("abc", "cde")[. > "b"]  
    ```  
  
-   步骤中的 **parent：： node ()** 可以缩写为双点 (。) 。  
  
  
