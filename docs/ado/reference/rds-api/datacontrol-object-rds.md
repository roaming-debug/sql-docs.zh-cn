---
description: DataControl 对象 (RDS)
title: DataControl 对象 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataControl
- RDS.DataControl
helpviewer_keywords:
- DataControl object [ADO]
ms.assetid: d85ea4fc-451c-436e-97b8-58f92b149dd0
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c65d8f639ec1747e7085f8d88c266ed6b6299d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169011"
---
# <a name="datacontrol-object-rds"></a>DataControl 对象 (RDS)
将数据查询 [记录集](../ado-api/recordset-object-ado.md) 绑定到一个或多个控件 (例如，文本框、网格控件或组合框) ，用于在网页上显示 **记录集** 数据。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
</OBJECT>  
```  
  
## <a name="remarks"></a>备注  
 RDS 的类 ID **。DataControl** 对象为 BD96C556-65A3-11D0-983A-00C04FC29E33。  
  
> [!NOTE]
>  如果收到 [RDS。空间](./dataspace-object-rds.md) 或 **RDS。DataControl** 对象不会加载，请确保使用的是正确的类 ID。 这些对象的类 Id 已从版本1.0 和1.1 更改。 另外，请注意，在使用 **RDS DataControl** 对象时，必须设置甚至可以为 null 的列。  
  
 对于基本方案，只需设置 RDS 的 **SQL**、 **Connect** 和 **Server** 属性 **。DataControl** 对象，它将自动调用默认业务对象 [RDSServer. DataFactory](./datafactory-object-rdsserver.md)。  
  
 RDS 中的所有属性 **。DataControl** 是可选的，因为自定义业务对象可以替换其功能。  
  
> [!NOTE]
>  如果查询多个结果，则仅返回第一个 [记录集](../ado-api/recordset-object-ado.md) 。 如果需要多个结果集，请将每个结果集分配给它自己的 **DataControl**。 以下为多个结果的查询示例： `"Select * from Authors, Select * from Topics"`  
  
 使用 RDS 时，将 "DFMode = 20;" 添加到连接字符串 **。** 当你更新数据时，DataControl 对象可以提高服务器的性能。 使用此设置时，服务器上的 **RDSServer DataFactory** 对象使用的资源占用资源更少。 但是，以下功能在此配置中不可用：  
  
-   使用参数化查询。  
  
-   在调用 **Execute** 方法之前获取参数或列信息。  
  
-   将 " **Transact-sql 更新** " 设置为 " **True**"。  
  
-   正在获取行状态。  
  
-   调用 [Resync](../ado-api/resync-method.md) 方法。  
  
-   通过 " [更新重新同步](../ado-api/update-resync-property-dynamic-ado.md) " 属性显式刷新 (或自动) 。  
  
-   设置 **命令** 或 [记录集](./recordset-sourcerecordset-properties-rds.md) 属性。  
  
-   使用 **adCmdTableDirect**。  
  
 **RDS。** 默认情况下，DataControl 对象在异步模式下运行。 如果需要对应用程序执行同步，请将 [ExecuteOptions](./executeoptions-property-rds.md) 参数设置为等于 **AdcExecSync** ，将 [FetchOptions](./fetchoptions-property-rds.md) 参数设置为等于 **adcFetchUpFront**，如下面的示例所示。  
  
```  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"   
    ID="DataControl"  
   <PARAM NAME="Connect" VALUE="DSN=DSNName;UID=MyUserID;PWD=MyPassword;">  
   <PARAM NAME="Server" VALUE="https://awebsrvr">  
   <PARAM NAME="SQL" VALUE="QueryText">  
   <PARAM NAME="ExecuteOptions" VALUE="1">   <PARAM NAME="FetchOptions" VALUE="1">  
</OBJECT>  
```  
  
 使用一个 **RDS。DataControl** 对象，用于将一个查询的结果链接到一个或多个可视控件。 例如，假设您对请求客户数据的查询进行编码，例如姓名、居民、出生地、年龄和优先级客户状态。 您可以使用单个 **RDS。** 用于在三个单独的文本框中显示客户姓名、年龄和区域的 DataControl 对象;复选框中的优先级客户状态;以及网格控件中的所有数据。  
  
 使用不同 **的 RDS。DataControl** 对象，用于将多个查询的结果链接到不同的视觉对象。 例如，假设您使用一个查询来获取有关客户的信息，另一个查询用于获取有关客户已购买的商品的信息。 需要在三个文本框和一个复选框中显示第一个查询的结果，并在网格控件中显示第二个查询的结果。 如果使用默认业务对象 (**RDSServer. DataFactory**) ，则必须执行以下操作：  
  
-   添加两个 **RDS。** 将对象 DataControl 到网页中。  
  
-   编写两个查询，每个查询对应两个 RDS 的 **SQL** 属性 **。DataControl** 对象。 一个 **RDS。DataControl** 对象将包含请求客户信息的 SQL 查询;第二个将包含一个查询，请求客户已购买的商品的列表。  
  
-   在每个绑定控件的对象标记中，指定 DATAFLD 值以设置要在每个可视化控件中显示的数据的值。  
  
 RDS 的数量没有限制 **。DataControl** 对象，可以通过在单个网页上使用对象标记进行嵌入。  
  
 定义 **RDS 时。DataControl** 对象在网页上，使用非零 **高度** 和 **宽度** 值（如 1 (）以避免包含额外的空间) 。  
  
 远程数据服务客户端组件已作为 Internet Explorer 4.0 的一部分包含在内;因此，不需要在 RDS 中包含 CODEBASE 参数 **。DataControl** 对象标记。  
  
 使用 Internet Explorer 4.0 或更高版本时，仅当 HTML 控件和 ActiveX®控件标记为单元模型控件时，才能使用这些控件绑定到数据。  
  
> [!NOTE]
>  **Microsoft Visual Basic 用户****RDS。DataControl** 对于脚本是安全的，仅在基于 Web 的应用程序中使用。 Visual Basic 客户端应用程序不需要此应用程序。  
  
 本部分包含以下主题。  
  
-   [DataControl 对象 (RDS) 属性、方法和事件](./datacontrol-object-rds-properties-methods-and-events.md)  
  
## <a name="see-also"></a>另请参阅  
 [DataControl 对象示例 (VBScript)](./datacontrol-object-example-vbscript.md)