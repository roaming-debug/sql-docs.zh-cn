---
description: Open 方法（ADO 记录集）
title: " (ADO 记录集) 打开方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Open
- Recordset15::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 3236749c-4b71-4235-89e2-ccdfaaa9319d
author: rothja
ms.author: jroth
ms.openlocfilehash: 0f9cf81ef9845875db984e4cba0fcd7bc23e380c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041397"
---
# <a name="open-method-ado-recordset"></a>Open 方法（ADO 记录集）
打开 [记录集](./recordset-object-ado.md) 对象上的游标。  
  
## <a name="syntax"></a>语法  
  
```  
  
recordset.Open Source, ActiveConnection, CursorType, LockType, Options  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 一个 **变量**，该变量的计算结果为有效的 [命令](./command-object-ado.md)对象、SQL 语句、表名、存储过程调用、URL 或包含持久存储的 [记录集](./recordset-object-ado.md)的文件或 [流](./stream-object-ado.md)对象的名称。  
  
 *ActiveConnection*  
 可选。 计算结果为有效的 [连接](./connection-object-ado.md)对象变量名称的 **变量**，或包含 [ConnectionString](./connectionstring-property-ado.md)参数的 **字符串**。  
  
 *CursorType*  
 可选。 一个 [CursorTypeEnum](./cursortypeenum.md) 值，确定提供程序在打开 **记录集** 时应使用的游标类型。 默认值为 **adOpenForwardOnly**。  
  
 *LockType*  
 可选。 [LockTypeEnum](./locktypeenum.md)值，用于确定提供程序在打开 **记录集** 时应使用的 (并发) 类型。 默认值为 **adLockReadOnly**。  
  
 *选项*  
 可选。 一个 **长整型** 值，该值指示当提供程序表示某个 **命令** 对象以外的内容时，该提供程序应如何计算 *Source* 参数，或者应从以前保存该记录集的文件还原该 **记录集**。 可以是一个或多个 [CommandTypeEnum](./commandtypeenum.md) 或 [ExecuteOptionEnum](./executeoptionenum.md) 值，可以与按位 or 运算符组合。  
  
> [!NOTE]
>  如果从包含持久 **记录集** 的 **流** 打开 **记录集**，则使用 **adAsyncFetchNonBlocking** 的 [ExecuteOptionEnum](./executeoptionenum.md)值将不起任何作用;提取将是同步和阻塞。  
  
> [!NOTE]
>  **AdExecuteNoRecords** 或 **adExecuteStream** 的 **ExecuteOpenEnum** 值不应与 **Open** 一起使用。  
  
## <a name="remarks"></a>备注  
 ADO **记录集** 的默认游标是位于服务器上的只进只读游标。  
  
 对 **Recordset** 对象使用 **Open** 方法将打开一个游标，该游标表示基表、查询结果或以前保存的 **记录集** 的记录。  
  
 使用可选的 *Source* 自变量来指定数据源，使用以下项之一： **命令** 对象变量、SQL 语句、存储过程、表名称、URL 或完整的文件路径名称。 如果 *源* 是文件路径名称，则它可以是 ( "c:\dir\file.rst" ) 的完整路径， ( "。\file.rst ") 或 () 的 URL `https://files/file.rst` 。  
  
 使用 **Open** 方法的 *Source* 参数执行不返回记录的操作查询是一个不错的做法，因为没有简单的方法来确定调用是否成功。 此类查询返回的 **记录集** 将关闭。 若要执行不返回记录的查询（如 SQL INSERT 语句），请改为调用 **命令** 对象的 [Execute](./execute-method-ado-command.md)方法或 [连接](./connection-object-ado.md)对象的 [execute](./execute-method-ado-connection.md)方法。  
  
 *ActiveConnection* 参数对应于 [ActiveConnection](./activeconnection-property-ado.md)属性，并指定打开 **Recordset** 对象的连接。 如果传递此参数的连接定义，ADO 将使用指定的参数打开新连接。 通过将 [CursorLocation](./cursorlocation-property-ado.md)属性设置为 **adUseClient**，打开包含客户端游标的 **记录集** 后，可以更改此属性的值以将更新发送到另一个提供程序。 或者，你可以在 Microsoft Visual Basic) 中将此属性设置为 " (**无** "，或者为 NULL，以便从任何提供程序断开 **记录集** 的连接。 但更改服务器端游标的 *ActiveConnection* 会生成错误。  
  
 对于直接与 **Recordset** 对象的属性相对应的其他参数 (*源*、 *CursorType* 和 *LockType*) ，属性的参数关系如下：  
  
