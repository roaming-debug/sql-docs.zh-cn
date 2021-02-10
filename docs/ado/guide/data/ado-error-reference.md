---
description: ADO 错误参考
title: ADO 错误引用 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], number reference
- errors [ADO], ErrorValueEnum
- ErrorValueEnum enumeration [ADO]
ms.assetid: f653393e-d4b0-4c34-ad5f-2bdf56bc1305
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb6a2ede665c939aa5824810c692bdbe02df6c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028369"
---
# <a name="ado-errors"></a>ADO 错误
**ErrorValueEnum** 常数描述 ADO 错误值。 有关这些枚举常量（包括值）的完整列表，请参阅 [附录 B： ADO 错误](../appendixes/appendix-b-ado-errors.md)。 本部分将介绍一些更有趣的错误，并说明一些可能引发这些错误的特定情况，或解决问题的解决方案。 同时列出了 **ErrorValueEnum** 常量和短的十进制数字。

|数字|ErrorValueEnum 常量|说明/可能的原因|
|------------|-----------------------------|----------------------------------|
|**3000**|**adErrProviderFailed**|提供程序无法执行所请求的操作。|
|**3001**|**adErrInvalidArgument**|参数的类型错误，不在可接受的范围内，或者相互冲突。 此错误通常是由 SQL SELECT 语句中的打字错误引起的。 例如，拼写错误的字段名称或表名称可能会生成此错误。 当在 SELECT 语句中指定的字段或表在数据存储中不存在时，也会发生此错误。|
|**3002**|**adErrOpeningFile**|无法打开文件。 指定了拼写错误的文件名，或者文件已被移动、重命名或删除。 通过网络，驱动器可能暂时不可用，或者网络流量可能会阻止连接。|
|**3003**|**adErrReadFile**|无法读取文件。 错误地指定了该文件的名称，该文件可能已被移动或删除，或者该文件已损坏。|
|**3004**|**adErrWriteFile**|写入文件失败。 您可能已经关闭了某个文件，然后尝试写入该文件，或者该文件已损坏。 如果文件位于网络驱动器上，则暂时性的网络情况可能会阻止写入网络驱动器。|
|**3021**|**adErrNoCurrentRecord**|**BOF** 或 **EOF** 为 True，或当前记录已被删除。 请求的操作需要当前记录。<br /><br /> 尝试通过使用 " **查找** **" 或 "查找"** 将记录指针移到所需的记录来更新记录。 如果找不到该记录， **EOF** 将为 True。 此错误还可能在出现失败的 **AddNew** 或 **Delete** 之后出现，因为当这些方法失败时，没有当前记录。|
|**3219**|**adErrIllegalOperation**|不允许在此上下文中执行操作。|
|**3220**|**adErrCantChangeProvider**|提供的提供程序与已在使用的提供程序不同。|
|**3246**|**adErrInTransaction**|无法在事务中显式关闭 **连接** 对象。 无法关闭当前参与事务的 **记录集** 或 **连接** 对象。 在关闭对象之前调用 **RollbackTrans** 或 **CommitTrans** 。|
|**3251**|**adErrFeatureNotAvailable**|对象或提供程序无法执行所请求的操作。 某些操作依赖于特定的提供程序版本。|
|**3265**|**adErrItemNotFound**|在与请求的名称或序号相对应的集合中找不到项。 指定的字段或表名不正确。|
|**3367**|**adErrObjectInCollection**|对象已在集合中。 无法追加。 不能将一个对象添加到同一个集合两次。|
|**3420**|**adErrObjectNotSet**|对象不再有效。|
|**3421**|**adErrDataConversion**|应用程序为当前操作使用错误类型的值。 例如，你可能为需要流的操作提供了一个字符串。|
|**3704**|**adErrObjectClosed**|对象处于关闭状态时，不允许执行操作。 已关闭 **连接** 或 **记录集** 。 例如，另一个例程可能已关闭全局对象。 在尝试操作之前，可以通过检查 **状态** 属性来避免此错误。|
|**3705**|**adErrObjectOpen**|对象处于打开状态时，不允许执行操作。 无法打开打开的对象。 字段不能追加到打开的 **记录集**。|
|**3706**|**adErrProviderNotFound**|找不到提供程序。 它可能未正确安装。<br /><br /> 可能错误地指定了提供程序的名称，指定的访问接口可能未安装在执行您的代码的计算机上，或者安装可能已损坏。|
|**3707**|**adErrBoundToCommand**|不能更改 **记录集** 对象的 **ActiveConnection** 属性，该对象将 **命令** 对象作为其源。 应用程序尝试将新的 **连接** 对象分配给将 **命令** 对象作为其源的 **记录集**。|
|**3708**|**adErrInvalidParamInfo**|**参数** 对象定义不正确。 提供了不一致或不完整的信息。|
|**3709**|**adErrInvalidConnection**|无法使用连接来执行此操作。 它已关闭或在此上下文中无效。|
|**3710**|**adErrNotReentrant**|处理事件时无法执行操作。 操作不能在事件处理程序中执行，从而导致事件再次激发。 例如，不应从 **WillMove** 事件处理程序中调用导航方法。|
|**3711**|**adErrStillExecuting**|异步执行时，无法执行操作。|
|**3712**|**adErrOperationCancelled**|操作已被用户取消。 应用程序调用了 **CancelUpdate** 或 **CancelBatch** 方法，但当前操作已取消。|
|**3713**|**adErrStillConnecting**|异步连接时无法执行操作。|
|**3714**|**adErrInvalidTransaction**|协调事务无效或尚未启动。|
|**3715**|**adErrNotExecuting**|无法执行时无法执行操作。|
|**3716**|**adErrUnsafeOperation**|此计算机上的安全设置禁止访问另一个域中的数据源。|
|**3717**|**adWrnSecurityDialog**|仅限内部使用。 请勿使用。 出于完整性考虑，包含 (项。 此错误不应出现在你的代码中。 ) |
|**3718**|**adWrnSecurityDialogHeader**|仅限内部使用。 请勿使用。 出于完整性考虑， (项。 此错误不应出现在你的代码中。 ) |
|**3719**|**adErrIntegrityViolation**|数据值与字段的完整性约束冲突。 **字段** 的新值将导致重复键。 构成两个记录之间的关系的一方的值可能是不可更新的。|
|**3720**|**adErrPermissionDenied**|权限不足阻止写入字段。 连接字符串中指定的用户没有写入 **字段** 的适当权限。|
|**3721**|**adErrDataOverflow**|数据值太大，无法由字段数据类型表示。 为指定的字段分配的数字值太大。 例如，将 long 整数值分配给短整型字段。|
|**3722**|**adErrSchemaViolation**|数据值与字段的数据类型或约束冲突。 数据存储具有与 **字段** 值不同的验证约束。|
|**3723**|**adErrSignMismatch**|由于数据值已签名且提供程序使用的字段数据类型为无符号，导致转换失败。|
|**3724**|**adErrCantConvertvalue**|由于除符号不匹配或数据溢出之外的其他原因，无法转换数据值。 例如，转换可能会截断数据。|
|**3725**|**adErrCantCreate**|由于字段数据类型未知，或者提供程序没有足够的资源来执行操作，因此无法设置或检索数据值。|
|**3726**|**adErrColumnNotOnThisRow**|记录不包含该字段。 指定的字段名称不正确，或引用的字段不在当前记录的 **字段** 集合中。|
|**3727**|**adErrURLDoesNotExist**|源 URL 或目标 URL 的父项不存在。 源或目标 URL 中存在录入错误。 您可能会遇到这样的 `https://mysite/photo/myphoto.jpg` 情况 `https://mysite/photos/myphoto.jpg` 。 父 URL 中的录入错误 (在本例中， *照片* 而非 *照片*) 导致了错误。|
|**3728**|**adErrTreePermissionDenied**|权限不足以访问树或子树。 连接字符串中指定的用户没有适当的权限。|
|**3729**|**adErrInvalidURL**|URL 包含无效字符。 请确保正确键入了 URL。 URL 遵循注册到当前提供程序的方案 (例如，为 http) 注册了 Internet 发布提供程序。|
|**3730**|**adErrResourceLocked**|由指定的 URL 表示的对象已被一个或多个进程锁定。 请等待该进程完成，然后重试该操作。 您尝试访问的对象已被其他用户或应用程序中的其他进程锁定。 这很可能出现在多用户环境中。|
|**3731**|**adErrResourceExists**|无法执行复制操作。 由目标 URL 命名的对象已存在。 指定 **adCopyOverwrite** 以替换对象。 如果在将文件复制到目录中时没有指定 **adCopyOverwrite** ，则在尝试复制目标位置中已存在的项时，复制将失败。|
|**3732**|**adErrCannotComplete**|服务器无法完成此操作。 这可能是因为服务器正忙于处理其他操作，或者资源可能不足。|
|**3733**|**adErrVolumeNotFound**|提供程序找不到 URL 所指示的存储设备。 请确保正确键入了 URL。 存储设备的 URL 可能不正确，但出于其他原因，可能会发生此错误。 设备可能已脱机，或者大量网络流量可能会阻止连接。|
|**3734**|**adErrOutOfSpace**|无法执行操作。 提供程序无法获得足够的存储空间。 对于服务器上的临时文件，可能没有足够的 RAM 或硬盘空间。|
|**3735**|**adErrResourceOutOfScope**|源或目标 URL 超出了当前记录的范围。|
|**3736**|**adErrUnavailable**|操作无法完成，状态为 "不可用"。 此字段可能不可用或未尝试此操作。 其他用户可能已更改或删除了您尝试访问的字段。|
|**3737**|**adErrURLNamedRowDoesNotExist**|此 URL 命名的记录不存在。 尝试使用 **Record** 对象打开文件时，文件名或文件路径的拼写错误。|
|**3738**|**adErrDelResOutOfScope**|要删除的对象的 URL 在当前记录的范围之外。|
|**3747**|**adErrCatalogNotSet**|操作需要有效的 **ParentCatalog**。|
|**3748**|**adErrCantChangeConnection**|连接被拒绝。 您请求的新连接与已使用的连接具有不同的特征。|
|**3749**|**adErrFieldsUpdateFailed**|字段更新失败。 有关详细信息，请检查各个字段对象的 **Status** 属性。 在以下两种情况下可能会发生此错误：在更改 **字段** 对象的值的过程中，在将记录更改或添加到数据库时，更改 **字段** 对象本身的属性。<br /><br /> 由于当前记录中的某个字段出现问题， **记录** 或 **记录集** 更新失败。 枚举 **Fields** 集合并检查每个字段的 **Status** 属性，以确定问题的原因。|
|**3750**|**adErrDenyNotSupported**|提供程序不支持共享限制。 尝试限制文件共享，并且提供程序不支持此概念。|
|**3751**|**adErrDenyTypeNotSupported**|提供程序不支持所请求的共享限制种类。 尝试建立特定类型的文件共享限制，而提供程序不支持此限制。 请参阅提供程序的文档，以确定支持的文件共享限制。|