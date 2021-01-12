---
description: CREATE MESSAGE TYPE (Transact-SQL)
title: CREATE MESSAGE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_MESSAGE_TSQL
- MESSAGE_TSQL
- MESSAGE
- CREATE MESSAGE
- CREATE_MESSAGE_TYPE_TSQL
- MESSAGE TYPE
- MESSAGE_TYPE_TSQL
- CREATE MESSAGE TYPE
dev_langs:
- TSQL
helpviewer_keywords:
- XML [Service Broker]
- validation [Service Broker]
- message types [Service Broker], creating
- empty messages [SQL Server]
- binary [SQL Server], message types
- CREATE MESSAGE TYPE statement
ms.assetid: 98fe0fff-1a2e-4ca2-b37f-83a06fdf098e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b32fe2417a7a9d9d290b72cf3de2dfc325fc0eaf
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093439"
---
# <a name="create-message-type-transact-sql"></a>CREATE MESSAGE TYPE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建新消息类型。 消息类型定义消息的名称，以及 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对具有该名称的消息执行的验证。 会话双方必须定义相同的消息类型。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE MESSAGE TYPE message_type_name  
    [ AUTHORIZATION owner_name ]  
    [ VALIDATION = {  NONE  
                    | EMPTY   
                    | WELL_FORMED_XML  
                    | VALID_XML WITH SCHEMA COLLECTION schema_collection_name  
                   } ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *message_type_name*  
 要创建的消息类型的名称。 在当前数据库中创建一条新消息，并归 AUTHORIZATION 子句定义的主体数据库所有。 不能指定服务器、数据库和架构名称。  message_type_name 最多可具有 128 个字符。  
  
 AUTHORIZATION owner_name   
 将消息类型所有者设置为指定数据库用户或角色。 如果当前用户为 dbo 或 sa，则 owner_name 可以为任意有效用户或角色的名称。 否则，owner_name 必须是当前用户的名称，或者是当前用户对其有 IMPERSONATE 权限的用户的名称，或者是当前用户所属的角色的名称。 如果省略此子句，则消息类型属于当前用户。  
  
 VALIDATION  
 指定 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 对此类型消息的消息正文的验证方式。 如果未指定此子句，则验证默认为 NONE。  
  
 无  
 指定不执行验证。 消息正文可以包含数据，也可以为 NULL。  
  
 EMPTY  
 指定消息正文必须为 NULL。  
  
 WELL_FORMED_XML  
 指定消息正文必须包含格式正确的 XML。  
  
 VALID_XML WITH SCHEMA COLLECTION schema_collection_name  
 指定消息正文必须包含符合指定架构集合中的某一架构的 XML。schema_collection_name 必须是现有 XML 架构集合的名称。  
  
## <a name="remarks"></a>注解  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 验证传入消息。 如果消息包含的消息正文与指定的验证类型不符，则 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 将放弃此无效消息，并向发送此消息的服务返回一条错误消息。  
  
 会话双方必须定义相同的消息类型名称。 为便于排除故障，尽管 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 不要求会话双方使用相同的验证，但通常会话双方还是会为消息类型指定相同的验证。  
  
 消息类型不能是临时对象。 允许使用以 # 开头的消息类型名称，但它们是永久对象。  
  
## <a name="permissions"></a>权限  
 默认情况下，db_ddladmin 或 db_owner 固定数据库角色和 sysadmin 固定服务器角色的成员拥有创建消息类型的权限。  
  
 默认情况下，消息类型的所有者、db_owner 固定数据库角色和 sysadmin 固定服务器角色的成员拥有消息类型的 REFERENCES 权限。  
  
 如果 CREATE MESSAGE TYPE 语句指定了架构集合，则执行该语句的用户必须对指定的架构集合拥有 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-message-type-containing-well-formed-xml"></a>A. 创建包含格式正确的 XML 的消息类型  
 以下示例创建一个包含格式正确的 XML 的新消息类型。  
  
```sql  
CREATE MESSAGE TYPE  
  [//Adventure-Works.com/Expenses/SubmitExpense]  
  VALIDATION = WELL_FORMED_XML ;     
```  
  
### <a name="b-creating-a-message-type-containing-typed-xml"></a>B. 创建包含类型化 XML 的消息类型  
 以下示例为使用 XML 编码的费用报表创建一个消息类型。 该示例将创建一个 XML 架构集合，用于保存一个简单费用报表的架构。 然后，该示例将创建一个新消息类型，用于根据该架构来验证消息。  
  
```sql  
CREATE XML SCHEMA COLLECTION ExpenseReportSchema AS  
N'<?xml version="1.0" encoding="UTF-16" ?>  
  <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
     targetNamespace="https://Adventure-Works.com/schemas/expenseReport"  
     xmlns:expense="https://Adventure-Works.com/schemas/expenseReport"  
     elementFormDefault="qualified"  
   >   
    <xsd:complexType name="expenseReportType">  
       <xsd:sequence>  
         <xsd:element name="EmployeeName" type="xsd:string"/>  
         <xsd:element name="EmployeeID" type="xsd:string"/>  
         <xsd:element name="ItemDetail"  
           type="expense:ItemDetailType" maxOccurs="unbounded"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:complexType name="ItemDetailType">  
      <xsd:sequence>  
        <xsd:element name="Date" type="xsd:date"/>  
        <xsd:element name="CostCenter" type="xsd:string"/>  
        <xsd:element name="Total" type="xsd:decimal"/>  
        <xsd:element name="Currency" type="xsd:string"/>  
        <xsd:element name="Description" type="xsd:string"/>  
      </xsd:sequence>  
    </xsd:complexType>  
  
    <xsd:element name="ExpenseReport" type="expense:expenseReportType"/>  
  
  </xsd:schema>' ;  
  
  CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = VALID_XML WITH SCHEMA COLLECTION ExpenseReportSchema ;  
```  
  
### <a name="c-creating-a-message-type-for-an-empty-message"></a>C. 创建一个用于空消息的消息类型  
 以下示例创建一个使用空编码的新消息类型。  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/SubmitExpense]  
    VALIDATION = EMPTY ;  
```  
  
### <a name="d-creating-a-message-type-containing-binary-data"></a>D. 创建一个包含二进制数据的消息类型  
 以下示例创建一个用于保存二进制数据的新消息类型。 由于该消息将包含非 XML 的数据，因此消息类型将验证类型指定为 `NONE`。 请注意，在这种情况下，接收该类型消息的应用程序必须验证消息是否包含数据，以及数据是否为期望的类型。  
  
```sql  
CREATE MESSAGE TYPE  
    [//Adventure-Works.com/Expenses/ReceiptImage]  
    VALIDATION = NONE ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/alter-message-type-transact-sql.md)   
 [DROP MESSAGE TYPE (Transact-SQL)](../../t-sql/statements/drop-message-type-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
