---
title: 涉及层次结构的 Xquery |Microsoft Docs
description: 查看涉及层次结构的 Xquery 的示例。
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
- hierarchies [XQuery]
- XQuery, hierarchies
ms.assetid: 6953d8b7-bad8-4b64-bf7b-12fa4f10f65c
author: rothja
ms.author: jroth
ms.openlocfilehash: fdebb93459b9519eb6495054908208de610e8222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352796"
---
# <a name="xqueries-involving-hierarchy"></a>涉及层次结构的 XQuery
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  **AdventureWorks** 数据库中的大多数 **xml** 类型列是半结构化文档。 因此，每行中存储的文档可能看起来不同。 本主题中的查询示例说明如何从这些不同的文档提取信息。  
  
## <a name="examples"></a>示例  
  
### <a name="a-from-the-manufacturing-instructions-documents-retrieve-work-center-locations-together-with-the-first-manufacturing-step-at-those-locations"></a>A. 从生产说明文档检索生产车间以及这些生产车间的第一个生产步骤  
 对于产品型号7，该查询将构造包含 <> 元素的 XML，其中包含 `ManuInstr` **ProductModelID** 和 **ProductModelName** 属性以及一个或多个 <`Location`> 子元素。  
  
 每个 <`Location`> 元素都有其自己的一组属性和一个 <`step`> 子元素。 此 <`step`> 子元素是工作中心位置的第一个生产步骤。  
  
```sql
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   \<ManuInstr  ProdModelID = "{sql:column("Production.ProductModel.ProductModelID") }"   
                ProductModelName = "{ sql:column("Production.ProductModel.Name") }" >  
            {   
              for $wc in //AWMI:root/AWMI:Location  
              return  
                <Location>  
                 {$wc/@* }  
                 <step1> { string( ($wc//AWMI:step)[1] ) } </step1>  
                </Location>  
            }  
          </ManuInstr>  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 请注意上述查询的以下方面：  
  
-   [XQuery Prolog](../xquery/modules-and-prologs-xquery-prolog.md)中的 **namespace** 关键字定义命名空间前缀。 随后，将在查询正文中使用此前缀。  
  
-   上下文切换标记 {) 和 (} 用于将查询从 XML 构造切换到查询计算。  
  
-   **Sql： column ()** 用于在正在构造的 XML 中包含关系值。  
  
-   在构造 <`Location`> 元素中，$wc/@ * 检索所有工作中心位置属性。  
  
-   **字符串 ()** 函数从 <> 元素返回字符串值 `step` 。  
  
 下面是部分结果：  
  
```xml
<ManuInstr ProdModelID="7" ProductModelName="HL Touring Frame">  
   <Location LocationID="10" SetupHours="0.5"   
            MachineHours="3" LaborHours="2.5" LotSize="100">  
     <step1>Insert aluminum sheet MS-2341 into the T-85A   
             framing tool.</step1>  
   </Location>  
   <Location LocationID="20" SetupHours="0.15"   
            MachineHours="2" LaborHours="1.75" LotSize="1">  
      <step1>Assemble all frame components following   
             blueprint 1299.</step1>  
   </Location>  
...  
</ManuInstr>   
```  
  
### <a name="b-find-all-telephone-numbers-in-the-additionalcontactinfo-column"></a>B. 在 AdditionalContactInfo 列中查找所有电话号码  
 下面的查询通过搜索整个层次结构中 <> 元素来检索特定客户联系人的其他电话号码 `telephoneNumber` 。 由于 <`telephoneNumber`> 元素可以出现在层次结构中的任何位置，因此，该查询将使用搜索中 (//) 的后代和 self 运算符。  
  
```sql
SELECT AdditionalContactInfo.query('  
 declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";  
 declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";  
for $ph in /ci:AdditionalContactInfo//act:telephoneNumber  
   return  
      $ph/act:number  
') as x  
FROM  Person.Contact  
WHERE ContactID = 1  
```  
  
 结果如下：  
  
```xml
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  111-111-1111  
\</act:number>  
\<act:number   
  xmlns:act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes">  
  112-111-1111  
\</act:number>  
```  
  
 若要仅检索顶级电话号码（特别是 `telephoneNumber` <> 的 <> 子元素 `AdditionalContactInfo` ），则查询中的 FOR 表达式将更改为  
  
 `for $ph in /ci:AdditionalContactInfo/act:telephoneNumber`.  
  
## <a name="see-also"></a>请参阅  
 [XQuery 基础知识](../xquery/xquery-basics.md)   
 [XML 构造 &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)   
 [XML 数据 (SQL Server)](../relational-databases/xml/xml-data-sql-server.md)  
  
  
