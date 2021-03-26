---
description: 了解可用于进行应用程序查询的不同游标类型以及这些类型的使用场景。
title: 了解游标类型
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4f4d3db7-4f76-450d-ab63-141237a4f034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1eed3881d9c7162b370b17a5dacb1aa4ea87f956
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793055"
---
# <a name="understanding-cursor-types"></a>了解游标类型

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

关系数据库中的操作会对整个行集起作用。 由 SELECT 语句返回的行集包括满足该语句的 WHERE 子句中条件的所有行。 这种由语句返回的完整行集称为结果集。 应用程序并不总能将整个结果集作为一个单元来有效地处理。 这些应用程序需要一种机制以便每次处理一行或一部分行。 游标就是提供这种机制的对结果集的一种扩展。

游标通过执行以下事项来扩展结果集处理：

- 允许定位在结果集的特定行。
- 从结果集的当前位置检索一行或一部分行。
- 支持对结果集中当前位置的行进行数据修改。
- 为由其他用户对显示在结果集中的数据库数据所做的更改提供不同级别的可见性支持。

> [!NOTE]
> 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 游标类型的完整说明，请参阅[游标类型](../../relational-databases/cursors.md#type-of-cursors)。

JDBC 规范支持的只进和可滚动游标对其他作业所做的更改可能敏感，也可能不敏感，而且可以是只读游标或可更新游标。 这一功能由 [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] 的 [SQLServerResultSet](reference/sqlserverresultset-class.md) 类提供。

## <a name="remarks"></a>注解

JDBC 驱动程序支持以下游标类型：

|结果集<br /><br /> （游标）类型|SQL Server 游标类型|特征|select<br /><br /> 方法|响应<br /><br /> 缓冲|说明|
|------------------------------------|----------------------------|---------------------|-----------------------|----------------------------|-----------------|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|不可用|只进，只读|直通|full|应用程序必须（向前）经过结果集一次。 此经过操作是默认行为，其行为与 TYPE_SS_DIRECT_FORWARD_ONLY 游标相同。 在语句执行期间，驱动程序将整个结果集从服务器读入内存中。|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|不可用|只进，只读|直通|自适应|应用程序必须（向前）经过结果集一次。 其行为与 TYPE_SS_DIRECT_FORWARD_ONLY 游标的行为相同。 当应用程序请求行时，驱动程序从服务器读取对应的行，这样可以最大限度地减少客户端内存占用。|
|TYPE_FORWARD_ONLY (CONCUR_READ_ONLY)|快进|只进，只读|cursor|不可用|应用程序必须通过使用服务器游标在结果集中进行一次（前进）传递。 其行为与 TYPE_SS_SERVER_CURSOR_FORWARD_ONLY 游标的行为相同。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_FORWARD_ONLY (CONCUR_UPDATABLE)|动态（只进）|只进，可更新|空值|不可用|应用程序必须（向前）经过结果集一次才能更新一行或多行。<br /><br /> 从提取大小指定的服务器中分块检索行。<br /><br /> 默认情况下，当应用程序调用 [SQLServerResultSet](reference/setfetchsize-method-sqlserverresultset.md) 对象的 [setFetchSize](reference/sqlserverresultset-class.md) 方法时，提取大小是固定的。<br /><br /> **注意：** JDBC 驱动程序提供一项自适应缓冲功能，使你能够在应用程序需要时从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中检索语句执行结果，而不是一次性检索全部结果。 例如，如果应用程序需要检索一个因为太大而无法完全容纳于应用程序内存中的大型数据块，则自适应缓冲使客户端应用程序可以检索诸如流这样的值。 驱动程序的默认行为是“adaptive”。 但是，若要为只进的可更新结果集获取自适应缓冲，应用程序必须通过提供字符串值“adaptive”来显式调用 [SQLServerStatement](reference/sqlserverstatement-class.md) 对象的 [setResponseBuffering](reference/setresponsebuffering-method-sqlserverstatement.md) 方法。 有关示例代码，请参阅[更新大型数据示例](updating-large-data-sample.md)。|
|TYPE_SCROLL_INSENSITIVE|静态|可滚动，不可更新。<br /><br /> 外部行更新、插入和删除不可见。|空值|不可用|应用程序需要数据库快照。 结果集不可更新。 仅支持 CONCUR_READ_ONLY。  使用此游标类型时，所有其他并发类型都将导致异常。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SCROLL_SENSITIVE<br /><br /> (CONCUR_READ_ONLY)|Keyset|可滚动，只读。 外部行更新可见，删除显示为缺失数据。<br /><br /> 外部行插入不可见。|空值|不可用|应用程序必须只看到现有行的已更改数据。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SCROLL_SENSITIVE<br /><br /> （CONCUR_UPDATABLE、CONCUR_SS_SCROLL_LOCKS、CONCUR_SS_OPTIMISTIC_CC、CONCUR_SS_OPTIMISTIC_CCVAL）|Keyset|可滚动，可更新。<br /><br /> 外部和内部行更新可见，删除显示为缺失数据；插入不可见。|空值|不可用|应用程序可以使用 ResultSet 对象更改现有行中的数据。 应用程序还必须能够从 ResultSet 对象之外看到其他人员对行所做的更改。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SS_DIRECT_FORWARD_ONLY|不可用|只进，只读|不可用|full 或 adaptive|整数值 = 2003。 提供一个完全缓冲的只读客户端游标。 不创建服务器游标。<br /><br /> 仅支持 CONCUR_READ_ONLY 并发类型。 与此游标类型一起使用时，所有其他并发类型都将导致异常。|
|TYPE_SS_SERVER_CURSOR_FORWARD_ONLY|快进|只进|空值|不可用|整数值 = 2004。 快速，使用服务器游标访问所有数据。 使用 CONCUR_UPDATABLE 并发类型时可更新。<br /><br /> 从提取大小指定的服务器中分块检索行。<br /><br /> 对于这种情况，为了获得自适应缓冲，应用程序必须通过提供一个 字符串值“adaptive”来显式调用 [SQLServerStatement](reference/sqlserverstatement-class.md) 对象的 [setResponseBuffering](reference/setresponsebuffering-method-sqlserverstatement.md) 方法。 有关示例代码，请参阅[更新大型数据示例](updating-large-data-sample.md)。|
|TYPE_SS_SCROLL_STATIC|静态|不反映其他用户的更新。|空值|不可用|整数值 = 1004。 应用程序需要数据库快照。 此选项是 JDBC TYPE_SCROLL_INSENSITIVE 特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的同义词，并且具有相同的并发设置行为。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SS_SCROLL_KEYSET<br /><br /> (CONCUR_READ_ONLY)|Keyset|可滚动，只读。 外部行更新可见，删除显示为缺失数据。<br /><br /> 外部行插入不可见。|空值|不可用|整数值 = 1005。 应用程序必须只看到现有行的已更改数据。 此选项是 JDBC TYPE_SCROLL_SENSITIVE 特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的同义词，并且具有相同的并发设置行为。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SS_SCROLL_KEYSET<br /><br /> （CONCUR_UPDATABLE、CONCUR_SS_SCROLL_LOCKS、CONCUR_SS_OPTIMISTIC_CC、CONCUR_SS_OPTIMISTIC_CCVAL）|Keyset|可滚动，可更新。<br /><br /> 外部和内部行更新可见，删除显示为缺失数据；插入不可见。|空值|不可用|整数值 = 1005。 应用程序必须更改现有行的数据或看到现有行的已更改数据。 此选项是 JDBC TYPE_SCROLL_SENSITIVE 特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的同义词，并且具有相同的并发设置行为。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SS_SCROLL_DYNAMIC<br /><br /> (CONCUR_READ_ONLY)|动态|可滚动，只读。<br /><br /> 外部行更新和插入可见，删除显示为当前提取缓冲区中的临时缺失数据。|空值|不可用|整数值 = 1006。 应用程序在游标的生存期内必须看到现有行的已更改数据并看到已插入和已删除的行。<br /><br /> 从提取大小指定的服务器中分块检索行。|
|TYPE_SS_SCROLL_DYNAMIC<br /><br /> （CONCUR_UPDATABLE、CONCUR_SS_SCROLL_LOCKS、CONCUR_SS_OPTIMISTIC_CC、CONCUR_SS_OPTIMISTIC_CCVAL）|动态|可滚动，可更新。<br /><br /> 外部和内部行更新和插入可见，删除显示为当前提取缓冲区中的临时缺失数据。|空值|不可用|整数值 = 1006。 应用程序可以使用 ResultSet 对象更改现有行的数据或者插入或删除行。 应用程序还必须能够从 ResultSet 对象之外看到其他人员所做的更改、插入和删除。<br /><br /> 从提取大小指定的服务器中分块检索行。|

## <a name="cursor-positioning"></a>游标定位

TYPE_FORWARD_ONLY、TYPE_SS_DIRECT_FORWARD_ONLY 和 TYPE_SS_SERVER_CURSOR_FORWARD_ONLY 游标仅支持 [next](../../connect/jdbc/reference/next-method-sqlserverresultset.md) 定位方法。

TYPE_SS_SCROLL_DYNAMIC 游标不支持 [absolute](reference/absolute-method-sqlserverresultset.md) 和 [getRow](reference/getrow-method-sqlserverresultset.md) 方法。 absolute 方法可能近似等效于针对动态游标组合调用 [first](reference/first-method-sqlserverresultset.md) 和 [relative](reference/relative-method-sqlserverresultset.md) 方法。

getRow 方法仅受 TYPE_FORWARD_ONLY、TYPE_SS_DIRECT_FORWARD_ONLY、TYPE_SS_SERVER_CURSOR_FORWARD_ONLY、TYPE_SS_SCROLL_KEYSET 和 TYPE_SS_SCROLL_STATIC 游标的支持。 带有所有只进游标类型的 getRow 方法返回迄今为止通过游标读取的行数。

> [!NOTE]
> 当应用程序进行不受支持的游标定位调用或对 getRow 方法进行不受支持的调用时，将引发异常并显示“此游标类型不支持所请求的操作”的消息。

仅 TYPE_SS_SCROLL_KEYSET 和等效的 TYPE_SCROLL_SENSITIVE 游标才显示已删除的行。 如果游标定位在已删除的行上，则列值不可用并且 [rowDeleted](reference/rowdeleted-method-sqlserverresultset.md) 方法会返回“true”。 对 get\<Type> 方法进行调用将引发异常并显示“无法从已删除行获取值”的消息。 无法更新已删除的行。 如果尝试对已删除的行调用 update\<Type> 方法，将引发异常并显示“无法更新已删除的行”的消息。 TYPE_SS_SCROLL_DYNAMIC 游标具有这种相同的行为，直到将此游标移出当前提取缓冲区之外。

前进游标和动态游标以类似的方式显示已删除的行，但仅限仍可在提取缓冲区中访问游标的情况。 对于前进游标，此行为相当简单。 对于动态游标，当提取大小大于 1 时，此行为就变得较复杂了。 应用程序可以在由提取缓冲区定义的窗口内前后移动游标，但是，当离开在其中更新已删除行的原始提取缓冲区时，已删除的行将消失。 如果应用程序并不希望通过使用动态游标来查看临时删除的行，则应使用 Fetch Relative (0)。

如果 TYPE_SS_SCROLL_KEYSET 或 TYPE_SCROLL_SENSITIVE 游标行的键值使用游标进行更新，行会保留它在结果集中的原始位置，无论更新后的行是否符合游标的选择条件。 如果行在游标之外进行更新，已删除的行会出现在此行的原始位置，但仅当包含新键值的另一行出现在游标中，但已遭删除时，此行才会出现在游标中。

对于动态游标，已更新的行将保留其在提取缓冲区的位置，直到离开由提取缓冲区定义的窗口。 已更新的行之后可能重新出现在结果集内的不同位置，或者可能完全消失。 必须避免结果集中出现临时不一致的应用程序所使用的提取大小应为 1（对于 CONCUR_SS_SCROLL_LOCKS 并发机制，默认值为 8 行；对于其他并发机制，则为 128 行）。

## <a name="cursor-conversion"></a>游标转换

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 有时选择实现的游标类型与所请求的游标类型不同，这称为隐式游标转换（或游标降级）。

对于 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]，当你通过 ResultSet.TYPE_SCROLL_SENSITIVE 和 ResultSet.CONCUR_UPDATABLE 结果集更新数据时，将引发异常并显示消息“游标是只读的”。 发生此异常的原因是 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 已针对该结果集执行了隐式游标转换，而没有返回所请求的可更新游标。

