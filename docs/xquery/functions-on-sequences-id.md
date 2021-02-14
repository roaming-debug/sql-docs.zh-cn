---
title: )  (XQuery 的 id 函数 |Microsoft Docs
description: 了解如何使用 XQuery id 函数以文档顺序返回 XML 实例中的元素序列和提供的 xs： IDREF 值。
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
- fn:id function
- id function
ms.assetid: de99fc60-d0ad-4117-a17d-02bdde6512b4
author: rothja
ms.author: jroth
ms.openlocfilehash: c6ae0aedd4971d6345811e6ee0bfff5afa395c15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344983"
---
# <a name="functions-on-sequences---id"></a>基于序列的函数 - id
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  返回具有 xs： ID 值的元素节点序列，该序列与 *$arg* 中提供的一个或多个 XS： IDREF 值的值相匹配。  
  
## <a name="syntax"></a>语法  
  
```  
  
fn:id($arg as xs:IDREF*) as element()*  
```  
  
## <a name="arguments"></a>参数  
 *$arg*  
 一个或多个 xs:IDREF 值。  
  
## <a name="remarks"></a>备注  
 此函数的结果是 XML 实例中的一组元素（以文档顺序），其中有 xs:ID 值等于候选 xs:IDREF 列表中的一个或多个 xs:IDREF 值。  
  
 如果 xs:IDREF 值不匹配任何元素，则该函数返回空序列。  
  
## <a name="examples"></a>示例  
 本主题提供针对数据库中各种 **xml** 类型列中存储的 xml 实例的 XQuery 示例 [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] 。  
  
### <a name="a-retrieving-elements-based-on-the-idref-attribute-value"></a>A. 基于 IDREF 属性值检索元素  
 下面的示例使用 fn： id `employee` 基于 IDREF manager 属性检索 <> 元素。 在此示例中，manager 属性是一个 IDREF 类型的属性，eid 属性是一个 ID 类型的属性。  
  
 对于特定的管理器属性值， **id ()** 函数将查找 `employee` 其 id 类型属性值与输入 IDREF 值匹配的 <> 元素。 换言之，对于特定的员工， **id ()** 函数返回 employee 经理。  
  
 在该示例中执行下列操作：  
  
-   创建一个 XML 架构集合。  
  
-   使用 XML 架构集合创建类型化的 **xml** 变量。  
  
-   查询检索具有 ID 属性值的元素，该属性值由 <> 元素的 **管理器** IDREF 特性引用 `employee` 。  
  
```  
-- If exists, drop the XML schema collection (SC).  
-- drop xml schema collection SC  
-- go  
  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:e="emp" targetNamespace="emp">  
            <element name="employees" type="e:EmployeesType"/>  
            <complexType name="EmployeesType">  
                 <sequence>  
                      <element name="employee" type="e:EmployeeType" minOccurs="0" maxOccurs="unbounded" />  
                 </sequence>  
            </complexType>    
  
            <complexType name="EmployeeType">  
                        <attribute name="eid" type="ID" />  
                        <attribute name="name" type="string" />  
                        <attribute name="manager" type="IDREF" />  
            </complexType>         
</schema>'  
go  
```  
  
```  
declare @x xml(SC)  
set @x='<e:employees xmlns:e="emp">  
<employee eid="e1" name="Joe" manager="e10" />  
<employee eid="e2" name="Bob" manager="e10" />  
<employee eid="e10" name="Dave" manager="e10" />  
</e:employees>'  
  
select @x.value(' declare namespace e="emp";   
 (fn:id(e:employees/employee[@name="Joe"]/@manager)/@name)[1]', 'varchar(50)')   
Go  
```  
  
 该查询返回值“Dave”。 这表示 Dave 是 Joe 的经理。  
  
### <a name="b-retrieving-elements-based-on-the-orderlist-idrefs-attribute-value"></a>B. 基于 OrderList IDREFS 属性值检索元素  
 在下面的示例中，<> 元素的 OrderList 属性 `Customer` 是一个 IDREFS 类型属性。 它列出特定客户的订单 ID。 对于每个订单 id， `Order` <`Customer`> 提供订单值 <> 元素子级。  
  
 查询表达式 `data(CustOrders:Customers/Customer[1]/@OrderList)[1]` 检索第一个客户的 IDRES 列表中的第一个值。 然后，将此值传递给 **()** 函数的 id。 然后，该函数查找 `Order` 其 "订单 id" 属性值与 **id ()** 函数的输入匹配的 <> 元素。  
  
```  
drop xml schema collection SC  
go  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:Customers="Customers" targetNamespace="Customers">  
            <element name="Customers" type="Customers:CustomersType"/>  
            <complexType name="CustomersType">  
                        <sequence>  
                            <element name="Customer" type="Customers:CustomerType" minOccurs="0" maxOccurs="unbounded" />  
                        </sequence>  
            </complexType>  
             <complexType name="OrderType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="OrderValue" type="integer" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="OrderID" type="ID" />  
            </complexType>  
  
            <complexType name="CustomerType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="spouse" type="string" minOccurs="0" maxOccurs="unbounded"/>  
                                <element name="Order" type="Customers:OrderType" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="CustomerID" type="string" />  
                <attribute name="OrderList" type="IDREFS" />  
            </complexType>  
 </schema>'  
go  
declare @x xml(SC)  
set @x='<CustOrders:Customers xmlns:CustOrders="Customers">  
                <Customer CustomerID="C1" OrderList="OrderA OrderB"  >  
                              <spouse>Jenny</spouse>  
                                <Order OrderID="OrderA"><OrderValue>11</OrderValue></Order>  
                                <Order OrderID="OrderB"><OrderValue>22</OrderValue></Order>  
  
                </Customer>  
                <Customer CustomerID="C2" OrderList="OrderC OrderD" >  
                                <spouse>John</spouse>  
                                <Order OrderID="OrderC"><OrderValue>33</OrderValue></Order>  
                                <Order OrderID="OrderD"><OrderValue>44</OrderValue></Order>  
  
                        </Customer>  
                <Customer CustomerID="C3"  OrderList="OrderE OrderF" >  
                                <spouse>Jane</spouse>  
                                <Order OrderID="OrderE"><OrderValue>55</OrderValue></Order>  
                                <Order OrderID="OrderF"><OrderValue>55</OrderValue></Order>  
                </Customer>  
                <Customer CustomerID="C4"  OrderList="OrderG"  >  
                                <spouse>Tim</spouse>  
                                <Order OrderID="OrderG"><OrderValue>66</OrderValue></Order>  
                        </Customer>  
                <Customer CustomerID="C5"  >  
                </Customer>  
                <Customer CustomerID="C6" >  
                </Customer>  
                <Customer CustomerID="C7"  >  
                </Customer>  
</CustOrders:Customers>'  
select @x.query('declare namespace CustOrders="Customers";  
  id(data(CustOrders:Customers/Customer[1]/@OrderList)[1])')  
  
-- result  
<Order OrderID="OrderA">  
  <OrderValue>11</OrderValue>  
</Order>  
```  
  
### <a name="implementation-limitations"></a>实现限制  
 限制如下：  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不支持 **id ()** 的双参数版本。  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 要求 id 的参数类型 **()** 是 XS： IDREF * 的子类型。  
  
## <a name="see-also"></a>另请参阅  
 [序列上的函数](./xquery-functions-against-the-xml-data-type.md)  
  
