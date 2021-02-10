---
description: 替代方法：使用 SQL 语句
title: 替代方法：使用 SQL 语句 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ADO]
- editing data [ADO], sql statements
- ADO, SQL statements
ms.assetid: 8b528b23-063d-45ea-8dea-6a90d4060b20
author: rothja
ms.author: jroth
ms.openlocfilehash: 91111602587b7857559a633a816d7fbb6b125dcb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028051"
---
# <a name="alternatives-using-sql-statements"></a>替代方法：使用 SQL 语句
ADO 还允许使用命令作为其内置属性的替代方法和用于编辑数据的方法。 根据您的提供程序，本部分中提到的所有操作也可以通过将命令传递到您的数据源来实现。 例如，可以使用 SQL UPDATE 语句来修改数据，而无需使用 **字段** 的 **Value** 属性。 SQL INSERT 语句可用于向数据源添加新记录，而不是 ADO 方法 **AddNew**。 有关 SQL 或提供程序的数据操作语言的详细信息，请参阅数据源的文档。  
  
 例如，可以将包含 DELETE 语句的 SQL 字符串传递到数据库，如以下代码所示：  
  
```  
'BeginSQLDelete  
strSQL = "DELETE FROM Shippers WHERE ShipperID = " & intId  
objConn1.Execute strSQL, , adCmdText + adExecuteNoRecords  
'EndSQLDelete  
```