若要解决此问题，可以采用以下两种解决方案之一：

- 确保基础表具有主键
- 创建语句时，使用 [SQLServerResultSet.TYPE_SS_SCROLL_DYNAMIC](reference/type-ss-scroll-dynamic-field-sqlserverresultset.md)，而不使用 ResultSet.TYPE_SCROLL_SENSITIVE。

## <a name="cursor-updating"></a>游标更新

对于游标而言，如果游标类型和并发支持更新，则支持对游标进行就地更新。 如果游标未定位在结果集中的可更新行上（get\<Type> 方法调用未成功），则调用 update\<Type> 方法将引发异常并显示“结果集没有当前行”的消息。 JDBC 规范规定，对类型为 CONCUR_READ_ONLY 的游标的列调用 update 方法时，将引发异常。 在由于乐观并发冲突（例如竞争性的更新或删除）等此类原因而导致无法更新行的情况下，在调用 [insertRow](reference/insertrow-method-sqlserverresultset.md)、[updateRow](reference/updaterow-method-sqlserverresultset.md) 或 [deleteRow](reference/deleterow-method-sqlserverresultset.md) 之前可能不会引发异常。

在调用 update\<Type> 之后，一直到调用了 updateRow 或 [cancelRowUpdates](reference/cancelrowupdates-method-sqlserverresultset.md) 之前，get\<Type> 都无法访问受影响的列。 此行为将避免以下问题：使用与服务器返回的类型不同的类型更新列，而随后调用的 getter 可能会引起给出不准确结果的客户端类型转换。 调用 get\<Type> 将引发异常并显示“在调用 updateRow() 或 cancelRowUpdates() 之前，无法访问已更新列”的消息。

