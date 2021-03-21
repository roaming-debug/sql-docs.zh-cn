---
description: 'IBCPSession：： BCPControl (Native Client OLE DB 提供程序) '
title: IBCPSession：： BCPControl (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- IBCPSession::BCPControl (OLE DB)
apitype: COM
helpviewer_keywords:
- BCPControl method
ms.assetid: d58f3fe1-45e3-4e46-8e9c-000971829d99
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ddb46c302345a4031f08597ecdbac428916061bc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749397"
---
# <a name="ibcpsessionbcpcontrol-native-client-ole-db-provider"></a>IBCPSession：： BCPControl (Native Client OLE DB 提供程序) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  设置大容量复制操作的选项。  
  
## <a name="syntax"></a>语法  
  
```  

HRESULT BCPControl(   
      int eOption,  
      void *iValue);  
```  
  
## <a name="remarks"></a>备注  
 BCPControl 方法设置用于大容量复制操作的各种控制参数，其中包括取消大容量复制之前允许的错误数、要从数据文件中复制的第一行和最后一行的行数和批量大小  。  
  
 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制数据时，此方法还可用于指定要使用的 SELECT 语句。 可将 eOption 参数设置为 BCP_OPTION_HINTS，并将 iValue 参数设置为具有一个指针，该指针指向包含该 SELECT 语句的宽字符串   。  
  
 eOption  的可能值为：  
  
|选项|说明|  
|------------|-----------------|  
|BCP_OPTION_ABORT|停止正在进行的大容量复制操作。 可以从其他线程调用 eOption 参数为 BCP_OPTION_ABORT 的 BCPControl 方法，以停止正在运行的大容量复制操作   。 忽略 iValue  参数。|  
|BCP_OPTION_BATCH|每批的行数。 默认值为 0，在提取数据时，该默认值表示表中的所有行；在将数据复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，该默认值表示用户数据文件中的所有行。 值小于 1 则将 BCP_OPTION_BATCH 重置为默认值。|  
|BCP_OPTION_DELAYREADFMT|一个布尔值，如果设置为 true，将导致 [IBCPSession::BCPReadFmt](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpreadfmt-ole-db.md) 在执行时读取该值。 如果为 false（默认值），IBCPSession::BCPReadFmt 将立即读取格式化文件。 如果 BCP_OPTION_DELAYREADFMT  为 true，并且调用 IBCPSession::BCPColumns 或 IBCPSession::BCPColFmt，则会发生序列错误。<br /><br /> 如果在调用 `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)TRUE)` 和 IBCPSession::BCPWriteFmt 后调用 `IBCPSession::BCPControl(BCPDELAYREADFMT, (void *)FALSE))`，同样会发生序列错误。<br /><br /> 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。|  
|BCP_OPTION_FILECP|iValue 参数包含数据文件的代码页的编号  。 可以指定代码页的编号，例如 1252 或 850，或者采用以下值之一：<br /><br /> BCP_FILECP_ACP：文件中的数据位于客户端的 Microsoft Windows® 代码页中。<br /><br /> BCP_FILECP_OEMCP：文件中的数据位于客户端的 OEM 代码页中（默认值）。<br /><br /> BCP_FILECP_RAW：文件中的数据位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的代码页中。|  
|BCP_OPTION_FILEFMT|数据文件格式的版本号。 该版本号可以是 80 ([!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)])、90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)])、100（[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]）、110 ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) 或 120 ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])。 120 是默认值。 对于采用服务器早期版本所支持的格式的数据，该选项对导出和导入这样的数据非常有用。  例如，若要将从 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 服务器中的文本列获取的数据导入到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本服务器中的 varchar(max) 列，则应该指定为 80  。 同样，如果从 varchar(max) 列导出数据时指定为 80，数据的保存方式就与按照 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 格式保存的文本列类似，并且可以将这些数据导入到 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 服务器的文本列中  。|  
|BCP_OPTION_FIRST|要复制的文件或表的第一行数据。 默认值为 1；值小于 1 则将此选项重置为其默认值。|  
|BCP_OPTION_FIRSTEX|对于 BCP out 操作，指定要复制到数据文件的数据库表的第一行。<br /><br /> 对于 BCP in 操作，指定要复制到数据库表的数据文件的第一行。<br /><br /> iValue 参数应为包含值的带符号的 64 位整数的地址  。 可以传递到 BCPFIRSTEX 的最大值为 2^63-1。|  
|BCP_OPTION_FMTXML|用于指定生成的格式化文件应采用 XML 格式。 默认情况下关闭此选项，此时将格式化文件作为文本文件保存。 XML 格式化文件提供更大的灵活性，但具有某些额外约束。 例如，不能同时为字段指定前缀和终止符，而在较早的格式化文件中则可以执行此操作。<br /><br /> 注意：仅当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具与 Native Client 一起安装时，才支持 XML 格式化文件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|BCP_OPTION_HINTS|iValue 参数包含宽字符串指针  。 寻址的字符串指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制处理提示或返回结果集的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 如果指定返回多个结果集的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，则忽略第一个结果集之后的所有结果集。|  
|BCP_OPTION_KEEPIDENTITY|将 iValue 参数设置为 TRUE 时，此选项指定大容量复制方法插入为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 列（使用标识约束定义）提供的数据值  。 输入文件必须提供标识列的值。 如果未进行设置，则为插入的行生成新标识值。 忽略文件的标识列中所存在的任何数据。|  
|BCP_OPTION_KEEPNULLS|指定是否会将文件中的空数据值转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的 NULL 值。 将 iValue 参数设置为 TRUE 时，会将空值转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的 NULL  。 默认情况下会将空值转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的列的默认值（如果存在默认值）。|  
|BCP_OPTION_LAST|要复制的最后一行。 默认值为复制所有行。 值小于 1 则将此选项重置为其默认值。|  
|BCP_OPTION_LASTEX|对于 BCP out 操作，指定要复制到数据文件的数据库表的最后一行。<br /><br /> 对于 BCP in 操作，指定要复制到数据库表的数据文件的最后一行。<br /><br /> iValue 参数应为包含值的带符号的 64 位整数的地址  。 可以传递到 BCPLASTEX 的最大值为 2^63-1。|  
|BCP_OPTION_MAXERRS|在大容量复制操作失败之前允许的错误数。 默认值为 10。 值小于 1 则将此选项重置为其默认值。 大容量复制将最大错误数限制为 65,535 个。 如果尝试将该选项设置为大于 65,535 的值，将导致该选项设置为 65,535。|  
|BCP_OPTION_ROWCOUNT|返回当前（或上一次）BCP 操作所影响的行数。|  
|BCP_OPTION_TEXTFILE|数据文件不是二进制文件而是文本文件。 BCP 将通过检查数据文件的前两个字节中的 Unicode 字节标记来确定该文本文件是否是 Unicode。|  
|BCP_OPTION_UNICODEFILE|如果设置为 TRUE，则此选项指定输入文件是 Unicode 文件格式。|  
  
## <a name="arguments"></a>参数  
  eOption[in]  
 设置为以上备注一节中所列的选项之一。  
  
  iValue[in]  
 指定的 eOption 的值  。 iValue 参数是转换为 void 指针的整数值，允许将来扩展到 64 位值  。  
  
## <a name="return-code-values"></a>返回代码值  
 S_OK  
 方法成功。  
  
 E_FAIL  
 出现访问接口特定的错误；若要获取详细信息，请使用 [ISQLServerErrorInfo](isqlservererrorinfo-geterrorinfo-ole-db.md) 接口。  
  
 E_UNEXPECTED  
 意外调用了该方法。 例如，在调用此函数前，未调用 [IBCPSession::BCPInit](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpinit-ole-db.md) 方法。  
  
 E_OUTOFMEMORY  
 内存不足错误。  
  
## <a name="see-also"></a>另请参阅  
 [IBCPSession &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-ole-db.md)   
 [执行大容量复制操作](../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
  
