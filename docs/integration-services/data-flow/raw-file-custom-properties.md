---
description: 原始文件自定义属性
title: 原始文件自定义属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 7e81f7e1-fac0-4b57-b145-8f1b9e4720bf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b1b7f0e1416b20ce641848abe47d3497f7c7b7f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92196392"
---
# <a name="raw-file-custom-properties"></a>原始文件自定义属性

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  **源自定义属性**  
  
 原始文件源具有自定义属性和所有数据流组件通用的属性。  
  
 下表介绍原始文件源的自定义属性。 所有属性均可读/写。  
  
|属性名称|数据类型|说明|  
|-------------------|---------------|-----------------|  
|AccessMode|Integer（枚举）|用来访问原始数据的模式。 可能的值为“文件名”(0) 和“来自变量的文件名”(1)。 默认值为“文件名”(0)。|  
|FileName|字符串|源文件的路径和文件名。|  
  
 原始文件源的输出和输出列没有自定义属性。  
  
 有关详细信息，请参阅 [Raw File Source](../../integration-services/data-flow/raw-file-source.md)。  
  
 **目标自定义属性**  
  
 原始文件目标具有自定义属性和所有数据流组件通用的属性。  
  
 下表介绍了原始文件目标的自定义属性。 所有属性均可读/写。  
  
|属性名称|数据类型|说明|  
|-------------------|---------------|-----------------|  
|AccessMode|Integer（枚举）|一个值，指定 FileName 属性是包含文件名还是包含变量（包含文件名）名。 选项为“文件名”(0) 和“来自变量的文件名”(1)。|  
|FileName|字符串|原始文件目标要写入的文件的名称。|  
|WriteOption|Integer（枚举）|一个指定原始文件目标是否删除具有相同名称的现有文件的值。 选项为“始终创建”(0)、“创建一次”(1)、“截断和追加”(3)、“追加”(2)。 此属性的默认值为“始终创建”(0)。|  
  
> [!NOTE]  
>  追加操作要求追加数据的元数据与文件中已有数据的元数据匹配。  
  
 原始文件目标的输入和输入列没有自定义属性。  
  
 有关详细信息，请参阅 [Raw File Destination](../../integration-services/data-flow/raw-file-destination.md)。  
  
## <a name="see-also"></a>另请参阅  
 [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
