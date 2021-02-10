---
description: 指定连接属性
title: 指定连接属性 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connection properties [ADO]
- connections [ADO]
ms.assetid: 49456201-b085-4851-9686-e814136b07be
author: rothja
ms.author: jroth
ms.openlocfilehash: d69af7af414c1c864d47e39160f4b4a94d726ac9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032449"
---
# <a name="specifying-connection-properties"></a>指定连接属性
可以通过在打开连接之前设置 **连接** 对象的属性，来提供 [连接字符串](../../../ado/guide/data/creating-a-connection-string.md)所指定的大量信息。 例如，你可以通过使用以下代码来实现与 [创建连接字符串](../../../ado/guide/data/creating-a-connection-string.md) 中所述的连接字符串相同的效果。  
  
```  
With objConn  
.Provider = "SQLOLEDB"  
.Properties("Data Source") = "MySqlServer"  
   .Properties("Integrated Security") = "SSPI"  
.Open  
.DefaultDatabase = "Northwind"  
End With  
```  
  
 只有打开连接后才设置 DefaultDatabase。  
  
> [!NOTE]
>  在 ADO 中，不能使用包含分号的密码 ( ";") ，除非密码用单引号引起来。
