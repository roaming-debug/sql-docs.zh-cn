---
title: )  (XQuery 的条件表达式 |Microsoft Docs
description: 了解 XQuery 支持的条件表达式。
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
- XQuery, conditional expressions
- expressions [XQuery], conditional
- effective Boolean value [XQuery]
- if-then-else statement [XQuery]
- conditional expressions [XQuery]
- EBV
ms.assetid: b280dd96-c80f-4c51-bc06-a88d42174acb
author: rothja
ms.author: jroth
ms.openlocfilehash: 5fdc8703482f570d0ecb749b471af170b62770ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349372"
---
# <a name="conditional-expressions-xquery"></a>条件表达式 (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  XQuery 支持以下条件 **if-else** 语句：  
  
```  
if (<expression1>)  
then  
  <expression2>  
else  
  <expression3>  
```  
  
 根据 `expression1` 的有效布尔值，选择计算 `expression2` 或 `expression3`。 例如：  
  
-   如果测试表达式 `expression1` 结果为空序列，则结果为 False。  
  
-   如果测试表达式 `expression1` 结果为简单布尔值，则此值即为该表达式的结果。  
  
-   如果测试表达式 `expression1` 产生一个或多个节点的序列，则该表达式的结果为 True。  
  
-   否则，将引发静态错误。  
  
 另请注意以下几点：  
  
-   测试表达式必须用括号括起来。  
  
-   **Else** 表达式是必需的。 如果不需要该表达式，可以返回“( )”，如本主题中的示例所示。  
  
 例如，下面的查询是针对 **xml** 类型变量指定的。 **If** 条件 @v 使用 [sql： variable () 函数](../xquery/xquery-extension-functions-sql-variable.md)扩展函数测试 XQuery 表达式内 () 的 sql 变量的值。 如果变量值为 "FirstName"，它将返回 <`FirstName`> 元素。 否则，它将返回 <`LastName`> 元素。  
  
```  
declare @x xml  
declare @v varchar(20)  
set @v='FirstName'  
set @x='  
<ROOT rootID="2">  
  <FirstName>fname</FirstName>  
  <LastName>lname</LastName>  
</ROOT>'  
SELECT @x.query('  
if ( sql:variable("@v")="FirstName" ) then  
  /ROOT/FirstName  
 else  
   /ROOT/LastName  
')  
```  
  
 结果如下：  
  
```  
<FirstName>fname</FirstName>  
```  
  
 以下查询从特定产品样式的产品目录说明中检索前两个功能说明。 如果文档中有多个功能，则会添加一个 `there-is-more` 包含空内容的 <> 元素。  
  
```  
SELECT CatalogDescription.query('  
     declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     <Product>   
          { /p1:ProductDescription/@ProductModelID }  
          { /p1:ProductDescription/@ProductModelName }   
          {  
            for $f in /p1:ProductDescription/p1:Features/*[position()\<=2]  
            return  
            $f   
          }  
          {  
            if (count(/p1:ProductDescription/p1:Features/*) > 2)  
            then \<there-is-more/>  
            else ()  
          }   
     </Product>          
') as x  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 在前面的查询中， **if** 表达式中的条件将检查 <> 中是否存在两个以上的子元素 `Features` 。 如果有，则在结果中返回 `\<there-is-more/>` 元素。  
  
 结果如下：  
  
```  
<Product ProductModelID="19" ProductModelName="Mountain 100">  
  \<p1:Warranty xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p1:WarrantyPeriod>3 years\</p1:WarrantyPeriod>  
    \<p1:Description>parts and labor\</p1:Description>  
  \</p1:Warranty>  
  \<p2:Maintenance xmlns:p2="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain">  
    \<p2:NoOfYears>10 years\</p2:NoOfYears>  
    \<p2:Description>maintenance contract available through your dealer or any AdventureWorks retail store.\</p2:Description>  
  \</p2:Maintenance>  
  \<there-is-more />  
</Product>  
```  
  
 在下面的查询中， `Location` 如果工作中心位置未指定设置时间，则返回具有 LocationID 属性的 <> 元素。  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        for $WC in //AWMI:root/AWMI:Location  
        return  
        if ( $WC[not(@SetupHours)] )  
        then  
          <WorkCenterLocation>  
             { $WC/@LocationID }   
          </WorkCenterLocation>  
         else  
           ()  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 结果如下：  
  
```  
<WorkCenterLocation LocationID="30" />  
<WorkCenterLocation LocationID="45" />  
<WorkCenterLocation LocationID="60" />  
```  
  
 此查询可以不带 **if** 子句编写，如以下示例中所示：  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $WC in //AWMI:root/AWMI:Location[not(@SetupHours)]   
        return  
          <Location>  
             { $WC/@LocationID }   
          </Location>  
') as Result  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
## <a name="see-also"></a>另请参阅  
 [XQuery 表达式](../xquery/xquery-expressions.md)  
  
  
