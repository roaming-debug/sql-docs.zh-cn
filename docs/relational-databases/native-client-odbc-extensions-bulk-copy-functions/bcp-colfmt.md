---
description: bcp_colfmt
title: bcp_colfmt |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_colfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_colfmt function
ms.assetid: 5c3b6299-80c7-4e84-8e69-4ff33009548e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2478558cdb8df2de852fcdd4654c90d51b8356a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473628"
---
# <a name="bcp_colfmt"></a>bcp_colfmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  指定用户文件中的数据的源或目标格式。 用作源格式时， **bcp_colfmt** 指定用作大容量复制到表中的数据源的现有数据文件的格式 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 用作目标格式时，将使用 **bcp_colfmt** 指定的列格式创建数据文件。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_colfmt (  
        HDBC hdbc,  
        INT idxUserDataCol,  
        BYTE eUserDataType,  
        INT cbIndicator,  
        DBINT cbUserData,  
        LPCBYTE pUserDataTerm,  
        INT cbUserDataTerm,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *idxUserDataCol*  
 正在为其指定格式的用户数据文件中的列序号。 第一列为 1。  
  
 *eUserDataType*  
 用户文件中此列的数据类型。 如果与数据库表中相应列的数据类型不同 (*idxServerColumn*) ，则大容量复制会转换数据（如果可能）。  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 在 *eUserDataType* 参数中引入了对 SQLXML 和 SQLUDT 数据类型标记的支持。  
  
 *EUserDataType* 参数由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sqlncli.msi 中的数据类型标记进行枚举，而非 ODBC C 数据类型枚举器。 例如，可以使用特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的类型 SQLCHARACTER 来指定一个 ODBC 类型 SQL_C_CHAR 的字符串。  
  
 若要为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型指定默认的数据表示形式，则将此参数设置为 0。  
  
 若要在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *EUSERDATATYPE* 为 SQLDECIMAL 或 SQLNUMERIC 时，从大容量复制到文件中：  
  
-   如果源列不是 **decimal** 或 **numeric**，则使用默认的精度和小数位数。  
  