> [!NOTE]
> 如果在未更新任何列时调用 updateRow 方法，JDBC 驱动程序将引发异常并显示“在未更新任何列时调用了 updateRow()”的消息

在调用了 [moveToInsertRow](reference/movetoinsertrow-method-sqlserverresultset.md) 之后，如果对结果集调用了除 get\<Type>、update\<Type>、insertRow 和游标定位方法（包括 [moveToCurrentRow](reference/movetocurrentrow-method-sqlserverresultset.md)）之外的任何其他方法，则将引发异常。 moveToInsertRow 方法有效地将结果集置于插入模式，而游标定位方法会终止插入模式。 相对的游标定位调用会将游标在调用 moveToInsertRow 之前它所处的相对位置进行移动。 在游标定位调用之后，最终的目标游标位置将成为新的游标位置。

如果在插入模式下进行的游标定位调用未成功，则调用失败之后的游标位置为调用 moveToInsertRow 之前的原始游标位置。 如果 insertRow 失败，游标将保持在插入行上，并且游标将保持在插入模式下。

位于插入行中的列最初处于未初始化状态。 调用 update\<Type> 方法可将列状态设置为已初始化。 对未初始化的列调用 get\<Type> 方法将引发异常。 调用 insertRow 方法会将位于插入行中的所有列返回到未初始化状态。

如果在调用 insertRow 方法时任何列均未初始化，则将插入列的默认值。 如果没有默认值而列可为 Null，则插入 NULL。 如果没有默认值且列不可为 Null，服务器将返回错误，此时将引发异常。

> [!NOTE]
> 在插入模式下，调用 getRow 方法将返回 0。
>
> JDBC 驱动程序不支持定位更新或删除。 根据 JDBC 规范，调用 [setCursorName](reference/setcursorname-method-sqlserverstatement.md) 方法将不起任何作用，而调用 [getCursorName](reference/getcursorname-method-sqlserverresultset.md) 方法将引发异常。
>
> 只读和静态游标始终无法更新。
>
> SQL Server 将服务器游标限制到单一结果集中。 如果批处理或存储过程包含多条语句，则必须使用只进只读客户端游标。

## <a name="see-also"></a>另请参阅

[通过 JDBC 驱动程序管理结果集](managing-result-sets-with-the-jdbc-driver.md)  
