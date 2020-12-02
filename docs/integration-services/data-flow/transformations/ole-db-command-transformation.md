---
description: OLE DB 命令转换
title: OLE DB 命令转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbcommandtrans.f1
helpviewer_keywords:
- statements [Integration Services]
- OLE DB Command transformation
ms.assetid: baa6735c-5acf-4759-b077-1216aca16c6c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5cde3dc6eb7805f412ed7e820fb4946cf0c20c1a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88477622"
---
# <a name="ole-db-command-transformation"></a>OLE DB 命令转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  OLE DB 命令转换对数据流中的每一行运行一条 SQL 语句。 例如，您可以运行 SQL 语句以在数据库表中插入、更新或删除行。  
  
 可以按下列方式配置 OLE DB 命令转换：  
  
-   提供转换为每一行所运行的 SQL 语句。  
  
-   指定一个秒数，到此秒数该 SQL 语句即告超时。  
  
-   指定默认的代码页。  
  
 通常，所运行的 SQL 语句会包含一些参数。 参数值存储在转换输入的外部列中，将输入列映射到外部列便可将输入列映射到参数。 例如，若要按 **ProductKey** 列中的值在 **DimProduct** 表中查找行，然后删除这些行，可将名为 **Param_0** 的外部列映射到名为 **ProductKey** 的输入列，然后再运行 SQL 语句 `DELETE FROM DimProduct WHERE ProductKey = ?`。 OLE DB 命令转换提供参数的名称，这些参数名不可修改。 参数名的形式为 **Param_0**、 **Param_1** 等，依此类推。  
  
 如果使用 **“高级编辑器”** 对话框配置 OLE DB 命令转换，单击 **“刷新”** 按钮便可将 SQL 语句中的参数自动映射到转换输入中的外部列，并定义每个参数的特征。 但是，如果 OLE DB 命令转换所使用的 OLE DB 访问接口不支持从参数派生参数信息，则必须手动配置外部列。 这意味着对每个参数，必须在转换的外部输入中添加一列，用类似 **Param_0** 的名称更新列名，指定 DBParamInfoFlags 属性的值，并将包含参数值的输入列映射到这些外部列。  
  
 DBParamInfoFlags 的值表示参数的特征。 例如，值 **1** 指定参数为输入参数，而值 **65** 则指定参数为输入参数，且可能包含空值。 该值必须与 OLE DB DBPARAMFLAGSENUM 枚举中包含的值相匹配。 有关详细信息，请参阅 OLE DB 参考文档。  
  
 OLE DB 命令转换包括 **SQLCommand** 自定义属性。 加载包时，可以通过属性表达式更新此属性。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)、[在包中使用属性表达式](../../../integration-services/expressions/use-property-expressions-in-packages.md)和[转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
 此转换有一个输入、一个常规输出和一个错误输出。  
  
## <a name="logging"></a>日志记录  
 可以记录 OLE DB 命令转换对外部数据访问接口所做的调用。 利用此日志记录功能，可以排除 OLE DB 命令转换对外部数据源执行的连接和命令中发生的故障。 若要记录 OLE DB 命令转换对外部数据访问接口所做的调用，请在包级别启用包日志记录并选择 **“诊断”** 事件。 有关详细信息，请参阅 [包执行的疑难解答工具](../../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)。  
  
## <a name="related-tasks"></a>Related Tasks  
 可通过使用 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或对象模型配置转换。 有关以编程方式配置此转换的详细信息，请参阅开发人员指南。  
  
## <a name="configure-the-ole-db-command-transformation"></a>配置 OLE DB 命令转换
  若要添加和配置 OLE DB 命令转换，包必须已包含至少一个数据流任务和一个源（如平面文件源或 OLE DB 源）。 这种转换通常用于运行参数化查询。  
  
#### <a name="to-configure-the-ole-db-command-transformation"></a>配置 OLE DB 命令转换  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击 **“数据流”** 选项卡，然后从 **“工具箱”** 中将 OLE DB 命令转换拖动到设计图面。  
  
4.  将连接线（绿色或红色箭头）从数据源或前一个转换拖动到 OLE DB 命令转换，从而将 OLE DB 命令转换连接到数据流。  
  
5.  右键单击组件，并选择“编辑高级编辑器”或“显示高级编辑器”。  
  
6.  在 **“连接管理器”** 选项卡上，从 **“连接管理器”** 列表中选择 OLE DB 连接管理器。 有关详细信息，请参阅 [OLE DB Connection Manager](../../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
7.  单击“组件属性”选项卡，并单击“SqlCommand”框中的省略号按钮 (…)。  
  
8.  在“字符串值编辑器”中，键入参数化 SQL 语句，并且使用问号 (?) 作为每个参数的参数标记。  
  
9. 单击“刷新”。 单击“刷新”时，转换将为 External Columns 集合中的每一个参数都创建一列，并设置 DBParamInfoFlags 属性。  
  
10. 单击 **“输入属性和输出属性”** 选项卡。  
  
11. 展开 **“OLE DB 命令输入”**，再展开 **“外部列”**。  
  
12. 验证 **“外部列”** 是否为 SQL 语句中的每个参数列出了一列。 列的名称是 **Param_0**、 **Param_1**，以此类推。  
  
     不应更改列名称。 如果更改列名称， [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 会生成有关 OLE DB 命令转换的验证错误。  
  
     此外，不应更改数据类型。 每列的 DataType 属性均设置为正确的数据类型。  
  
13. 如果 **“外部列”** 没有列出任何列，则您必须手动添加这些列。  
  
    -   对 SQL 语句中的每一个参数，单击一次 **“添加列”** 。  
  
    -   将列名更新为 **Param_0**、 **Param_1**，以此类推。  
  
    -   在 DBParamInfoFlags 属性中指定值。 该值必须与 OLE DB DBPARAMFLAGSENUM 枚举中的值相匹配。 有关详细信息，请参阅 OLE DB 参考文档。  
  
    -   指定列的数据类型，并根据该数据类型指定列的代码页、长度、精度和小数位数。  
  
    -   若要删除未使用的参数，请选择 **“外部列”** 中的参数，再单击 **“删除列”**。  
  
    -   单击 **“列映射”** ，并将 **“可用输入列”** 列表中的列映射到 **“可用目标列”** 列表中的参数。  
  
14. 单击“确定”。   
  
15. 若要保存更新后的包，请单击 **“文件”** 菜单上的 **“保存”** 。  
  
## <a name="see-also"></a>另请参阅  
 [数据流](../../../integration-services/data-flow/data-flow.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
  
