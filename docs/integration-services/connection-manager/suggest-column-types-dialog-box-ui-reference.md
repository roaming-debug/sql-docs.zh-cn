---
description: “提供列类型建议”对话框 UI 参考
title: “提供列类型建议”对话框 UI 参考 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.suggestdatatypes.f1
ms.assetid: 8d5652e0-cf57-483f-828b-10f00c08418b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d1c2bfd93e046141889d9cfa5c1c438462efa92d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425959"
---
# <a name="suggest-column-types-dialog-box-ui-reference"></a>“提供列类型建议”对话框 UI 参考

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“提供列类型建议”** 对话框，根据文件内容抽样指定平面文件连接管理器中的列的数据类型和长度。  
  
 若要了解有关 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]使用的数据类型的详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="options"></a>选项  
 **行数**  
 键入或选择算法使用的示例中的行数。  
  
 **建议使用最小整数数据类型**  
 清除此复选框可以略过评估。 选中此复选框之后，将为包含整型数字数据的列确定可能的最小整型数据类型。  
  
 **建议使用最小的实型数据类型**  
 清除此复选框可以略过评估。 选中此复选框之后，将确定包含实型数字数据的列是否可以使用较小的实型数据类型 DT_R4。  
  
 **使用下列值标识布尔值列**  
 键入要用作布尔值 True 和 False 的两个值。 两个值必须用逗号分隔，并且第一个值代表 True。  
  
 **填充字符串列**  
 选中此复选框，可以启用字符串填充。  
  
 **填充百分比**  
 对于字符数据类型的列，键入或选择列长度增加的百分比。 百分比必须为整数。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 错误和消息引用](../../integration-services/integration-services-error-and-message-reference.md)   
 [平面文件连接管理器](../../integration-services/connection-manager/flat-file-connection-manager.md)  
  
  
