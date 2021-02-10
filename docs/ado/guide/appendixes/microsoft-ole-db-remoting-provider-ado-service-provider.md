---
description: 'Microsoft OLE DB 远程处理提供程序 (ADO 服务提供程序) '
title: Microsoft OLE DB 远程处理提供程序 (ADO 服务提供程序) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB remoting provider [ADO]
- providers [ADO], OLE DB remoting provider
- remoting provider [ADO]
ms.assetid: a4360ed4-b70f-4734-9041-4025d033346b
author: rothja
ms.author: jroth
ms.openlocfilehash: c73122e5fe1b06fb44ad69219145972ef4009202
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029241"
---
# <a name="microsoft-ole-db-remoting-provider-overview"></a>Microsoft OLE DB 远程处理提供程序概述
Microsoft OLE DB 远程处理提供程序允许客户端计算机上的本地用户调用远程计算机上的数据访问接口。 指定远程计算机的数据提供程序参数，就像远程计算机上的本地用户一样。 然后指定远程处理提供程序用来访问远程计算机的参数。 然后，你可以访问远程计算机，就像你是本地用户一样。

> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到  [WCF 数据服务](/dotnet/framework/wcf/)。

## <a name="provider-keyword"></a>Provider 关键字
 若要调用 OLE DB 远程处理提供程序，请在连接字符串中指定以下关键字和值。  (记下提供程序名称中的空格。 ) 

```vb
"Provider=MS Remote"
```

## <a name="additional-keywords"></a>其他关键字
 调用此服务提供程序时，以下附加关键字是相关的。

|关键字|说明|
|-------------|-----------------|
|**数据源**|指定远程数据源的名称。 它被传递到 OLE DB 远程处理提供程序以进行处理。<br /><br /> 此关键字等效于 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象的 [连接](../../reference/rds-api/connect-property-rds.md) 属性。|

## <a name="dynamic-properties"></a>动态属性
 调用此服务提供程序时，会将以下动态属性添加到 [连接](../../reference/ado-api/connection-object-ado.md)对象的 [properties](../../reference/ado-api/properties-collection-ado.md) 集合。

|动态属性名称|说明|
|---------------------------|-----------------|
|**DFMode**|指示 DataFactory 模式。 一个字符串，指定服务器上的 [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) 对象的所需版本。 在打开连接之前设置此属性，以请求 **DataFactory** 的特定版本。 如果请求的版本不可用，则将尝试使用上述版本。 如果没有以前的版本，则会出现错误。 如果 **DFMode** 小于可用版本，则会出现错误。 建立连接后，此属性是只读的。<br /><br /> 可以是以下有效的字符串值之一：<br /><br /> -"25"-版本 2.5 (默认值) <br />-"21"-版本2。1<br />-"20"-版本2。0<br />-"15"-版本1。5|
|**命令属性**|指示将添加到由 MS 远程提供程序发送到服务器的命令 (行集) 属性的值。 此字符串的默认值为 vt_empty。|
|**当前 DFMode**|指示服务器上的 **DataFactory** 的实际版本号。 检查此属性以查看 **DFMode** 属性中请求的版本是否有效。<br /><br /> 可以是以下有效的长整数值之一：<br /><br /> -25-版本 2.5 (默认值) <br />-21 版本2。1<br />-20-版本2。0<br />-15-版本1。5<br /><br /> 使用 **MSRemote** 提供程序时，在连接字符串中添加 "DFMode = 20;" 可以提高服务器在更新数据时的性能。 使用此设置时，服务器上的 **RDSServer DataFactory** 对象使用的资源占用资源更少。 但是，以下功能在此配置中不可用：<br /><br /> -使用参数化查询。<br />-在调用 **Execute** 方法前获取参数或列信息。<br />-将 " **Transact-sql 更新** " 设置为 " **True**"。<br />-获取行状态。<br />-调用 **Resync** 方法。<br />-通过 " **更新重新同步** " 属性显式或自动) 刷新 (。<br />-设置 **命令** 或 **记录集** 属性。<br />-使用 **adCmdTableDirect**。|
|**Handler**|指示服务器端自定义程序的名称 (或处理程序) ，该程序扩展了 [DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)的功能，以及处理程序使用的任何参数，所有这些参数由逗号分隔 ( "，" ) 。 一个字符串值。|
|**Internet 超时**|指示等待请求传输到服务器以及从服务器传递的最大毫秒数。  (默认值为5分钟。 ) |
|**远程提供程序**|指示要在远程服务器上使用的数据提供程序的名称。|
|**远程服务器**|指示此连接要使用的服务器名称和通信协议。 此属性等效于 [RDS。DataContro](../../reference/rds-api/datacontrol-object-rds.md) 对象 [服务器](../../reference/rds-api/server-property-rds.md) 属性。|
|**事务更新**|当设置为 **True** 时，此值指示在服务器上执行 [UpdateBatch](../../reference/ado-api/updatebatch-method.md) 时，它将在一个事务内完成。 此布尔动态属性的默认值为 **False**。|

 还可以通过在连接字符串中将其名称指定为关键字，来设置可写的动态属性。 例如，通过指定以下内容将 **Internet 超时** 动态属性设置为5秒：

```vb
Dim cn as New ADODB.Connection
cn.Open "Provider=MS Remote;Internet Timeout=5000"
```

 还可以通过指定动态属性的名称作为 **属性** 属性的索引来设置或检索该属性。 下面的示例演示如何获取和打印 **Internet 超时** 动态属性的当前值，并设置一个新值：

```vb
Debug.Print cn.Properties("Internet Timeout")
cn.Properties("Internet Timeout") = 5000
```

## <a name="remarks"></a>备注
 在 ADO 2.0 中，只能在 [Recordset](../../reference/ado-api/recordset-object-ado.md)对象 **Open** 方法的 *ActiveConnection* 参数中指定 OLE DB 远程处理提供程序。 从 ADO 2.1 开始，还可以在 [连接](../../reference/ado-api/connection-object-ado.md)对象 **Open** 方法的 *ConnectionString* 参数中指定提供程序。

 等效于 **RDS。DataControl** 对象 [SQL](../../reference/rds-api/sql-property.md) 属性不可用。 改为使用 [Recordset](../../reference/ado-api/recordset-object-ado.md) 对象 **Open** method *Source* 参数。

 **注意** 指定 "...;远程访问接口 = MS 远程; ... "将创建一个四层方案。 除了三个层以外的方案尚未经过测试，因此不需要这样做。

## <a name="example"></a>示例
 此示例对名为 *YourServer* 的服务器上 **Pubs** 数据库的 **Authors** 表执行查询。 远程数据源和远程服务器的名称是在[连接](../../reference/ado-api/connection-object-ado.md)对象的[open](../../reference/ado-api/open-method-ado-connection.md)方法中提供的，SQL 查询是在[Recordset](../../reference/ado-api/recordset-object-ado.md)对象的[open](../../reference/ado-api/open-method-ado-recordset.md)方法中指定的。 返回、编辑 **记录集** 对象并用于更新数据源。

```vb
Dim rs as New ADODB.Recordset
Dim cn as New ADODB.Connection
cn.Open  "Provider=MS Remote;Data Source=pubs;" & _
         "Remote Server=https://YourServer"
rs.Open "SELECT * FROM authors", cn
...                'Edit the recordset
rs.UpdateBatch     'Equivalent of RDS SubmitChanges
...
```

## <a name="see-also"></a>另请参阅
 [OLE DB 远程处理提供程序概述](/previous-versions/windows/desktop/ms713673(v=vs.85))