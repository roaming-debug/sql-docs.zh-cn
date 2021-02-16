---
description: 在运行时提供 OData 源查询
title: 在运行时提供 OData 源查询 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: bcbba7f4-6e5d-46e6-a73a-3f17d3ff376a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3cddac6ff27883aac6840f9ce3c04ba4441d1e71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343241"
---
# <a name="provide-an-odata-source-query-at-runtime"></a>在运行时提供 OData 源查询

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 可以通过向数据流任务的“[OData 源].[查询]”属性添加表达式，在运行时修改 OData 源查询   。  
  
 返回的列必须与设计时返回的列相同。否则，执行包时会出错。 请务必在使用 $select 查询选项时指定相同的列（采用相同顺序）。 使用 $select 选项的更安全替代方法是直接从源组件 UI 中取消选择不需要的列。  
  
 可通过几种不同的方式在运行时动态设置查询值。 下面是一些较常用的方法。  
  
## <a name="provide-the-query-as-a-parameter"></a>以参数的形式提供查询  
 以下过程介绍如何将 OData 源组件使用的查询公开为包的参数。  
  
1.  右键单击“数据流任务”并选择“参数化…”选项   。  
  
2.  在“参数化”对话框中，针对“属性”选择 [\<Name of the OData Source Component>].[Query]  。  
  
3.  选择是 **“创建新参数”** 还是 **“使用现有参数”** 。  
  
4.  如果选择“创建新参数”  ：  
  
    1.  为参数输入 **“名称”** 和 **“说明”** 。  
  
    2.  为参数指定默认 **“值”** 。  
  
    3.  为参数指定“范围”  （“包”  或“项目”  ）。  
  
    4.  指定参数是否为 **“必需”**  
  
5.  单击 **“确定”** 关闭对话框。  
  
## <a name="provide-the-query-with-an-expression"></a>使用表达式提供查询
 要在运行时动态构造查询字符串时，可使用此方法。
  
1.  选择包含“OData 源”的“数据流任务”   。  
  
2.  在 **“属性”** 窗口中，突出显示 **“表达式”** 属性。  
  
3.  单击 …（省略号）按钮以显示“属性表达式编辑器”  。  
  
4.  选择“[OData 源].[查询]”  属性。  
  
5.  单击“表达式”的 …（省略号）按钮  。  
  
6.  输入 **“表达式”** 。  
  
7.  单击“确定”。  
  
> [!NOTE]  
> 当使用此方法的时候，需要确保设置的值为正确编码的 URL。 从用户输入接收值时（例如，通过参数设置各个查询选项值），必须确保值已验证，以避免潜在的 SQL 注入类型攻击。  
  
  
