---
title: SQLXML) 的 NULL 处理 (
description: 了解如何通过使用 updg： nullvalue 特性，在 SQLXML 4.0 updategram 中指定 NULL 特性或元素。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- updg:nullvalue attribute
- updategrams [SQLXML], null values
- nullvalue attribute
- null values [SQLXML]
ms.assetid: 5e11eebb-d94e-4ce6-a6d0-870225706bc1
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b3fac9c5de10981dbb8afff517a106135ccca82f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484819"
---
# <a name="null-handling-sqlxml-40"></a>NULL 处理 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  XML 语法将 NULL 视为不存在。  (例如，如果属性或元素值为 NULL，则该属性或元素在 XML 文档中不存在。在 SQLXML 中 ) [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ， **updg： nullvalue** 属性允许为元素或属性值指定 NULL。  
  
 例如，以下 updategram 确保 **ContactID** 为64的联系人的 **标题** 值为 NULL，然后将 **Title** 值更新为 "Mr"。 此联系人。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync updg:nullvalue="IsNULL"  >  
    <updg:before>  
       <Person.Contact ContactID="64" Title="IsNULL" />  
    </updg:before>  
    <updg:after>  
       <Person.Contact ContactID="64" Title="Mr." />  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 参数传递到 Updategram 时，NULL 可以作为参数值进行传递。 这是通过在块中指定 **nullvalue** 属性来完成的 **\<updg:header>** 。 有关示例，请参阅 [将参数传递到 updategram &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/passing-parameters-to-updategrams-sqlxml-4-0.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
