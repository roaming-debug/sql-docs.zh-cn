---
description: ErrorValueEnum
title: ErrorValueEnum |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ErrorValueEnum
helpviewer_keywords:
- ErrorValueEnum enumeration [ADO]
ms.assetid: 9469ba3a-5e4f-4a10-bbb8-a51a6c9660ea
author: rothja
ms.author: jroth
ms.openlocfilehash: 519b505c0fcb985e6f0ca9cfd04f890114b3fdb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025064"
---
# <a name="errorvalueenum"></a>ErrorValueEnum
指定 ADO 运行时错误的类型。  
  
 列出了三种形式的错误号：  
  
-   正十进制-十进制格式的完整数字的低2字节。 此数字显示在默认 Visual Basic 错误消息 "对话框中。 例如，运行时错误 "3707"。  
  
-   负小数点-完整错误号的十进制转换。  
  
-   完整错误号 Hexadecimal-The 十六进制表示形式。 Windows 设备代码采用第四位数。 ADO 错误 *号的工具代码是。* 例如 ***： 0x800 0E7B***。  
  
> [!NOTE]
>  可能会向 ADO 应用程序传递 OLE DB 错误。 通常，可以通过 Windows 设备代码 *4* 来标识这些。 例如，0x800 ***4***。  
  
|返回的常量|Value|说明|  
|--------------|-----------|-----------------|  
|**adErrBoundToCommand**|3707-2146824581 0x800A0E7B|无法更改将 **命令** 对象作为其源的 **记录集** 对象的 **ActiveConnection** 属性。|  
|**adErrCannotComplete**|3732-2146824556 0x800A0E94|服务器无法完成此操作。|  
|**adErrCantChangeConnection**|3748-2146824540 0x800A0EA4|连接被拒绝。 你请求的新连接具有不同于已使用的的特征。|  
|**adErrCantChangeProvider**|3220-2146825068 0X800A0C94|提供的提供程序与已使用的提供程序不同。|  
|**adErrCantConvertvalue**|3724-2146824564 0x800A0E8C|由于除符号不匹配或数据溢出之外的其他原因，无法转换数据值。 例如，转换可能会截断数据。|  
|**adErrCantCreate**|3725-2146824563 0x800A0E8D|由于字段数据类型未知，或者提供程序没有足够的资源来执行操作，因此无法设置或检索数据值。|  
|**adErrCatalogNotSet**|3747-2146824541 0x800A0EA3|操作需要有效的 **ParentCatalog**。|  
|**adErrColumnNotOnThisRow**|3726-2146824562 0x800A0E8E|记录不包含该字段。|  
|**adErrDataConversion**|3421-2146824867 0x800A0D5D|应用程序为当前操作使用错误类型的值。|  
|**adErrDataOverflow**|3721-2146824567 0x800A0E89|数据值太大，无法由字段数据类型表示。|  
|**adErrDelResOutOfScope**|3738-2146824550 0x800A0E9A|要删除的对象的 URL 在当前记录的范围之外。|  
|**adErrDenyNotSupported**|3750-2146824538 0x800A0EA6|提供程序不支持共享限制。|  
|**adErrDenyTypeNotSupported**|3751-2146824537 0x800A0EA7|提供程序不支持所请求的共享限制种类。|  
|**adErrFeatureNotAvailable**|3251-2146825037 0x800A0CB3|对象或提供程序无法执行请求的操作。|  
|**adErrFieldsUpdateFailed**|3749-2146824539 0x800A0EA5|字段更新失败。 有关详细信息，请检查各个字段对象的 **Status** 属性。|  
|**adErrIllegalOperation**|3219-2146825069 0x800A0C93|不允许在此上下文中执行操作。|  
|**adErrIntegrityViolation**|3719-2146824569 0x800A0E87|数据值与字段的完整性约束冲突。|  
|**adErrInTransaction**|3246-2146825042 0x800A0CAE|无法在事务中显式关闭 **连接** 对象。|  
|**adErrInvalidArgument**|3001-2146825287 0x800A0BB9|参数的类型错误，不在可接受的范围内，或者相互冲突。|  
|**adErrInvalidConnection**|3709-2146824579 0x800A0E7D|无法使用连接来执行此操作。 它已关闭或在此上下文中无效。|  
|**adErrInvalidParamInfo**|3708-2146824580 0x800A0E7C|**参数** 对象定义不正确。 提供了不一致或不完整的信息。|  
|**adErrInvalidTransaction**|3714-2146824574 0x800A0E82|协调事务无效或尚未启动。|  
|**adErrInvalidURL**|3729-2146824559 0x800A0E91|URL 包含无效字符。 请确保正确键入了 URL。|  
|**adErrItemNotFound**|3265-2146825023 0x800A0CC1|在与请求的名称或序号相对应的集合中找不到项。|  
|**adErrNoCurrentRecord**|3021-2146825267 0x800A0BCD|**BOF** 或 **EOF** 为 True，或当前记录已被删除。 请求的操作需要当前记录。|  
|**adErrNotExecuting**|3715-2146824573 0x800A0E83|无法执行时无法执行操作。|  
|**adErrNotReentrant**|3710-2146824578 0x800A0E7E|处理事件时无法执行操作。|  
|**adErrObjectClosed**|3704-2146824584 0x800A0E78|对象处于关闭状态时，不允许执行操作。|  
|**adErrObjectInCollection**|3367-2146824921 0x800A0D27|对象已在集合中。 无法追加。|  
|**adErrObjectNotSet**|3420-2146824868 0x800A0D5C|对象不再有效。|  
|**adErrObjectOpen**|3705-2146824583 0x800A0E79|对象处于打开状态时，不允许执行操作。|  
|**adErrOpeningFile**|3002-2146825286 0x800A0BBA|无法打开文件。|  
|**adErrOperationCancelled**|3712-2146824576 0x800A0E80|操作已被用户取消。|  
|**adErrOutOfSpace**|3734-2146824554 0x800A0E96|无法执行操作。 提供程序无法获得足够的存储空间。|  
|**adErrPermissionDenied**|3720-2146824568 0x800A0E88|权限不足阻止写入字段。|  
|**adErrProviderFailed**|3000-2146825288 0x800A0BB8|提供程序未执行请求的操作。|  
|**adErrProviderNotFound**|3706-2146824582 0x800A0E7A|找不到提供程序。 它可能未正确安装。|  
|**adErrReadFile**|3003-2146825285 0x800A0BBB|无法读取文件。|  
|**adErrResourceExists**|3731-2146824557 0x800A0E93|无法执行复制操作。 由目标 URL 命名的对象已存在。 指定 **adCopyOverwrite** 以替换对象。|  
|**adErrResourceLocked**|3730-2146824558 0x800A0E92|由指定的 URL 表示的对象已被一个或多个进程锁定。 请等待该进程完成，然后重试该操作。|  
|**adErrResourceOutOfScope**|3735-2146824553 0x800A0E97|源或目标 URL 超出了当前记录的范围。|  
|**adErrSchemaViolation**|3722-2146824566 0x800A0E8A|数据值与字段的数据类型或约束冲突。|  
|**adErrSignMismatch**|3723-2146824565 0x800A0E8B|由于数据值已签名且提供程序使用的字段数据类型为无符号，导致转换失败。|  
|**adErrStillConnecting**|3713-2146824575 0x800A0E81|异步连接时无法执行操作。|  
|**adErrStillExecuting**|3711-2146824577 0x800A0E7F|异步执行时，无法执行操作。|  
|**adErrTreePermissionDenied**|3728-2146824560 0x800A0E90|权限不足以访问树或子树。|  
|**adErrUnavailable**|3736-2146824552 0x800A0E98|操作未完成，状态为 "不可用"。 此字段可能不可用或未尝试此操作。|  
|**adErrUnsafeOperation**|3716-2146824572 0x800A0E84|此计算机上的安全设置阻止访问另一个域中的数据源。|  
|**adErrURLDoesNotExist**|3727-2146824561 0x800A0E8F|源 URL 或目标 URL 的父项不存在。|  
|**adErrURLNamedRowDoesNotExist**|3737-2146824551 0x800A0E99|此 URL 命名的记录不存在。|  
|**adErrVolumeNotFound**|3733-2146824555 0x800A0E95|提供程序找不到 URL 所指示的存储设备。 请确保正确键入了 URL。|  
|**adErrWriteFile**|3004-2146825284 0x800A0BBC|写入文件失败。|  
|**adWrnSecurityDialog**|3717-2146824571 0x800A0E85|仅限内部使用。 请勿使用。|  
|**adWrnSecurityDialogHeader**|3718-2146824570 0x800A0E86|仅限内部使用。 请勿使用。|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC 等效项  
 Package： **.com. 数据**  
  
 仅定义了 ADO/WFC 等效项的以下子集。  
  
