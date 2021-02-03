---
description: CREATE XML SCHEMA COLLECTION (Transact-SQL)
title: CREATE XML SCHEMA COLLECTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE XML SCHEMA COLLECTION
- CREATE_XML_SCHEMA_COLLECTION_TSQL
- CREATE XML SCHEMA
- CREATE_XML_SCHEMA_TSQL
- COLLECTION
- COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML SCHEMA COLLECTION statement
- importing schema components
- schema collections [SQL Server], creating
- multiple schema namespaces
- XML schema collections [SQL Server], creating
ms.assetid: 350684e8-b3f6-4b58-9dbc-0f05cc776ebb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bac56a4919ddfcca7a56e7f3a39aba75c82c1e10
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177821"
---
# <a name="create-xml-schema-collection-transact-sql"></a>CREATE XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  将架构组件导入数据库中。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE XML SCHEMA COLLECTION [ <relational_schema>. ]sql_identifier AS Expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 relational_schema  
 标识关系架构的名称。 如果不指定，则假定为默认关系架构。  
  
 sql_identifier  
 是 XML 架构集合的 SQL 标识符。  
  
 *表达式*  
 字符串常量或标量变量。 为 varchar、varbinary、nvarchar 或 xml 类型。  
  
## <a name="remarks"></a>注解  
 通过 [ALTER XML SCHEMA COLLECTION](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)，还可以向集合中添加新命名空间或向集合的现有命名空间中添加新组件。  
  
 若要删除集合，请使用 [DROP XML SCHEMA COLLECTION (Transact SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 若要创建 XML SCHEMA COLLECTION，需要至少拥有下列权限集之一：  
  
-   对服务器具有 CONTROL 权限  
  
-   对服务器具有 ALTER ANY DATABASE 权限  
  
-   对数据库具有 ALTER 权限  
  
-   数据库中的 CONTROL 权限  
  
-   数据库中的 ALTER ANY SCHEMA 权限和 CREATE XML SCHEMA COLLECTION 权限  
  
-   对关系架构具有 ALTER 或 CONTROL 权限以及数据库中的 CREATE XML SCHEMA COLLECTION 权限  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-xml-schema-collection-in-the-database"></a>A. 在数据库中创建 XML 架构集合  
 下面的示例将创建 XML 架构集合 `ManuInstructionsSchemaCollection`。 该集合只有一个架构命名空间。  
  
```sql  
-- Create a sample database in which to load the XML schema collection.  
CREATE DATABASE SampleDB;  
GO  
USE SampleDB;  
GO  
CREATE XML SCHEMA COLLECTION ManuInstructionsSchemaCollection AS  
N'<?xml version="1.0" encoding="UTF-16"?>  
<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   xmlns          ="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"   
   elementFormDefault="qualified"   
   attributeFormDefault="unqualified"  
   xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
  
    <xsd:complexType name="StepType" mixed="true" >  
        <xsd:choice  minOccurs="0" maxOccurs="unbounded" >   
            <xsd:element name="tool" type="xsd:string" />  
            <xsd:element name="material" type="xsd:string" />  
            <xsd:element name="blueprint" type="xsd:string" />  
            <xsd:element name="specs" type="xsd:string" />  
            <xsd:element name="diag" type="xsd:string" />  
        </xsd:choice>   
    </xsd:complexType>  
  
    <xsd:element  name="root">  
        <xsd:complexType mixed="true">  
            <xsd:sequence>  
                <xsd:element name="Location" minOccurs="1" maxOccurs="unbounded">  
                    <xsd:complexType mixed="true">  
                        <xsd:sequence>  
                            <xsd:element name="step" type="StepType" minOccurs="1" maxOccurs="unbounded" />  
                        </xsd:sequence>  
                        <xsd:attribute name="LocationID" type="xsd:integer" use="required"/>  
                        <xsd:attribute name="SetupHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="MachineHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LaborHours" type="xsd:decimal" use="optional"/>  
                        <xsd:attribute name="LotSize" type="xsd:decimal" use="optional"/>  
                    </xsd:complexType>  
                </xsd:element>  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>' ;  
GO  
-- Verify - list of collections in the database.  
SELECT *  
FROM sys.xml_schema_collections;  
-- Verify - list of namespaces in the database.  
SELECT name  
FROM sys.xml_schema_namespaces;  
  
-- Use it. Create a typed xml variable. Note collection name specified.  
DECLARE @x xml (ManuInstructionsSchemaCollection);  
GO  
--Or create a typed xml column.  
CREATE TABLE T (  
        i int primary key,   
        x xml (ManuInstructionsSchemaCollection));  
GO  
-- Clean up  
DROP TABLE T;  
GO  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
Go  
USE master;  
GO  
DROP DATABASE SampleDB;  
```  
  
 另外，您还可以将架构集合分配给一个变量，并按如下方式在 `CREATE XML SCHEMA COLLECTION` 语句中指定该变量：  
  
```  
DECLARE @MySchemaCollection nvarchar(max)  
Set @MySchemaCollection  = N' copy the schema collection here'  
CREATE XML SCHEMA COLLECTION MyCollection AS @MySchemaCollection   
```  
  
 示例中的变量为 `nvarchar(max)` 类型。 该变量也可以为 xml 数据类型，在这种情况下，它将隐式转换为字符串。  
  
 有关详细信息，请参阅 [查看存储 XML 架构集合](../../relational-databases/xml/view-a-stored-xml-schema-collection.md)。  
  
 可以在 xml 类型列中存储架构集合。 在这种情况下，若要创建 XML 架构集合，请执行以下操作：  
  
1.  使用 SELECT 语句从列中检索该架构集合，然后将它分配给一个类型为 xml 或 varchar 的变量 。  
  
2.  在 CREATE XML SCHEMA COLLECTION 语句中指定变量名称。  
  
 CREATE XML SCHEMA COLLECTION 只存储 SQL Server 了解的架构组件；XML 架构中的所有内容都不会存储在数据库中。 因此，如果您希望 XML 架构集合保持提供它时的原样，建议您在数据库列或计算机上的其他文件夹中保存您的 XML 架构。  
  
### <a name="b-specifying-multiple-schema-namespaces-in-a-schema-collection"></a>B. 在架构集合中指定多个架构命名空间  
 在创建 XML 架构集合时，可以指定多个 XML 架构。 例如：  
  
```sql  
CREATE XML SCHEMA COLLECTION MyCollection AS N'  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->    
</xsd:schema>  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
<!-- Contents of schema here -->  
</xsd:schema>';  
```  
  
 下面的示例将创建包含两个 XML 架构命名空间的 XML 架构集合 `ProductDescriptionSchemaCollection`。  
  
```sql  
CREATE XML SCHEMA COLLECTION ProductDescriptionSchemaCollection AS   
'<xsd:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"  
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain"   
    elementFormDefault="qualified"   
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" >  
    <xsd:element name="Warranty"  >  
        <xsd:complexType>  
            <xsd:sequence>  
                <xsd:element name="WarrantyPeriod" type="xsd:string"  />  
                <xsd:element name="Description" type="xsd:string"  />  
            </xsd:sequence>  
        </xsd:complexType>  
    </xsd:element>  
</xsd:schema>  
 <xs:schema targetNamespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription"   
    elementFormDefault="qualified"   
    xmlns:mstns="https://tempuri.org/XMLSchema.xsd"   
    xmlns:xs="http://www.w3.org/2001/XMLSchema"  
    xmlns:wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" >  
    <xs:import   
namespace="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain" />  
    <xs:element name="ProductDescription" type="ProductDescription" />  
        <xs:complexType name="ProductDescription">  
            <xs:sequence>  
                <xs:element name="Summary" type="Summary" minOccurs="0" />  
            </xs:sequence>  
            <xs:attribute name="ProductModelID" type="xs:string" />  
            <xs:attribute name="ProductModelName" type="xs:string" />  
        </xs:complexType>  
        <xs:complexType name="Summary" mixed="true" >  
            <xs:sequence>  
                <xs:any processContents="skip" namespace="http://www.w3.org/1999/xhtml" minOccurs="0" maxOccurs="unbounded" />  
            </xs:sequence>  
        </xs:complexType>  
</xs:schema>'  
;  
GO -- Clean up  
DROP XML SCHEMA COLLECTION ProductDescriptionSchemaCollection;  
GO  
```  
  
### <a name="c-importing-a-schema-that-does-not-specify-a-target-namespace"></a>C. 导入未指定目标命名空间的架构  
 如果向集合中导入未包含 targetNamespace 属性的架构，该架构的组件将与空字符串目标命名空间相关联，如下面的示例所示。 注意，如果不关联在集合中导入的一个或多个架构，将导致多个架构组件（可能是无关的）与默认空字符串命名空间关联。  
  
```sql  
-- Create a collection that contains a schema with no target namespace.  
CREATE XML SCHEMA COLLECTION MySampleCollection AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  xmlns:ns="http://ns">  
<element name="e" type="dateTime"/>  
</schema>';  
go  
-- Query will return the names of all the collections that   
--contain a schema with no target namespace.  
SELECT sys.xml_schema_collections.name   
FROM   sys.xml_schema_collections   
JOIN   sys.xml_schema_namespaces   
ON     sys.xml_schema_collections.xml_collection_id =   
       sys.xml_schema_namespaces.xml_collection_id   
WHERE  sys.xml_schema_namespaces.name='';  
```  
  
### <a name="d-using-an-xml-schema-collection-and-batches"></a>D. 使用 XML 架构集合和批  
 无法在创建架构集合的相同批中引用该架构集合。 如果试图在创建集合的相同批中引用该集合，将得到说明该集合不存在的错误消息。 以下示例有效；但如果删除 `GO`，并因此试图引用 XML 架构集合以便在相同批中键入 `xml` 变量，那么，它将返回错误。  
  
```sql  
CREATE XML SCHEMA COLLECTION mySC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="string"/>  
</schema>  
';  
GO  
CREATE TABLE T (Col1 xml (mySC));  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER XML SCHEMA COLLECTION (Transact SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [DROP XML SCHEMA COLLECTION (Transact SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [DROP XML SCHEMA COLLECTION (Transact SQL)](../../t-sql/statements/drop-xml-schema-collection-transact-sql.md)   
 [在服务器上使用 XML 架构集合的要求和限制](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  
