---
description: 自定义 DataFactory
title: 自定义 DataFactory |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory customization in RDS [ADO]
ms.assetid: 86d77985-a0d0-405a-8587-c85a20540a0e
author: rothja
ms.author: jroth
ms.openlocfilehash: 4bc3a017e74e8384bd97d9503663731e353f2242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031979"
---
# <a name="datafactory-customization"></a>自定义 DataFactory
远程数据服务 (RDS) 提供一种在三层客户端/服务器系统中轻松执行数据访问的方法。 客户端数据控件指定连接和命令字符串参数以对远程数据源执行查询，或指定连接字符串和 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象参数来执行更新。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 将参数传递给服务器程序，该程序对远程数据源执行数据访问操作。 RDS 提供名为 [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象的默认服务器程序。 **RDSServer. DataFactory** 对象将查询生成的任何 **记录集** 对象返回给客户端。  
  
 但是， **RDSServer DataFactory** 仅限于执行查询和更新。 它无法对连接或命令字符串执行任何验证或处理。  
  
 使用 ADO，你可以将 **DataFactory** 指定为与另一种类型的服务器程序（称为 *处理程序*）一起工作。 处理程序可以在使用客户端连接和命令字符串访问数据源之前修改它们。 此外，处理程序可以强制实施访问权限，这些访问权限控制客户端读取数据和将数据写入数据源的能力。  
  
 处理程序用来修改客户端参数和访问权限的参数是在自定义文件的节中指定的。  
  
 以下主题提供了有关自定义 **DataFactory** 对象的详细信息。  
  
-   [了解自定义文件](./understanding-the-customization-file.md)  
  
-   [自定义文件 Connect 部分](./customization-file-connect-section.md)  
  
-   [自定义文件 SQL 部分](./customization-file-sql-section.md)  
  
-   [自定义文件 UserList 部分](./customization-file-userlist-section.md)  
  
-   [自定义文件 Logs 部分](./customization-file-logs-section.md)  
  
-   [必需的客户端设置](./required-client-settings.md)  
  
-   [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)