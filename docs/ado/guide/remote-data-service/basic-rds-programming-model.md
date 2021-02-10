---
description: 基本的 RDS 编程模型
title: 基本 RDS 编程模型 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO]
ms.assetid: 0bdd236b-edff-4aac-94c3-93e1465ca6c5
author: rothja
ms.author: jroth
ms.openlocfilehash: 81d789c003921b4e8359df281cb9fa51dbdf1f26
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036576"
---
# <a name="basic-rds-programming-model"></a>基本的 RDS 编程模型
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 RDS 解决了在以下环境中存在的应用程序：客户端应用程序指定将在服务器上执行的程序以及返回所需信息所需的参数。 服务器上调用的程序将获得对指定数据源的访问权限、检索信息，还可以选择处理数据，然后将生成的信息以可以轻松使用的形式返回到客户端应用程序。 RDS 提供了执行以下一系列操作的方法：  
  
-   指定要在服务器上调用的程序，并获取从客户端引用该程序的方法。  (此引用有时称为 *代理*。 它表示远程服务器程序。 客户端应用程序将 "调用" 代理程序，就像它是本地程序一样，但是它实际上调用远程服务器程序。 )   
  
-   调用服务器程序。 将参数传递给用于标识数据源和要发出的命令的服务器程序。  (服务器程序实际使用 ADO 获取对数据源的访问权限。 ADO 使用给定的参数之一进行连接，然后发出在其他参数中指定的命令。 )   
  
-   服务器程序从数据源中获取 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象。 或者，在服务器上处理 **Recordset** 对象。  
  
-   服务器程序将最终的 **记录集** 对象返回到客户端应用程序。  
  
-   在客户端上， **记录集** 对象将被置于可由视觉对象轻松使用的窗体中。  
  
-   对 **Recordset** 对象的任何修改都将发送回服务器程序，该程序使用这些修改来更新数据源。  
  
 此编程模型包含某些便利功能。 如果你不需要复杂的服务器程序来访问数据源，并且如果提供所需的连接和命令参数，则 RDS 将使用简单的默认服务器程序自动检索指定的数据。  
  
 如果需要更复杂的处理，可以指定您自己的自定义服务器程序。 例如，由于自定义服务器程序在其处理中具有 ADO 的全部功能，因此它可以连接到多个不同的数据源，以一种复杂的方式组合其数据，然后将简单的处理结果返回给客户端应用程序。  
  
 最后，如果你的需求介于之间，ADO 现在支持自定义默认服务器程序的行为。  
  
## <a name="see-also"></a>另请参阅  
 [RDS 编程模型详细信息](./rds-programming-model-in-detail.md)   
 [RDS 方案](./rds-scenario.md)   
 [RDS 教程](./rds-tutorial.md)   
 [ADO)  (Recordset 对象 ](../../reference/ado-api/recordset-object-ado.md)   
 [RDS 使用情况和安全性](./rds-usage-and-security.md)