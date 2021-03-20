---
description: 'IRowsetFastLoad：： Commit (Native Client OLE DB 提供程序) '
title: IRowsetFastLoad：： Commit (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IRowsetFastLoad::Commit (OLE DB)
apitype: COM
helpviewer_keywords:
- Commit method
ms.assetid: 19de9128-b91a-4626-847f-af721edaa24e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5e12a0c7ce8b92b430284ede1f62047abcb7954b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740997"
---
# <a name="irowsetfastloadcommit-native-client-ole-db-provider"></a>IRowsetFastLoad：： Commit (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  标记一批插入的行的末尾并将这些行写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 有关示例，请参阅[使用 IRowsetFastLoad 大容量复制数据 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md) 和[使用 IROWSETFASTLOAD 和 ISEQUENTIALSTREAM 将 BLOB 数据发送到 SQL Server (OLE DB)](../../relational-databases/native-client-ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
HRESULT Commit(  
      BOOL fDone);  
```  
  
## <a name="arguments"></a>参数  
  fDone[in]  
 如果是 FALSE，则行集保持有效性，并且可由使用者用于插入其他行。 如果是 TRUE，则行集失去有效性，并且使用者无法执行进一步的插入。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功，并且所有插入的数据已写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
 E_FAIL  
 发生了特定于访问接口的错误。 从访问接口检索特定错误文本的错误信息。  
  
 E_UNEXPECTED  
 对以前被 **IRowsetFastLoad::Commit** 方法作废的大容量复制行集调用了该方法。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序大容量复制行集作为延迟更新模式行集的行为。 当用户通过行集插入行数据时，对插入行的处理方式与在支持 IRowsetUpdate 的行集上挂起插入相同  。  
  
 使用者必须对大容量复制行集调用 Commit 方法，才能以与使用 IRowsetUpdate::Update 方法将挂起行提交到 SQL Server 实例相同的方式将插入行写入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。  
  
 如果使用者释放其对大容量复制行集的引用，而不调用 Commit 方法，则以前未写入的所有插入行将丢失  。  
  
 通过在将 fDone 参数设置为 FALSE 的情况下调用 Commit 方法，使用者可以成批插入行   。 当 fDone 设置为 TRUE 时，行集变为无效  。 无效的大容量复制行集仅支持 ISupportErrorInfo 接口和 IRowsetFastLoad::Release 方法   。  
  
## <a name="see-also"></a>另请参阅  
 [IRowsetFastLoad &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md)  
  
  