|返回的常量|  
|--------------|  
|AdoEnums.ErrorValue.BOUNDTOCOMMAND|  
|AdoEnums.ErrorValue.DATACONVERSION|  
|AdoEnums.ErrorValue.FEATURENOTAVAILABLE|  
|AdoEnums.ErrorValue.ILLEGALOPERATION|  
|AdoEnums.ErrorValue.INTRANSACTION|  
|AdoEnums. ErrorValue. INVALIDARGUMENT|  
|AdoEnums.ErrorValue.INVALIDCONNECTION|  
|AdoEnums.ErrorValue.INVALIDPARAMINFO|  
|AdoEnums.ErrorValue.ITEMNOTFOUND|  
|AdoEnums.ErrorValue.NOCURRENTRECORD|  
|AdoEnums.ErrorValue.NOTEXECUTING|  
|AdoEnums.ErrorValue.NOTREENTRANT|  
|AdoEnums.ErrorValue.OBJECTCLOSED|  
|AdoEnums.ErrorValue.OBJECTINCOLLECTION|  
|AdoEnums.ErrorValue.OBJECTNOTSET|  
|AdoEnums.ErrorValue.OBJECTOPEN|  
|AdoEnums. ErrorValue. OPERATIONCANCELLED|  
|AdoEnums.ErrorValue.PROVIDERNOTFOUND|  
|AdoEnums.ErrorValue.STILLCONNECTING|  
|AdoEnums.ErrorValue.STILLEXECUTING|  
|AdoEnums.ErrorValue.UNSAFEOPERATION|  
  
## <a name="applies-to"></a>应用于  
 [Number 属性 (ADO)](../../../ado/reference/ado-api/number-property-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO 错误代码](../../../ado/guide/appendixes/ado-error-codes.md)
