---
description: 执行多个表的增量加载
title: 执行多个表的增量加载 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],multiple tables
ms.assetid: 39252dd5-09c3-46f9-a17b-15208cfd336d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 229809b060a9a0d3103acdb9a562a3dc639b91e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338482"
---
# <a name="perform-an-incremental-load-of-multiple-tables"></a>执行多个表的增量加载

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在主题 [通过变更数据捕获改善增量加载](../../integration-services/change-data-capture/change-data-capture-ssis.md)中，关系图演示的是仅对一个表执行增量加载的基本包。 但是，加载一个表并不像执行多个表的增量加载那样常见。  
  
 在执行多个表的增量加载时，有些步骤必须对所有表执行一次，而有些步骤则必须针对每个源表重复执行。 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中提供了多个用于实现这些步骤的选项：  
  
-   使用父包和子包。  
  
-   在一个包中使用多个数据流任务。  
  
## <a name="loading-multiple-tables-by-using-a-parent-package-and-multiple-child-packages"></a>使用一个父包和多个子包来加载多个表  
 可以使用父包来执行只需执行一次的步骤。 子包将执行针对各源表的步骤。  
  
#### <a name="to-create-a-parent-package-that-performs-those-steps-that-only-have-to-be-done-once"></a>创建父包以执行只需执行一次的步骤  
  
1.  创建父包。  
  
2.  在控制流中，使用执行 SQL 任务或 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 表达式来计算端点。  
  
     有关如何计算端点的示例，请参阅 [指定变更数据的间隔](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md)。  
  
3.  如果需要，可使用 For 循环容器来延迟执行，直到所选周期的变更数据准备就绪。  
  
     有关 For 循环容器的示例，请参阅 [确定变更数据是否已准备就绪](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md)。  
  
4.  使用多个执行包任务针对每个要加载的表执行子包。 使用父包变量配置将父包中计算的端点传递到各个子包。  
  
     有关详细信息，请参阅 [执行包任务](../../integration-services/control-flow/execute-package-task.md) 和 [在子包中使用变量和参数的值](../../integration-services/packages/legacy-package-deployment-ssis.md#child)。  
  
#### <a name="to-create-child-packages-to-perform-those-steps-that-have-to-be-done-for-each-source-table"></a>创建子包以执行针对各源表的步骤  
  
1.  应为每个源表各创建一个子包。  
  
2.  在控制流中，使用脚本任务或执行 SQL 任务汇集将用于查询变更的 SQL 语句。  
  
     有关如何汇集查询的示例，请参阅 [准备查询变更数据](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)。  
  
3.  在每个子包中使用一个的数据流任务加载变更数据，并将变更数据应用到目标。 按照下面的步骤说明配置数据流：  
  
    1.  在数据流中，使用源组件来查询变更表中在所选端点内发生的变更。  
  
         有关如何查询变更表的示例，请参阅 [检索和了解变更数据](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md)。  
  
    2.  使用有条件拆分转换将插入、更新和删除操作定向到不同的输出，以便于进行适当的处理。  
  
         有关如何配置此转换以定向输出的示例，请参阅 [处理插入、更新和删除](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md)。  
  
    3.  使用目标组件将插入操作应用到目标。 使用具有参数化的 UPDATE 和 DELETE 语句的 OLE DB 命令转换，将更新和删除操作应用到目标。  
  
         有关如何使用此转换来应用更新和删除操作的示例，请参阅 [将变更应用到目标](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md)。  
  
## <a name="loading-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>在单个包中使用多个数据流任务加载多个表  
 或者，您也可以使用针对每个要加载的源表都包含一个独立数据流任务的单个包。  
  
#### <a name="to-load-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>在单个包中使用多个数据流任务加载多个表  
  
1.  创建一个包。  
  
2.  在控制流中，使用执行 SQL 任务或 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 表达式来计算端点。  
  
     有关如何计算端点的示例，请参阅 [指定变更数据的间隔](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md)。  
  
3.  如果需要，可使用 For 循环容器延迟执行过程，直到所选间隔的变更数据准备就绪。  
  
     有关 For 循环容器的示例，请参阅 [确定变更数据是否已准备就绪](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md)。  
  
4.  使用脚本任务或执行 SQL 任务汇集将用于查询变更的 SQL 语句。  
  
     有关如何汇集查询的示例，请参阅 [准备查询变更数据](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)。  
  
5.  使用多个数据流任务加载各源表中的变更数据，并将变更数据应用到目标。 按照下面的步骤说明配置各个数据流。  
  
    1.  在各个数据流中，使用源组件来查询变更表中在所选端点内发生的变更。  
  
         有关如何查询变更表的示例，请参阅 [检索和了解变更数据](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md)。  
  
    2.  使用有条件拆分转换将插入、更新和删除操作定向到不同的输出，以便于进行适当的处理。  
  
         有关如何配置此转换以定向输出的示例，请参阅 [处理插入、更新和删除](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md)。  
  
    3.  使用目标组件将插入操作应用到目标。 使用具有参数化的 UPDATE 和 DELETE 语句的 OLE DB 命令转换，将更新和删除操作应用到目标。  
  
         有关如何使用此转换来应用更新和删除操作的示例，请参阅 [将变更应用到目标](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md)。  
  
  
