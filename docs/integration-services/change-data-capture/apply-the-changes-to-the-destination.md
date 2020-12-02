---
description: 将变更应用到目标
title: 将变更应用到目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],applying changes
ms.assetid: 338a56db-cb14-4784-a692-468eabd30f41
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0176dcc68e8c7f72c4bd9b78b1f3d926e52ad215
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123738"
---
# <a name="apply-the-changes-to-the-destination"></a>将变更应用到目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  在用于执行变更数据增量加载的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的数据流中，第三个任务（即最后一个任务）是将变更应用到目标。 您将需要一个组件应用插入操作、一个组件应用更新操作以及一个组件应用删除操作。  
  
> [!NOTE]  
>  为用于执行变更数据增量加载的包设计数据流的过程中，第二个任务是分隔插入、更新和删除操作。 有关此组件的详细信息，请参阅 [处理插入、更新和删除](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md)。 有关创建用于执行变更数据增量加载的包的完整过程的说明，请参阅[变更数据捕获 (SSIS)](../../integration-services/change-data-capture/change-data-capture-ssis.md)。  
  
## <a name="applying-inserts"></a>应用插入操作  
 若要应用插入操作，请使用 OLE DB 目标，因为新行不需要任何特殊处理。  
  
#### <a name="to-process-inserts-by-using-an-ole-db-destination"></a>使用 OLE DB 目标处理插入操作  
  
1.  在 **“数据流”** 选项卡上，添加 OLE DB 目标。  
  
2.  将包含来自有条件拆分转换的插入操作的输出连接到 OLE DB 目标。  
  
3.  在 **“OLE DB 目标编辑器”** 的 **“连接管理器”** 页上，选择下列选项：  
  
    1.  为目标数据库选择或创建一个 OLE DB 连接管理器。  
  
    2.  选择 **“数据访问模式”** 选项，然后选择包含目标列的目标表或输入包含目标列的 SQL 语句。  
  
4.  在编辑器的 **“映射”** 页上，将变更数据的相应列映射到目标表。  
  
## <a name="applying-updates"></a>应用更新操作  
 若要应用更新操作，请使用 OLE DB 命令转换。 因为您必须通过参数化的 UPDATE 语句使用新的列值一次更新一行，所以要使用此转换。  
  
> [!NOTE]  
>  还可以使用目标组件来应用更新操作。 如果使用此方法，则将使用目标组件将行保存到为此而创建的临时表中。 然后，使用执行 SQL 任务根据临时表对目标执行大容量更新和大容量删除操作。  
  
#### <a name="to-process-updates-by-using-an-ole-db-command-transformation"></a>使用 OLE DB 命令转换处理更新操作  
  
1.  在 **“数据流”** 选项卡上，添加 OLE DB 命令转换。  
  
2.  将包含来自有条件拆分转换的更新操作的输出连接到 OLE DB 命令转换。  
  
3.  在 **“OLE DB 命令的高级编辑器”** 的 **“连接管理器”** 选项卡上，为目标数据库选择或创建一个 OLE DB 连接管理器。  
  
4.  在 **“OLE DB 命令的高级编辑器”** 的 **“组件属性”** 选项卡上，对于 **SqlCommand**，输入参数化的 UPDATE 语句。  
  
     例如，Customer 表的 UPDATE 语句可能具有以下语法：  
  
    ```sql
    update CDCSample.Customer  
    set TerritoryID  = ?,  
        CustomerType  = ?,  
        rowguid  = ?,  
        ModifiedDate  = ?  
    where CustomerID = ?  
  
    ```  
  
5.  在编辑器的 **“列映射”** 选项卡上，将变更数据的相应列映射到 UPDATE 语句中的参数。  
  
## <a name="applying-deletes"></a>应用删除操作  
 若要应用删除操作，请使用 OLE DB 命令转换。 因为您必须通过参数化的 DELETE 语句使用唯一标识行的列值一次删除一行，所以要使用此转换。  
  
> [!NOTE]  
>  还可以使用目标组件来应用删除操作。 如果使用此方法，则将使用目标组件将行保存到为此而创建的临时表中。 然后，使用执行 SQL 任务根据临时表对目标执行大容量更新和大容量删除操作。  
  
#### <a name="to-process-deletes-by-using-an-ole-db-command-transformation"></a>使用 OLE DB 命令转换处理删除操作  
  
1.  在 **“数据流”** 选项卡上，向数据流中添加 OLE DB 命令转换。  
  
2.  将包含来自有条件拆分转换的删除操作的输出连接到 OLE DB 命令转换。  
  
3.  打开“高级编辑器”以配置该转换。  
  
4.  在 **“OLE DB 命令的高级编辑器”** 的 **“连接管理器”** 选项卡上，为目标数据库选择或创建一个 OLE DB 连接管理器。  
  
5.  在 **“OLE DB 命令的高级编辑器”** 的 **“组件属性”** 选项卡上，对于 **SqlCommand**，输入参数化的 DELETE 语句。  
  
     例如，Customer 表的 DELETE 语句可能具有以下语法：  
  
    ```sql
    delete from Customer where CustomerID = ?  
  
    ```  
  
6.  在编辑器的 **“列映射”** 选项卡上，将变更数据的相应列映射到 DELETE 语句中的参数。  
  
## <a name="optimizing-inserts-and-updates-by-using-merge-functionality"></a>使用 MERGE 功能优化插入和更新操作  
 可以通过将特定的变更数据捕获选项与 Transact-SQL MERGE 关键字结合使用，来优化插入操作和更新操作的处理。 有关 MERGE 关键字的详细信息，请参阅[ MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)。  
  
 在用于检索变更数据的 Transact-SQL 语句中，调用 **cdc.fn_cdc_get_net_changes_<capture_instance>** 函数时可以将 all with merge 指定为 row_filter_option 参数的值。 当此变更数据捕获函数不需要执行用于区分插入操作和更新操作的额外处理时，它的操作效率会大大提高。 将 all with merge 指定为参数值时，对于删除操作来说，变更数据的 **__$operation** 值为 1，而对于插入操作或更新操作引起的变更来说，该值为 5。 有关用于检索变更数据的 Transact-SQL 函数的详细信息，请参阅[检索和了解变更数据](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md)。使用 all with merge 参数值检索变更之后，可以应用删除操作，并将剩余行输出到临时表或中间临时表中。 然后，在下游执行 SQL 任务中，可以使用一个 MERGE 语句将所有插入操作或更新操作从中间临时表应用到目标中。  
  
  
