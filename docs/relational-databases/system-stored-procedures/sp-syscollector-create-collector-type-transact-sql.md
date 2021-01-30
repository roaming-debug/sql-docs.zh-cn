---
description: sp_syscollector_create_collector_type (Transact-SQL)
title: sp_syscollector_create_collector_type (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collector_type
- sp_syscollector_create_collector_type_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collector_type
- data collector [SQL Server], stored procedures
ms.assetid: 568e9119-b9b0-4284-9cef-3878c691de5f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5431d45716aa0c1c685f303c3ee4b5d3cec67a13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200513"
---
# <a name="sp_syscollector_create_collector_type-transact-sql"></a>sp_syscollector_create_collector_type (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为数据收集器创建收集器类型。 收集器类型是围绕包的逻辑包装 [!INCLUDE[ssIS](../../includes/ssis-md.md)] ，它们提供用于收集数据并将数据上载到管理数据仓库的实际机制。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_create_collector_type   
    [ [@collector_type_uid = ] 'collector_type_uid' OUTPUT ]  
    , [ @name = ] 'name'  
    , [ [ @parameter_schema = ] 'parameter_schema' ]  
    , [ [ @parameter_formatter = ] 'parameter_formatter' ]  
    , [ @collection_package_id = ] 'collection_package_id'  
    , [ @upload_package_id = ] 'upload_package_id'  
```  
  
## <a name="arguments"></a>参数  
 [ @collector_type_uid =] "*collector_type_uid*"  
 收集器类型的 GUID。 *collector_type_uid* 是 **uniqueidentifier** ，如果为 NULL，则它将自动创建并作为输出返回。  
  
 [ @name =] "*name*"  
 收集器类型的名称。 *名称* 为 **sysname** ，必须指定。  
  
 [ @parameter_schema =] "*parameter_schema*"  
 此收集器类型的 XML 架构。 *parameter_schema* 为 **xml** ，默认值为 NULL。  
  
 [ @parameter_formatter =] "*parameter_formatter*"  
 是用于转换 XML 以便在收集组属性页中使用的模板。 *parameter_formatter* 为 **xml** ，默认值为 NULL。  
  
 [ @collection_package_id =] *collection_package_id*  
 指向收集组使用的 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 收集包的本地唯一标识符。 *collection_package_id* **uniqueidentifer** ，并且是必需的。  
  
 [ @upload_package_id =] *upload_package_id*  
 指向收集组使用的 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 上载包的本地唯一标识符。 *upload_package_id* 是 **uniqueidentifier** ，且是必需的。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="permissions"></a>权限  
 需要具有 dc_admin（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。  
  
## <a name="example"></a>示例  
 这会创建一般 T-SQL 查询收集器类型。  
  
```  
EXEC sp_syscollector_create_collector_type  
@collector_type_uid = '302E93D1-3424-4be7-AA8E-84813ECF2419',  
@name = 'Generic T-SQL Query Collector Type',  
@parameter_schema = '<?xml version="1.0" encoding="utf-8"?>  
  <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="DataCollectorType">  
    <xs:element name="TSQLQueryCollector">  
      <xs:complexType>  
        <xs:sequence>  
          <xs:element name="Query" minOccurs="1" maxOccurs="unbounded">  
            <xs:complexType>  
              <xs:sequence>  
                <xs:element name="Value" type="xs:string" />  
                <xs:element name="OutputTable" type="xs:string" />  
              </xs:sequence>  
            </xs:complexType>  
          </xs:element>  
          <xs:element name="Databases" minOccurs="0" maxOccurs="1">  
            <xs:complexType>  
              <xs:sequence>  
                <xs:element name="Database" minOccurs="0" maxOccurs="unbounded" type="xs:string" />  
              </xs:sequence>  
              <xs:attribute name="UseSystemDatabases" type="xs:boolean" use="optional" />  
              <xs:attribute name="UseUserDatabases" type="xs:boolean" use="optional" />  
            </xs:complexType>  
          </xs:element>  
        </xs:sequence>  
      </xs:complexType>  
    </xs:element>  
  </xs:schema>',  
@collection_package_id = '292B1476-0F46-4490-A9B7-6DB724DE3C0B',  
@upload_package_id = '6EB73801-39CF-489C-B682-497350C939F0';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [“数据收集”](../../relational-databases/data-collection/data-collection.md)  
  
  