-   在打开 **Recordset** 对象之前，该属性是可读/写的。  
  
-   除非在执行 **Open** 方法时传递相应的参数，否则将使用属性设置。 如果传递参数，它将重写相应的属性设置，并使用参数值更新属性设置。  
  
-   打开 **Recordset** 对象之后，这些属性将变为只读。  
  
> [!NOTE]
>  对于 [Source](./source-property-ado-recordset.md)属性设置为有效 **命令** 对象的 **recordset** 对象， **ActiveConnection** 属性是只读的，即使 **recordset** 对象未打开也是如此。  
  
 如果在 *Source* 参数中传递 **Command** 对象并同时传递 *ActiveConnection* 参数，则会发生错误。 **Command** 对象的 **ActiveConnection** 属性必须已设置为有效的 **连接** 对象或连接字符串。  
  
 如果在 *source* 参数中传递除 **命令** 对象之外的内容，则可以使用 *Options* 参数优化 *source* 参数的计算。 如果未定义 *Options* 参数，则可能会遇到性能降低的情况，因为 ADO 必须对提供程序进行调用，以确定参数是 SQL 语句、存储过程、URL 还是表名称。 如果知道所使用的 *源* 类型，设置 *Options* 参数会指示 ADO 直接跳转到相关代码。 如果 *Options* 参数与 *源* 类型不匹配，则会发生错误。  
  
 如果在 *Source* 参数中传递 **Stream** 对象，则不应将信息传递到其他参数。 这样做会导致错误生成。 从 **流** 中打开 **记录集** 时，不会保留 **ActiveConnection** 信息。  
  
 如果没有与 **记录集** 相关联的连接，则 *Options* 参数的默认值为 **adCmdFile** 。 这通常是永久存储的 **记录集** 对象的事例。  
  
 如果数据源未返回任何记录，则提供程序会将 [BOF](./bof-eof-properties-ado.md) 和 [EOF](./bof-eof-properties-ado.md) 属性都设置为 **True**，并且不定义当前记录位置。 如果游标类型允许，你仍可以向此空 **Recordset** 对象添加新数据。  
  
 对打开的 **记录集** 对象结束操作后，请使用 [Close](./close-method-ado.md) 方法释放任何关联的系统资源。 关闭对象并不会将其从内存中删除;您可以更改其属性设置，并使用 **open** 方法稍后再次打开它。 若要从内存中完全消除对象，请将对象变量设置为 *Nothing*。  
  
 设置 **ActiveConnection** 属性之前，请调用 **Open** with no 操作数，以创建通过向 **recordset** [字段](./fields-collection-ado.md)集合追加字段而创建的 **记录集** 的实例。  
  
 如果已将 [CursorLocation](./cursorlocation-property-ado.md) 属性设置为 **adUseClient**，则可以通过以下两种方式之一异步检索行。 推荐的方法是将 *选项* 设置为 **adAsyncFetch**。 或者，您可以使用 [Properties](./properties-collection-ado.md) 集合中的 "异步行集处理" 动态属性，但如果不将 *Options* 参数设置为 **adAsyncFetch**，则相关的检索事件可能会丢失。  
  
> [!NOTE]
>  仅通过 **Open** 方法的 *OPTIONS* 参数支持 MS 远程访问接口中的后台获取。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
 [CommandTypeEnum](./commandtypeenum.md)和[ExecuteOptionEnum](./executeoptionenum.md)值的某些组合无效。 有关无法组合的选项的信息，请参阅 [ExecuteOptionEnum](./executeoptionenum.md)和 [CommandTypeEnum](./commandtypeenum.md)的主题。  
  
## <a name="applies-to"></a>应用于  
 [记录集对象 (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) 的打开和关闭方法示例 ](./open-and-close-methods-example-vb.md)   
 [ (VBScript) 的打开和关闭方法示例 ](./open-and-close-methods-example-vbscript.md)   
 [打开和关闭方法示例 (VC + +) ](./open-and-close-methods-example-vc.md)   
 [ (VB 保存和打开方法示例) ](./save-and-open-methods-example-vb.md)   
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [ (ADO 记录的 Open 方法) ](./open-method-ado-record.md)   
 [ADO 流 (打开方法) ](./open-method-ado-stream.md)   
 [OpenSchema 方法](./openschema-method.md)   
 [Save 方法](./save-method.md)