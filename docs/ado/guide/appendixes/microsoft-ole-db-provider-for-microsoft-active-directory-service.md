---
description: Microsoft OLE DB Provider for Microsoft Active Directory 服务
title: Microsoft OLE DB 提供商 Active Directory 服务 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADSI provider [ADO]
- Active Directory Service Interfaces provider [ADO]
- providers [ADO], OLE DB provider for Active Directory service
- OLE DB provider for Active Directory service [ADO]
ms.assetid: f9e81452-5675-4cfc-9949-cfbd2fe57534
author: rothja
ms.author: jroth
ms.openlocfilehash: 2174610e0747f984babae59c4b00d1245d763db2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029271"
---
# <a name="microsoft-ole-db-provider-for-microsoft-active-directory-service"></a>Microsoft OLE DB Provider for Microsoft Active Directory 服务
Active Directory Service 接口 (ADSI) 提供程序允许 ADO 通过 ADSI 连接到异类目录服务。 这使得 ADO 应用程序除了任何符合 LDAP 的目录服务和 Novell 目录服务之外，还可以对 Microsoft Windows NT 4.0 和 Microsoft Windows 2000 目录服务进行只读访问。 ADSI 本身基于提供程序模型，因此，如果有新的提供程序可访问另一个目录，ADO 应用程序将能够无缝地访问它。 ADSI 提供程序启用了自由线程和 Unicode。  
  
## <a name="connection-string-parameters"></a>连接字符串参数  
 若要连接到该提供程序，请将 [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md)属性的 **provider** 参数设置为以下内容：  
  
```vb
ADSDSOObject  
```  
  
 读取 [提供程序](../../reference/ado-api/provider-property-ado.md) 属性也会返回此字符串。  
  
## <a name="typical-connection-string"></a>典型连接字符串  
 此提供程序的典型连接字符串如下所示：  
  
```vb
"Provider=ADSDSOObject;User ID=MyUserID;Password=MyPassword;"  
```  
  
 字符串包含以下关键字。  
  
|关键字|说明|  
|-------------|-----------------|  
|**提供程序**|指定 Active Directory 服务的 OLE DB 提供程序。|  
|**用户 ID**|指定用户名。 如果省略此关键字，则使用当前登录名。|  
|**密码**|指定用户密码。 如果省略此关键字，则为。 则使用当前登录名。|  
  
> [!NOTE]
>  如果要连接到支持 Windows 身份验证的数据源提供程序，应在连接字符串中指定 **Trusted_Connection = yes** 或 **集成安全性 = SSPI** 而不是用户 ID 和密码信息。  
  
## <a name="command-text"></a>命令文本  
 提供程序使用以下语法识别由四个部分组成的命令文本字符串：  
  
```vb
"Root; Filter; Attributes[; Scope]"  
```  
  
|值|说明|  
|-----------|-----------------|  
|*Root*|指示从其开始搜索的 **ADsPath** 对象 (即，搜索) 的根。|  
|*Filter*|指示 RFC 1960 格式的搜索筛选器。|  
|*特性*|指示要返回的属性的逗号分隔列表。|  
|*Scope*|可选。 指定搜索范围的 **字符串** 。 可以是以下其中一个值：<br /><br /> -仅搜索基本对象 (搜索) 的根。<br />-OneLevel-仅搜索一级。<br />-子树-搜索整个子树。|  
  
 例如：  
  
```vb
"<LDAP://DC=ArcadiaBay,DC=COM>;(objectClass=*);sn, givenName; subtree"  
```  
  
 提供程序还支持 SQL SELECT for 命令文本。 例如：  
  
```vb
"SELECT title, telephoneNumber From 'LDAP://DC=Microsoft, DC=COM' WHERE   
objectClass='user' AND objectCategory='Person'"  
```  
  
## <a name="remarks"></a>备注  
 提供程序不接受存储过程调用或简单的表名称 (例如， [CommandType](../../reference/ado-api/commandtype-property-ado.md) 属性将始终) 为 **adCmdText** 。 有关命令文本元素的更全面说明，请参阅 Active Directory Service 接口文档。  
  
## <a name="recordset-behavior"></a>记录集行为  
 下表列出了使用此提供程序打开的 [记录集](../../reference/ado-api/recordset-object-ado.md) 对象上的可用功能。 只有静态游标类型 (**adOpenStatic**) 可用。  
  
 有关提供程序配置的 **记录集** 行为的详细信息，请运行 [支持](../../reference/ado-api/supports-method.md)方法，并枚举 **记录集** 的 [Properties](../../reference/ado-api/properties-collection-ado.md)集合，以确定是否存在特定于提供程序的动态属性。  
  
 **标准 ADO 记录集属性的可用性：**  
  
|属性|可用性|  
|--------------|------------------|  
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|读/写|  
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|读/写|  
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|只读|  
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|只读|  
|书签 |读/写|  
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|读/写|  
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|始终 **adUseServer**|  
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|始终 **adOpenStatic**|  
|[EditMode](../../reference/ado-api/editmode-property.md)|始终 **adEditNone**|  
|[EOF](../../reference/ado-api/bof-eof-properties-ado.md)|只读|  
|[Filter](../../reference/ado-api/filter-property.md)|读/写|  
|[LockType](../../reference/ado-api/locktype-property-ado.md)|读/写|  
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|不可用|  
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|读/写|  
|[PageCount](../../reference/ado-api/pagecount-property-ado.md)|只读|  
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|读/写|  
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|只读|  
|[Source](../../reference/ado-api/source-property-ado-recordset.md)|读/写|  
|[State](../../reference/ado-api/state-property-ado.md)|只读|  
|[Status](../../reference/ado-api/status-property-ado-recordset.md)|只读|  
  
 **标准 ADO 记录集方法的可用性：**  
  
|方法|是否可用？|  
|------------|----------------|  
|[AddNew](../../reference/ado-api/addnew-method-ado.md)|否|  
|[取消](../../reference/ado-api/cancel-method-ado.md)|否|  
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|否|  
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|否|  
|[克隆](../../reference/ado-api/clone-method-ado.md)|是|  
|[关闭](../../reference/ado-api/close-method-ado.md)|是|  
|[删除](../../reference/ado-api/delete-method-ado-recordset.md)|否|  
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|是|  
|[移动](../../reference/ado-api/move-method-ado.md)|是|  
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|  
|[MoveLast](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|  
|[MoveNext](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|  
|[MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|是|  
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)|是|  
|[打开](../../reference/ado-api/open-method-ado-recordset.md)|是|  
|[重新](../../reference/ado-api/requery-method.md)|是|  
|[重新同步](../../reference/ado-api/resync-method.md)|是|  
|[支持](../../reference/ado-api/supports-method.md)|是|  
|[Update](../../reference/ado-api/update-method.md)|否|  
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|否|  
  
 有关 ADSI 的详细信息以及提供程序的具体信息，请参阅 Active Directory 服务接口文档或访问 ADSI 网页。  
  
## <a name="see-also"></a>另请参阅  
 [CommandType 属性 (ADO) ](../../reference/ado-api/commandtype-property-ado.md)   
 [ (ADO) 的 ConnectionString 属性 ](../../reference/ado-api/connectionstring-property-ado.md)   
 [ADO)  (属性集合 ](../../reference/ado-api/properties-collection-ado.md)   
 [ADO) 提供程序属性 (](../../reference/ado-api/provider-property-ado.md)   
 [ADO)  (Recordset 对象 ](../../reference/ado-api/recordset-object-ado.md)   
 [Supports 方法](../../reference/ado-api/supports-method.md)