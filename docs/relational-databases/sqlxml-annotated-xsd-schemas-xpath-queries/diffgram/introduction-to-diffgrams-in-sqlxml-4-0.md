---
title: SQLXML 4.0 中的 DiffGrams 简介
description: 了解 SQLXML 4.0 中 Diffgram 的格式、批注和处理逻辑。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotations [SQLXML]
- DiffGrams [SQLXML], about DiffGrams
ms.assetid: 1902d67f-baf3-46e6-a36c-b24b5ba6f8ea
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66a58dfb19cf8f53f775bac663d5ba3e6147711b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414916"
---
# <a name="introduction-to-diffgrams-in-sqlxml-40"></a>SQLXML 4.0 中的 DiffGrams 简介
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  本主题提供对 DiffGrams 的简要介绍。  
  
## <a name="diffgram-format"></a>DiffGram 格式  
 这是常规 DiffGram 格式：  
  
```  
<?xml version="1.0"?>  
<diffgr:diffgram   
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"  
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1"  
         xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
   <DataInstance>  
      ...  
   </DataInstance>  
   [<diffgr:before>  
        ...  
   </diffgr:before>]  
  
   [<diffgr:errors>  
        ...  
   </diffgr:errors>]  
</diffgr:diffgram>  
```  
  
 DiffGram 格式由这些块组成：  
  
 **\<DataInstance>**  
 此元素的名称 **DataInstance** 用于此文档中的说明。 例如，如果 DiffGram 是从 .NET Framework 中的数据集生成的，则该数据集的 **name** 属性的值将用作此元素的名称。 此块包含更改后的所有相关数据，可能包括尚未修改的数据。 DiffGram 处理逻辑将忽略未指定 **diffgr： hasChanges** 特性的块中的元素。  
  
 **\<diffgr:before>**  
 此可选块包含必须更新或删除的原始记录实例（元素）。 要修改的所有数据库表 (DiffGram) 更新或删除这些数据库表都必须显示为块中的顶级元素 **\<before>** 。  
  
 **\<diffgr:errors>**  
 DiffGram 处理逻辑将忽略此可选块。  
  
## <a name="diffgram-annotations"></a>DiffGram 批注  
 这些批注在 DiffGram 命名空间 **"urn：架构-microsoft-com： xml-DiffGram-01"** 中定义：  
  
 **id**  
 此特性用于将和块中的元素配对 **\<before>** **\<DataInstance>** 。  
  
 **hasChanges**  
 对于插入操作或更新操作，DiffGram 必须使用 **插入** 或 **修改** 的值来指定此属性。 如果此属性不存在，则处理逻辑将忽略中的相应元素， **\<DataInstance>** 并且不执行任何更新。 有关工作示例，请参阅 [SQLXML 4.0&#41;的 DiffGram 示例 &#40;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md)。  
  
 **parentID**  
 该属性在 DiffGram 中用于指定元素之间的父子关系。 此属性仅出现在 \<before> 块中。 应用更新时，SQLXML 将使用它。 在确定 DiffGram 中元素的处理顺序时，将使用该父子关系。  
  
## <a name="understanding-the-diffgram-processing-logic"></a>了解 DiffGram 处理逻辑  
 DiffGram 处理逻辑使用某些规则来确定操作是否是插入、更新或删除操作。 下表描述了这些规则。  
  
|Operation|说明|  
|---------------|-----------------|  
|插入|当元素出现在 **\<DataInstance>** 块中但不在相应的块中时，DiffGram 指示插入操作 **\<before>** ，并且 **diffgr： hasChanges** 特性是 (**diffgr： hasChanges =** 指定元素上插入) 。 在这种情况下，DiffGram 将在块中指定的记录实例插入 **\<DataInstance>** 到数据库中。<br /><br /> 如果未指定 **diffgr： hasChanges** 属性，处理逻辑将忽略元素，并且不执行任何插入操作。 有关工作示例，请参阅 [SQLXML 4.0&#41;的 DiffGram 示例 &#40;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md)。|  
|更新|如果在块中有一个对应的 (元素，而该元素在块中有一个对应的元素，则 DiffGram 指示一个更新操作 \<before> **\<DataInstance>** ，这两个元素的 **diffgr： id** 属性具有相同的值) 并且使用在块中的元素上 **修改** 的值指定 **diffgr： hasChanges** 特性 **\<DataInstance>** 。<br /><br /> 如果未在块中的元素上指定 **diffgr： hasChanges** 属性 **\<DataInstance>** ，处理逻辑将返回错误。 有关工作示例，请参阅 [SQLXML 4.0&#41;的 DiffGram 示例 &#40;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md)。<br /><br /> 如果在块中指定了 **diffgr： parentid** **\<before>** ，则由 **parentID** 指定的元素的父子关系用于确定更新记录的顺序。|  
|删除|DiffGram 指示当元素出现在块中但不出现在 **\<before>** 相应的块中时的删除操作 **\<DataInstance>** 。 在这种情况下，DiffGram 将从数据库中删除在块中指定的记录实例 **\<before>** 。 有关工作示例，请参阅 [SQLXML 4.0&#41;的 DiffGram 示例 &#40;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/diffgram/diffgram-examples-sqlxml-4-0.md)。<br /><br /> 如果在块中指定了 **diffgr： parentid** **\<before>** ，则由 **parentID** 指定的元素的父子关系用于确定删除记录的顺序。|  
  
> [!NOTE]  
>  参数无法传递到 DiffGrams。  
  
  
