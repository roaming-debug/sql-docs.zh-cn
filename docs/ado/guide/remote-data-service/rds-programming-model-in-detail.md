---
description: RDS 编程模型的详细信息
title: RDS 编程模型详细信息 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO], details
ms.assetid: 3e57af8d-519b-4467-a0bd-af468534cefd
author: rothja
ms.author: jroth
ms.openlocfilehash: 8421340798a308204a0961340e06216f3b0cb24a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031889"
---
# <a name="rds-programming-model-in-detail"></a>RDS 编程模型的详细信息
以下是 RDS 编程模型的关键元素：  
  
-   RDS.空间  
  
-   RDSServer. DataFactory  
  
-   RDS.DataControl  
  
-   事件  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="rdsdataspace"></a>RDS.空间  
 客户端应用程序必须指定要调用的服务器和服务器程序。 在返回时，应用程序将接收对服务器程序的引用，并可以将引用视为服务器程序本身。  
  
 RDS 对象模型将此功能与 Rds 结合在一起 [。空间](../../reference/rds-api/dataspace-object-rds.md) 对象。  
  
 使用程序标识符或 *ProgID* 指定服务器程序。 服务器使用 *ProgID* 和服务器计算机的注册表来查找有关要启动的实际程序的信息。  
  
 RDS 根据服务器程序是否在 Internet 或 intranet 上的远程服务器上进行区分;本地网络上的服务器;而不是在服务器上，而是在本地动态链接库 (DLL) 。 这种区别决定了如何在客户端和服务器之间交换信息，并在返回到客户端应用程序的引用类型中产生有形差异。 然而，从角度来看，这种区别没有特殊意义。 重要的是，您会收到一个可供使用的程序参考。  
  
## <a name="rdsserverdatafactory"></a>RDSServer. DataFactory  
 RDS 提供了一个默认服务器程序，该程序可以对数据源执行 SQL 查询，并返回 [recordset](../../reference/ado-api/recordset-object-ado.md) 对象或获取记录 **集** 对象并更新数据源。  
  
 RDS 对象模型将此功能与 [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象结合在一起。  
  
 此外，此对象具有创建空 **Recordset** 对象的方法，您可以 ([CreateRecordset](../../reference/rds-api/createrecordset-method-rds.md)) 以编程方式填充该对象，还可以使用另一种方法将 **记录集** 对象转换为文本字符串，以 ([ConvertToString](../../reference/rds-api/converttostring-method-rds.md)) 构建网页。  
  
 使用 ADO，你可以使用 **DataFactory** 处理程序和包含连接、命令和安全参数的自定义文件替代 **RDSServer. DataFactory** 的一些标准连接和命令行为。  
  
 服务器程序有时称为 *业务对象*。 您可以编写自己的自定义业务对象，该对象可执行复杂的数据访问、有效性检查等操作。 即使编写自定义业务对象，也可以创建 **RDSServer DataFactory** 对象的实例并使用它的某些方法来完成自己的任务。  
  
## <a name="rdsdatacontrol"></a>RDS.DataControl  
 RDS 提供了一种结合使用 Rds 的功能的方法 **。数据空间** 和 DataFactory，并使可视控件可以轻松地使用数据源中的查询所返回的 **Recordset** 对象。 **RDSServer** 最常见的情况是，RDS 尝试尽可能多地自动访问服务器上的信息，并将其显示在视觉对象控件中。  
  
 RDS 对象模型将此功能与 Rds 结合在一起 [。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象。  
  
 **RDS。DataControl** 有两个方面。 一个方面就是数据源。 如果使用 RDS 的 **Connect** 和 **SQL** 属性设置命令和连接信息 **。DataControl**，它将自动使用 **RDS。** 用于创建对默认 **RDSServer DataFactory** 对象的引用的空间。 然后， **RDSServer** 将使用 **连接** 属性值连接到数据源，使用 **SQL** 属性值从数据源中获取 **记录集** ，并将 **记录集** 对象返回到 **RDS。DataControl**。  
  
 第二个方面是在视觉对象控件中显示返回的 **记录集** 信息。 可以将视觉对象控件与 RDS 进行关联 **。DataControl** 在名为 binding) 的进程中 (，并获取对关联 **记录集** 对象中的信息的访问权限，从而在 Microsoft® Internet Explorer 中的网页上显示查询结果。 每个 **RDS。DataControl** 对象将一个 **记录集** 对象（表示单个查询的结果）绑定到一个或多个可视控件 (例如，文本框、组合框、网格控件等) 。 可能有多个 **RDS。** 每页上的 DataControl 对象。 每个 **RDS。DataControl** 对象可以连接到其他数据源，并包含单独查询的结果。  
  
 **RDS。DataControl** 对象还具有自己的方法，用于对关联的 **Recordset** 对象的行进行导航、排序和筛选。 这些方法类似，但不同于 ADO **记录集** 对象上的方法。  
  
## <a name="events"></a>事件  
 RDS 支持其自己的两个事件，这些事件独立于 ADO 事件模型。 只要 RDS，就会调用 [onReadyStateChange](../../reference/rds-api/onreadystatechange-event-rds.md) 事件 **。DataControl** [ReadyState](../../reference/rds-api/readystate-property-rds.md) 属性更改，因此当异步操作成功完成、终止或遇到错误时，通知您。 只要发生错误，就会调用 [onError](../../reference/rds-api/onerror-event-rds.md) 事件，即使在异步操作期间发生了错误。  
  
> [!NOTE]
>  Microsoft Internet Explorer 向 RDS： **onDataSetChanged** 提供了两个附加事件，这指示 **记录集** 正常运行，但仍检索行，而 **onDataSetComplete** 指示 **记录集** 已完成行的检索。  
  
## <a name="see-also"></a>另请参阅  
 [具有对象的 RDS 编程模型](./rds-programming-model-with-objects.md)   
 [DataControl 对象 (RDS) ](../../reference/rds-api/datacontrol-object-rds.md)   
 [DataFactory 对象 (RDSServer) ](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [ (RDS) 的空间对象 ](../../reference/rds-api/dataspace-object-rds.md)   
 [RDS 方案](./rds-scenario.md)   
 [RDS 教程](./rds-tutorial.md)   
 [RDS 使用情况和安全性](./rds-usage-and-security.md)