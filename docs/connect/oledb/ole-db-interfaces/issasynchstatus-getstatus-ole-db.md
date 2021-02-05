---
title: ISSAsynchStatus::GetStatus（OLE DB 驱动程序）| Microsoft Docs
description: 了解 ISSAsynchStatus::GetStatus 方法如何返回 OLE DB Driver for SQL Server 中的异步执行操作的状态。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::GetStatus (OLE DB)
apitype: COM
helpviewer_keywords:
- GetStatus method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7220f8c6552858fda5d6c42b0a4ea2d7ee0c7857
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183722"
---
# <a name="issasynchstatusgetstatus-ole-db"></a>ISSAsynchStatus::GetStatus (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  返回异步执行操作的状态。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT GetStatus(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation,  
        DBCOUNTITEM *pulProgress,  
        DBCOUNTITEM *pulProgressMax,  
        DBASYNCHPHASE *peAsynchPhase,  
        LPOLESTR *ppwszStatusText);  
```  
  
## <a name="arguments"></a>参数  
 hChapter[in]   
 章节句柄。 如果轮询的对象不是行集对象或操作不适用于某一章节，则应将其设置为 DB_NULL_HCHAPTER，访问接口将忽略此参数。  
  
 eOperation[in]   
 请求其异步状态的操作。 应使用以下值：  
  
 DBASYNCHOP_OPEN - 使用者请求获取有关行集的异步打开或填充的信息，或有关数据源对象的异步初始化的信息。 如果访问接口是支持直接 URL 绑定且与 OLE DB 2.5 兼容的访问接口，使用者则请求有关异步初始化或填充数据源、行集、行或流对象的信息。  
  
 pulProgress[out]   
 指向内存的指针，将在此内存中返回与 pulProgressMax 参数指示的预期最大值相关的异步操作的当前进度  。 有关 pulProgress 的含义的详细信息，请参阅对 peAsynchPhase 的说明   。  
  
 如果 pulProgress 为空指针，则不返回进度  。  
  
 pulProgressMax[out]   
 指向内存的指针，将在此内存中返回 pulProgress 参数的预期最大值  。 此值可以随此方法的每次调用而更改。 有关 pulProgressMax 的含义的详细信息，请参阅对 peAsynchPhase 的说明   。  
  
 如果 pulProgressMax 为空指针，则不返回预期的最大值  。  
  
 peAsynchPhase[out]   
 指向内存的指针，将在此内存中返回有关异步操作的进度的其他信息。 有效值包括：  
  
 DBASYNCHPHASE_INITIALIZATION - 对象处于初始化阶段。 参数 pulProgress 和 pulProgressMax 指示估计的完成比率   。 对象尚未完全具体化。 尝试调用任何其他接口可能失败，并且可能无法对该对象使用整套接口。 如果异步操作是因调用更新、删除或插入行的命令的 ICommand::Execute 而引起的，并且如果 cParamSets 大于 1，pulProgress 和 pulProgressMax 可以指示一个参数集或所有参数集的进度     。  
  
 DBASYNCHPHASE_POPULATION - 对象处于填充阶段。 尽管已完全初始化行集，并且可针对该对象使用整套接口，但是可能还有其他行尚未填充到行集中。 虽然 pulProgress 和 pulProgressMax 可以基于填充的行数，但它们通常基于填充行集所需的时间和工作   。 因此，调用方应将此信息用作对此过程所需时间的粗略估计，而不是最终的行计数。 此阶段仅在填充行集期间返回；它永远不会在数据源对象初始化期间返回，也不会因执行更新、删除或插入行的命令而返回。  
  
 DBASYNCHPHASE_COMPLETE - 已完成对象的所有异步处理。 ISSAsynchStatus::GetStatus 方法返回指示操作结果的 HRESULT  。 通常，如果已同步调用操作，则将返回 HRESULT。 如果异步操作是因调用更新、删除或插入行的命令的 ICommand::Execute 而引起的，pulProgress 和 pulProgressMax 则等于该命令所影响的总行数    。 如果 cParamSets 大于 1，则等于执行过程中指定的所有参数集所影响的总行数  。 如果 peAsynchPhase 为空指针，则不返回状态代码  。  
  
 DBASYNCHPHASE_CANCELED - 已中止对象的异步处理。 ISSAsynchStatus::GetStatus 方法返回 DB_E_CANCELED  。 如果异步操作是因调用更新、删除或插入行的命令的 ICommand::Execute 而引起的，pulProgress 则等于取消之前该命令所影响的所有参数集的总行数   。  
  
 ppwszStatusText[in/out]   
 指向包含有关操作的其他信息的内存的指针。 提供程序可以使用此值来区分不同的操作元素（例如，要访问的不同资源）。 该字符串的本地化形式基于数据源对象的 DBPROP_INIT_LCID 属性。  
  
 如果 ppwszStatusText 在输入时为非 Null，提供程序则返回与 ppwszStatusText 标识的特定元素相关联的状态   。 如果 ppwszStatusText 未指示 eOperation 的元素，提供程序则返回 S_OK，并将 pulProgress 和 pulProgressMax 设置为相同的值     。 如果提供程序不能基于文本标识符区分各元素，则会将 ppwszStatusText 设置为 NULL，并返回有关整个操作的信息；否则，如果 ppwszStatusText 在输入时为非 Null，提供程序将使 ppwszStatusText 保持不变    。  
  
 如果 ppwszStatusText 在输入时为 Null，提供程序则将 ppwszStatusText 设置为指示操作的更多相关信息的值，如果没有此类信息或 ISSAsynchStatus::GetStatus 方法返回错误，则将其设置为 NULL    。 如果 ppwszStatusText 在输入时为 Null，提供程序将为状态字符串分配内存，并返回此内存的地址  。 如果不再需要此字符串，使用者可以使用 IMalloc::Free 释放此内存  。  
  
 如果 ppwszStatusText 在输入时为 NULL，则不会返回状态字符串，提供程序将返回有关操作的任一元素的信息或有关操作的总体信息  。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 该方法已成功返回。  
  
-   如果 peAsynchPhase 等于 DBASYNCHPHASE_INITIALIZATION，则尚未完全初始化对象；尝试调用任何其他接口可能失败，并且可能无法对该对象使用整套接口  。  
  
-   如果 peAsynchPhase 等于 DBASYNCHPHASE_POPULATION，则已完全初始化行集，并且可针对该对象使用整套接口；但是可能还有其他行尚未填充到行集中  。  
  
-   如果 peAsynchPhase 等于 DBASYNCHPHASE_COMPLETE，则已完成对象的所有异步处理  。 已完全初始化并填充该对象。  
  
 DB_E_CANCELED  
 异步处理在行集填充过程中取消。 填充停止，但是对于已填充的行，该行集仍保持有效。  
  
 异步处理在数据源对象的初始化过程中取消。 数据源对象处于未初始化状态。  
  
 E_INVALIDARG  
 hChapter  参数不正确。  
  
 E_UNEXPECTED  
 已对数据源对象调用 ISSAsynchStatus::GetStatus 方法，但尚未对该数据源对象调用 IDBInitialize::Initialize   。  
  
 已对行集调用 ISSAsynchStatus::GetStatus 方法，并且已调用 ITransaction::Commit 或 ITransaction::Abort，对象处于僵停状态    。  
  
 已针对在行集的初始化阶段中异步取消的行集调用 ISSAsynchStatus::GetStatus 方法  。 该行集处于僵停状态。  
  
 E_FAIL  
 发生了特定于访问接口的错误。  
  
## <a name="remarks"></a>备注  
 ISSAsynchStatus::GetStatus 方法的行为与 IDBAsynchStatus::GetStatus 方法完全类似，不同之处在于如果中止对数据源对象的初始化，前者将返回 E_UNEXPECTED，而不是 DB_E_CANCELED（但是 [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) 将返回 DB_E_CANCELED） 。 这是因为在中止后，数据源对象不会照常处于僵停状态，以便进一步尝试初始化操作。  
  
 如果异步初始化或填充行集，则必须支持此方法。  
  
 除列出的返回值以外，ISSAsynchStatus::GetStatus 还可以返回应由启动异步操作的方法返回的任何 HRESULT，以指示操作成功或失败  。  
  
 某些异步操作可能无法返回除“完成”和“尚未完成”以外的任何状态。 这些异步操作应将 pulProgressMax 值设置为 1，指示其估计的粒度（全部粒度或没有任何粒度），因此返回的值将为 0/1 或 1/1  。  
  
 提供程序可以在后续调用中更改 pulProgressMax，甚至返回比先前更小的比率，条件是该比率反映对任务完成程度的改善估计  。  
  
 访问接口不必保证任何进一步的准确性，但是对于可以对完成情况进行合理估计的情况，建议执行此工作。 这一工作将改进用户界面的质量，因为此函数的主要用途是向用户提供进度反馈。 由于改善了长期运行的不可见任务的反馈质量，用户的满意度也随之提高。  
  
 对已初始化的数据源对象或已填充的行集调用 ISSAsynchStatus::GetStatus，或者传递除 DBASYNCHOP_OPEN 以外的 eOperation 值将返回 S_OK，并将 pulProgress 和 pulProgressMax 设置为相同的值     。 对于通过执行更新、删除或插入行的命令而创建的对象，如果对这样的对象调用 ISSAsynchStatus::GetStatus 方法，pulProgress 和 pulProgressMax 则指示该命令所影响的总行数    。  
  
## <a name="see-also"></a>另请参阅  
 [执行异步操作](../../oledb/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
