---
description: Execute21 方法 (RDS)
title: Execute21 方法 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Execute21 method [RDS]
ms.assetid: 9f131c8d-1497-416d-8209-abb481c38f7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bd48d561f9b21997df0f7a77a993f95900e8cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053208"
---
# <a name="execute21-method-rds"></a>Execute21 方法 (RDS)
执行请求并创建 ADO 记录集以在 ADO 2.1 中使用。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.Execute21(ConnectionString As String, HandlerString As String, QueryString As String, lMarshalOptions As Long, Properties, TableId, lExecuteOptions As Long, pParameters)  
```  
  
#### <a name="parameters"></a>参数  
 *ConnectionString*  
 一个字符串，用于连接到将发送请求以执行的 OLE DB 提供程序。 如果使用 *HandlerString* 指定处理程序，则它可以编辑或替换连接字符串。  
  
 *HandlerString*  
 字符串标识要与此执行一起使用的处理程序。 该字符串包含两个部分。 第一部分包含要使用的处理程序 (ProgID) 的名称。 字符串的第二部分包含要传递给处理程序的参数。 如何解释参数字符串是处理程序特定的。 尽管参数字符串可以包含其他逗号) ，但这两个部分由字符串中的第一个逗号实例分隔 (。 这些参数是可选的。  
  
 *字符串*  
 在连接字符串中标识的 OLE DB 提供程序支持的命令语言中的命令。 对于基于 SQL 的提供程序，它可能包含 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 命令语句，但对于非 SQL 提供程序 (例如，MSDataShape) 这可能不是 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 查询语句。  
  
 此外，如果正在使用处理程序 (并且强烈建议) 使用处理程序，则该处理程序可以更改或替换此处指定的值。 例如，处理程序通常将 *QueryString* 替换为其 .ini 文件中的查询字符串。 默认情况下，使用 Msdfmap.ini 文件。  
  
 *lMarshalOptions*  
 用于设置所返回的行集/记录集的封送处理选项。  
  
 *TableID*  
 VT_EMPTY 或 VT_BSTR 类型的变体。 如果此值为 VT_EMPTY 类型，则将忽略此值。 如果它的类型为 VT_BSTR，则使用 **adCmdTableDirect** 创建记录集，并使用在此处指定的值并忽略 *QueryString* 参数。  
  
 *lExecuteOptions*  
 执行选项的位掩码：  
  
 1 =*ReadOnly* 将使用 **adLockReadOnly** 打开记录集。  
  
 2 =*NoBatch* 将使用 **adLockOptimistic** 打开记录集。  
  
 4 =*AllParamInfoSupplied* 调用方保证在 *pParameters* 中提供所有参数的参数信息。  
  
 8 = 将从 OLE DB 提供程序获取查询的 *GetInfo* 参数信息，并在 *pParameters* 参数中返回该信息。 查询不会执行，并且不会返回任何记录集。  
  
 16 = GetHiddenColumns 将使用 **adLockBatchOptimistic** 打开记录集，任何隐藏的列都将包含在记录集中。  
  
 尽管 *ReadOnly*、 *NoBatch* 和 *GetHiddenColumns* 是互斥的选项，但设置多个选项并不是错误。 如果设置了多个选项，则 *GetHiddenColumns* 将优先于所有其他选项，然后是 *ReadOnly*。 如果未指定任何选项，则默认情况下，将使用 **adLockBatchOptimistic** 打开记录集，但不会在记录集中包含隐藏列。  
  
 *pParameters*  
 一个包含参数定义的安全数组的变体。 如果在 *lExecuteOptions* 中指定了 *GetInfo* 选项，则此参数用于返回从 OLE DB 提供程序获取的参数定义。 否则，此参数可能为空。  
  
## <a name="remarks"></a>备注  
 *HandlerString* 参数可以为 null。 在这种情况下，会发生什么情况取决于 RDS 服务器的配置方式。 "MSDFMAP" 的处理程序字符串指示应使用 Microsoft 提供的处理程序 ( # A0) 。 "MASDFMAP，sample.ini" 的处理程序字符串指示应使用 Msdfmap.dll 处理程序，并且应将参数 "sample.ini" 传递到处理程序。 MSDFMAP.dll 会将参数解释为使用 sample.ini 检查连接和查询字符串的方向。  
  
> [!NOTE]
>  **Execute21** 方法是 [ (RDS) 的 Execute 方法](./execute-method-rds.md)的版本。 如果需要使用 **Execute** 方法与 ADO 2.1 进行通信，则可以改为调用 **Execute21** 方法。 ADO 2.5 和更高版本中 **执行** 方法的功能是为 ADO 2.1 中的同一方法提供的功能的超集。  
  
## <a name="applies-to"></a>应用于  
 [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)