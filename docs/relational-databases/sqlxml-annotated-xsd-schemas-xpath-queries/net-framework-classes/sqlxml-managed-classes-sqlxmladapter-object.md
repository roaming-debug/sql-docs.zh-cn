---
title: SQLXML)  (对象
description: 了解 SqlXmlAdapter 对象，该对象提供有助于与 .NET Framework 中的数据集进行交互的方法。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- void Update(DataSet ds) method
- SqlXmlAdapter object
- void Fill(DataSet ds) method
- SQLXML Managed Classes, SqlXmlAdapter object
- Managed Classes [SQLXML], SqlXmlAdapter object
ms.assetid: 0a16eddf-fc26-4d92-82d4-359b5fb905d5
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b58e0416907b32821c2d6dd99decf61ec0054917
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97414048"
---
# <a name="sqlxml-managed-classes---sqlxmladapter-object"></a>SQLXML 托管类 - SqlXmlAdapter 对象
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  此对象提供用于促进与 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .NET Framework 中的数据集进行交互的方法。 有关工作示例，请参阅 [在 .Net 环境中访问 SQLXML 功能](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/accessing-sqlxml-functionality-in-the-net-environment.md)。  
  
 SqlXmlAdapter 对象支持以下方法：  
  
 void Fill (DataSet ds)   
 用从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 检索的 XML 数据填充 .NET Framework 中的数据集。  
  
 void 更新 (DataSet ds)   
 用数据集中的数据更新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的记录。  
  
 SqlXmlAdapter 对象支持以下构造函数：  
  
```  
public SqlXmlAdapter(SqlXmlCommand  cmd)   
  
public SqlXmlAdapter(  
                     string commandText,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                      )   
  
public SqlXmlAdapter(  
                     Stream commandStream,   
                     SqlXmlCommandType cmdType,   
                     string connectionString  
                     )   
```  
  
## <a name="see-also"></a>另请参阅  
 [SQLXML 托管类 &#40;的 SqlXmlCommand 对象&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlcommand-object.md)   
 [SQLXML 托管类 &#40;的 SqlXmlParameter 对象&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-managed-classes-sqlxmlparameter-object.md)  
  
  