-   如果源列为 **decimal** 或 **numeric**，则使用源列的精度和小数位数。  
  
 cbIndicator  
 列数据中的长度/Null 指示器的长度（字节）。 有效指示器长度值是 0（不使用任何指示器时）、1、2、4 或 8。  
  
 若要指定默认的大容量复制指示器用法，请将此参数设置为 SQL_VARLEN_DATA。  
  
 指示器在内存中出现在任何数据的紧前面，在数据文件中出现在它们适用于的数据的紧前面。  
  
 如果使用多种方法来指定数据文件列长度（例如指示器和最大列长度，或者指示器和终止符序列），则大容量复制将选择导致数据复制量最少的方法。  
  
 如果列数据可能在长度上发生变化或列可能接受 NULL 作为值，则在没有调整数据格式的用户干预时，大容量复制生成的数据文件将包含指示器。  
  
 *cbUserData*  
 用户文件中该列数据的最大长度（字节），不包括任何长度指示器或终止符的长度。  
  
 如果将 *cbUserData* 设置为 SQL_NULL_DATA，则指示数据文件列中的所有值均为，或应设置为 NULL。  
  
 将 *cbUserData* 设置为 SQL_VARLEN_DATA 指示系统应确定每列中的数据长度。 对于某些列，这可能意味着生成长度/空指示符，以便置于来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的副本的数据前，或者意味着该指示符应位于复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据中。  
  
 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 字符和二进制数据类型， *cbUserData* 可以是 SQL_VARLEN_DATA、SQL_NULL_DATA、0或某个正值。 如果 SQL_VARLEN_DATA *cbUserData* ，则系统使用长度指示器（如果存在）或终止符序列来确定数据的长度。 如果长度指示符和终止符序列均提供，则大容量复制将采用导致数据复制量最少的方法。 如果 *cbUserData* SQL_VARLEN_DATA，则数据类型为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 字符或二进制类型，并且不指定长度指示符和终止符序列，系统将返回一条错误消息。  
  
 如果 cbUserData 为 0 或正值，则系统使用 cbUserData 作为最大数据长度 。 但是，如果除了正的 cbUserData 以外，还提供了长度指示器或终止符序列，则系统使用导致数据复制量最少的方法来确定数据长度。  
  
 cbUserData 值表示数据的字节计数。 如果字符数据由 Unicode 宽字符表示，则正的 cbUserData 参数值表示字符数乘以每个字符大小（字节）。  
  
 *pUserDataTerm*  
 要用于该列的终止符序列。 此参数主要用于字符数据类型，因为所有其他类型均属于固定长度，或者在二进制数据的情况下，要求长度指示器以精确记录提供的字节数目。  
  
 为了避免终止提取的数据，或者指示用户文件中的数据不终止，可将此参数设置为 NULL。  
  
 如果使用多种方法指定用户文件列长度（例如终止符和长度指示器，或者终止符和最大列长度），则大容量复制选择导致数据复制量最少的方法。  
  
 大容量复制 API 根据需要执行 Unicode 到 MBCS 的字符转换。 执行此操作时请务必小心，以便确保终止符字节字符串和字节字符串的长度都正确设置。  
  
 *cbUserDataTerm*  
 要用于该列的终止符序列的长度（字节）。 如果终止符不存在或不希望其出现在数据中，请将该值设置为 0。  
  
 *idxServerCol*  
 数据库表中列的序号位置。 第一个列编号为 1。 列的序号位置由 [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)报告。  
  
 如果该值是 0，则大容量复制将忽略数据文件中的该列。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 **Bcp_colfmt** 函数允许您为大容量复制指定用户文件格式。 对于大容量复制，格式包含以下部分：  
  
-   从用户文件列到数据库列的映射。  
  
-   每个用户文件列的数据类型。  
  
-   每个列的可选指示符的长度。  
  
-   每个用户文件列的数据的最大长度。  
  
-   每个列的可选终止字节序列。  
  
-   可选终止字节序列的长度。  
  
 对 **bcp_colfmt** 的每个调用都指定一个用户文件列的格式。 例如，若要更改五行用户数据文件中三列的默认设置，请首先调用 [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) **(5)**，然后调用 **bcp_colfmt** 5 次，其中三个调用设置您的自定义格式。 对于剩余的两次调用，将 *eUserDataType* 设置为0，并将 *cbIndicator*、 *cbUserData* 和 *cbUserDataTerm* 分别设置为0、SQL_VARLEN_DATA 和0。 此过程复制全部五列，其中的三列采用您的自定义格式，另两列采用默认格式。  
  
 对于 *cbIndicator*，值8指示大值类型现在有效。 如果为其相应列是新的 max 类型的字段指定了前缀，则只能将它设置为 8。 有关详细信息，请参阅 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md)。  
  
 调用 **bcp_colfmt** 之前，必须先调用 **bcp_columns** 函数。  
  
 您必须对用户文件中的每个列调用一次 **bcp_colfmt** 。  
  
 对任何用户文件列多次调用 **bcp_colfmt** 会导致错误。  
  
 不需要将用户文件中的所有数据都复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 若要跳过某一列，请指定该列的数据格式，并将 *idxServerCol* 参数设置为0。 如果您要跳过某一列，则必须指定其类型。  
  
 [Bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md)函数可用于保留格式规范。  
  
## <a name="bcp_colfmt-support-for-enhanced-date-and-time-features"></a>对增强的日期和时间功能的 bcp_colfmt 支持  
 有关用于日期/时间类型的 *eUserDataType* 参数所使用的类型的信息，请参阅 [OLE DB 和 ODBC&#41;&#40;的增强日期和时间类型的大容量复制更改](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
