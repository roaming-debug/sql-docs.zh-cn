---
title: 适用于 SQL Server 的 OLE DB 驱动程序中的 UTF-8 支持
description: 了解 OLE DB Driver for SQL Server 对 UTF-8 服务器编码和 UTF-8 客户端编码的支持。
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 52e66fe97869046d182f816a846b1b0121949385
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673893"
---
# <a name="utf-8-support-in-ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序中的 UTF-8 支持

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Microsoft OLE DB Driver for SQL Server（版本 18.2.1）添加了对 UTF-8 服务器编码的支持。 有关 SQL Server UTF-8 支持的信息，请参阅：

- [排序规则和 Unicode 支持](../../../relational-databases/collations/collation-and-unicode-support.md)
- [UTF-8 支持](../../../relational-databases/collations/collation-and-unicode-support.md#utf8)

18.4.0 版驱动程序添加了对 UTF-8 客户端编码的支持（在 Windows 10 中通过“区域设置”下的“所有语言都支持使用 Unicode UTF-8”复选框启用）。

> [!NOTE]  
> Microsoft OLE DB Driver for SQL Server 使用 [GetACP](/windows/win32/api/winnls/nf-winnls-getacp) 函数确定 DBTYPE_STR 输入缓冲区的编码。
>
> 从 18.4 版开始，支持 GetACP 返回 UTF-8 编码（在 Windows 10 中通过“区域设置”下的“所有语言都支持使用 Unicode UTF-8”复选框启用）的方案。 在以前的版本中，如果缓冲区需要存储 Unicode 数据，则应将缓冲区数据类型设置为 DBTYPE_WSTR（UTF-16 编码）。

## <a name="data-insertion-into-a-utf-8-encoded-char-or-varchar-column"></a>将数据插入到 UTF-8 编码的 CHAR 或 VARCHAR 列中

创建输入参数缓冲区以便进行插入时，使用 [DBBINDING 结构](/previous-versions/windows/desktop/ms716845(v=vs.85))的数组描述缓冲区。 每个 DBBINDING 结构将单个参数与使用者的缓冲区关联，并包含数据值的长度和类型等信息。 对于类型 CHAR 的输入参数缓冲区，DBBINDING 结构的 wType 应设置为 DBTYPE_STR。 对于类型 WCHAR 的输入参数缓冲区，DBBINDING 结构的 wType 应设置为 DBTYPE_WSTR。

执行包含参数的命令时，驱动程序将构造参数数据类型信息。 如果输入缓冲区类型与参数数据类型匹配，则不会在驱动程序中进行任何转换。 否则，驱动程序会将输入参数缓冲区转换为参数数据类型。 用户可以通过调用 [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)) 来显式设置参数数据类型。 如果未提供信息，则驱动程序会通过以下方式派生参数数据类型信息：(a) 当语句准备好后，从服务器中检索列元数据，或 (b) 尝试从输入参数数据类型进行默认转换。

输入参数缓冲区可以由驱动程序或服务器转换为服务器列排序规则，具体取决于输入缓冲区数据类型和参数数据类型。 在转换期间，如果客户端代码页或数据库排序规则代码页不能代表输入缓冲区中的所有字符，则可能会发生数据丢失。 下表介绍了将数据插入到启用了 UTF-8 的列中时的转换过程：

|缓冲区数据类型|参数数据类型|转换|用户预防措施|
|---             |---                |---       |---            |
|DBTYPE_STR|DBTYPE_STR|服务器从客户端代码页转换为数据库排序规则代码页；服务器从数据库排序规则代码页转换为列排序规则代码页。|确保客户端代码页和数据库排序规则代码页可以代表输入数据中的所有字符。 例如，若要插入波兰语字符，可以将客户端代码页设置为 1250（ANSI 中欧字符），并且数据库排序规则可以将波兰语用作排序规则指示符（例如，Polish_100_CI_AS_SC），或启用 UTF-8。|
|DBTYPE_STR|DBTYPE_WSTR|驱动程序从客户端代码页转换为 UTF-16 编码；服务器从 UTF-16 编码转换为列排序规则代码页。|确保客户端代码页可以代表输入数据中的所有字符。 例如，若要插入波兰语字符，可以将客户端代码页设置为 1250（ANSI 中欧字符）。|
|DBTYPE_WSTR|DBTYPE_STR|驱动程序从 UTF-16 编码转换为数据库排序规则代码页；服务器从数据库排序规则代码页转换为列排序规则代码页。|确保数据库排序规则代码页可以代表输入数据中的所有字符。 例如，若要插入波兰语字符，数据库排序规则代码页可以将波兰语用作排序规则指示符（例如，Polish_100_CI_AS_SC），或启用 UTF-8。|
|DBTYPE_WSTR|DBTYPE_WSTR|服务器从 UTF-16 转换为列排序规则代码页。|无。|

## <a name="data-retrieval-from-a-utf-8-encoded-char-or-varchar-column"></a>从 UTF-8 编码的 CHAR 或 VARCHAR 列进行数据检索

为检索到的数据创建缓冲区时，使用 [DBBINDING 结构](/previous-versions/windows/desktop/ms716845(v=vs.85))的数组描述缓冲区。 每个 DBBINDING 结构都会关联检索到的行中的单个列。 若要将列数据检索为 CHAR，请将 DBBINDING 结构的 wType 设置为 DBTYPE_STR。 若要将列数据检索为 WCHAR，请将 DBBINDING 结构的 wType 设置为 DBTYPE_WSTR。

对于结果缓冲区类型指示符 DBTYPE_STR，驱动程序将 UTF-8 编码的数据转换为客户端编码。 用户应确保客户端编码可以代表 UTF-8 列中的数据，否则可能会发生数据丢失。

对于结果缓冲区类型指示符 DBTYPE_WSTR，驱动程序将 UTF-8 编码的数据转换为 UTF-16 编码。

## <a name="communication-with-servers-that-dont-support-utf-8"></a>与不支持 UTF-8 的服务器通信

Microsoft OLE DB Driver for SQL Server 可确保以服务器可以理解的方式向服务器公开数据。 从启用 UTF-8 的客户端插入数据时，驱动程序先将 UTF-8 编码的字符串转换为数据库排序规则代码页，再将代码页发送到服务器。

> [!NOTE]  
> 只有支持 UTF-8 的服务器才能使用 [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) 接口将 UTF-8 编码的数据插入旧版文本列。 有关详细信息，请参阅 [BLOB 和 OLE 对象](../ole-db-blobs/blobs-and-ole-objects.md)。

## <a name="see-also"></a>另请参阅  

[适用于 SQL Server 的 OLE DB 驱动程序功能](oledb-driver-for-sql-server-features.md)  
[适用于 SQL Server 的 OLE DB 驱动程序的 UTF-16 支持](utf-16-support-in-oledb-driver-for-sql-server.md)  
