---
description: 将数据分类与 Microsoft ODBC Driver for SQL Server 结合使用
title: 将数据分类与 Microsoft ODBC Driver for SQL Server 结合使用 | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837411"
---
# <a name="data-classification"></a>数据分类
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>概述
为了管理敏感数据，SQL Server 和 Azure SQL Server 引入了向数据库列提供敏感度元数据的功能，使客户端应用程序可以根据数据保护策略处理不同类型的敏感数据（例如运行状况、财务等）。

有关如何将分类分配到列的详细信息，请参阅 [SQL 数据发现和分类](../../relational-databases/security/sql-data-discovery-and-classification.md)。

Microsoft ODBC Driver 17.2 允许使用 SQL_CA_SS_DATA_CLASSIFICATION 字段标识符通过 SQLGetDescField 检索此元数据。

## <a name="format"></a>格式
SQLGetDescField 的语法如下：

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [输入] IRD（实现行描述符）句柄。 可以通过使用 SQL_ATTR_IMP_ROW_DESC 语句属性调用 SQLGetStmtAttr 来检索
  
 *RecNumber*  
 [输入] 0
  
 *FieldIdentifier*  
 [Input] SQL_CA_SS_DATA_CLASSIFICATION
  
 *ValuePtr*  
 [输出] 输出缓冲区
  
 *BufferLength*  
 [输入] 输出缓冲区的长度（以字节为单位）

 StringLengthPtr  [输出] 指向缓冲区的指针，该缓冲区会返回 ValuePtr  可返回的总字节数。
 
> [!NOTE]
> 如果缓冲区的大小未知，则可以通过调用 ValuePtr  为 NULL 的 SQLGetDescField 并检查 StringLengthPtr  的值来确定。
 
如果数据分类信息不可用，将返回错误“无效描述符字段”  。

成功调用 SQLGetDescField 时，ValuePtr  指向的缓冲区将包含以下数据：

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> `nn nn`、`tt tt` 和 `cc cc` 为多字节整数，它们与最低地址处的最低有效字节一起存储。

*`sensitivitylabel`* 和 *`informationtype`* 都是窗体

 `nn [n bytes name] ii [i bytes id]`

*`columnsensitivity`* 为窗体

 `nn nn [n sensitivityprops]`

对于每个列 (c)，均存在 n 4 字节 `sensitivityprops`：

 `ss ss tt tt`

s - 索引到 `sensitivitylabels` 数组，如果未标记，则为 `FF FF`

t - 索引到 `informationtypes` 数组，如果未标记，则为 `FF FF`


<br><br>
数据格式可以表示为以下伪结构：

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>代码示例
演示如何读取数据分类元数据的测试应用程序。 在 Windows 上，可以使用 `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` 进行编译，并使用连接字符串和 SQL 查询（返回分类列）作为参数运行：

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>支持的版本
如果 `FieldIdentifier` 设置为 `SQL_CA_SS_DATA_CLASSIFICATION` (1237)，Microsoft ODBC Driver 17.2 将允许通过 `SQLGetDescField` 检索数据分类信息。 

从 Microsoft ODBC Driver 17.4.1.1 开始，可以使用 `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238) 字段标识符通过 `SQLGetDescField` 检索服务器支持的数据分类版本。 在 17.4.1.1 中，支持的数据分类版本设置为“2”。

 

从 17.4.2.1 开始，引入了默认版本的数据分类（设置为“1”），该版本驱动程序以受支持状态报告给 SQL Server。 新的连接属性 `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) 允许应用程序将支持的数据分类版本从“1”更改为最大支持的版本。  

示例： 

若要设置版本，此调用应在 SQLConnect 或 SQLDriverConnect 调用之前进行：
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

当前支持的数据分类版本的值可以通过 SQLGetConnectAttr 调用检索到： 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```