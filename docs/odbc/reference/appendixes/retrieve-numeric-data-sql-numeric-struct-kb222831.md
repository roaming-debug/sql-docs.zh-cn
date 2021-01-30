---
title: 用 SQL_NUMERIC_STRUCT 检索数字数据 |Microsoft Docs
description: C/c + + 使用 ODBC 通过使用与 SQL_C_NUMERIC 相关的 SQL_NUMERIC_STRUCT 来检索 SQL Server 数字数据类型。
editor: ''
ms.prod: sql
ms.technology: connectivity
ms.devlang: cpp
ms.topic: reference
ms.custom: ''
ms.date: 07/14/2017
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: dce2f56d3257ac3b513ef3839f949258c6f018df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187169"
---
# <a name="retrieve-numeric-data-with-sql_numeric_struct"></a>用 SQL \_ 数值结构检索数字 \_ 数据

本文介绍如何将 SQL Server ODBC 驱动程序中的数字数据检索到数字结构中。 还介绍了如何使用特定的精度和小数位数值获取正确的值。

此数据类型允许应用程序直接处理数值数据。 在2003年，ODBC 3.0 引入了一个新的 ODBC C 数据类型，由 **SQL \_ C \_ 数字** 标识。 此数据类型仍与2017相关。

使用的 C 缓冲区具有 **SQL \_ 数值 \_ 结构** 的类型定义。 此结构具有用于存储数值数据的精度、小数位数、签名和值的字段。 值本身存储为缩放的整数，其最小有效字节始于最左端的位置。 

[C 数据类型](c-data-types.md)一文提供了有关 SQL 数值结构的格式和使用的详细信息 \_ \_ 。 通常，ODBC 3.0 程序员参考的 [附录 D](appendix-d-data-types.md) 讨论了数据类型。


## <a name="sql_numeric_struct-overview"></a>SQL \_ 数字 \_ 结构概述


SQL \_ 数值 \_ 结构在 sqltypes 头文件中定义，如下所示：


```c
#define SQL_MAX_NUMERIC_LEN    16
typedef struct tagSQL_NUMERIC_STRUCT
{
   SQLCHAR    precision;
   SQLSCHAR   scale;
   SQLCHAR    sign;   /* 1 if positive, 0 if negative */
   SQLCHAR    val[SQL_MAX_NUMERIC_LEN];
} SQL_NUMERIC_STRUCT;
```

            
数字结构的精度和小数位数字段从不用于从应用程序进行输入，而仅用于从驱动程序到应用程序的输出。

该驱动程序在将数据返回到应用程序时，会使用默认精度 (驱动程序定义的) 和默认的小数位数 (0) 。 除非应用程序指定精度和小数位数的值，否则，驱动程序将采用默认值并截断数值数据的小数部分。

## <a name="sql_numeric_struct-code-sample"></a>SQL \_ 数值 \_ 结构代码示例

此代码示例演示如何执行以下操作：

- 设置精度。
- 设置小数位数。
- 检索正确的值。 

> [!Note]
> 本文中提供的代码的任何使用都是您自己的风险。 
>
> Microsoft 提供这些代码示例 "按原样"，没有任何明示或默示保证，包括但不限于适销性和/或适用于特定用途的默示保证。

```c
#include <stdio.h>
#include <string.h>
#include <conio.h>
#include <stdlib.h>
#include <ctype.h>
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

#define MAXDSN       25
#define MAXUID       25
#define MAXAUTHSTR   25
#define MAXBUFLEN   255

SQLHENV     henv   = SQL_NULL_HENV;
SQLHDBC     hdbc1  = SQL_NULL_HDBC;     
SQLHSTMT    hstmt1 = SQL_NULL_HSTMT;
SQLHDESC    hdesc  = NULL;


SQL_NUMERIC_STRUCT NumStr;

int main()
{
   RETCODE retcode;

//Change the values below as appropriate to make a successful connection.
//szDSN: DataSourceName, szUID=userid, szAuthStr: password

UCHAR szDSN[MAXDSN+1] = "sql33",szUID[MAXUID+1]="sa", szAuthStr[MAXAUTHSTR+1] = "";
SQLINTEGER strlen1;
SQLINTEGER a;
int i,sign =1;
long myvalue, divisor;
float final_val;
   
// Allocate the Environment handle. Set the Env attribute, allocate the
//connection handle, connect to the database and allocate the statement //handle.

retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);
retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,(SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);
retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);
retcode = SQLConnect(hdbc1, szDSN,SQL_NTS,szUID,SQL_NTS,szAuthStr,SQL_NTS);
retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);

// Execute the select statement. Here it is assumed that numeric_test
//table is created using the following statements:

// Create table numeric_test (col1 numeric(5,3))
//insert into numeric_test values (25.212)

retcode = SQLExecDirect(hstmt1,(UCHAR *)"select * from numeric_test",SQL_NTS);

// Use SQLBindCol to bind the NumStr to the column that is being retrieved.

retcode = SQLBindCol(hstmt1,1,SQL_C_NUMERIC,&NumStr,19,&strlen1);

// Get the application row descriptor for the statement handle using
//SQLGetStmtAttr.

retcode = SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_ROW_DESC,&hdesc, 0, NULL);

// You can either use SQLSetDescRec or SQLSetDescField when using
// SQLBindCol. However, if you prefer to call SQLGetData, you have to
// call SQLSetDescField instead of SQLSetDescRec. For more information on
// descriptors, please refer to the ODBC 3.0 Programmers reference or
// your Online documentation.

//Used when using SQLSetDescRec
//a=b=sizeof(NumStr);

// Set the datatype, precision and scale fields of the descriptor for the 
//numeric column. Otherwise the default precision (driver defined) and 
//scale (0) are returned.

// In this case, the table contains only one column, hence the second 
//parameter contains one. Zero applies to bookmark columns. Please check 
//the programmers guide for more information.

//retcode=SQLSetDescRec(hdesc,1,SQL_NUMERIC,NULL,sizeof(NumStr),5,3,&NumStr,&a,&b);

retcode = SQLSetDescField (hdesc,1,SQL_DESC_TYPE,(VOID*)SQL_C_NUMERIC,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_PRECISION,(VOID*) 5,0);
retcode = SQLSetDescField (hdesc,1,SQL_DESC_SCALE,(VOID*) 3,0);
    
// Initialize the val array in the numeric structure.

memset(NumStr.val,0,16);
   
// Call SQLFetch to fetch the first record.

while((retcode =SQLFetch(hstmt1)) != SQL_NO_DATA)
  {
// Notice that the TargetType (3rd Parameter) is SQL_ARD_TYPE, which  
//forces the driver to use the Application Row Descriptor with the 
//specified scale and precision.

   retcode = SQLGetData(hstmt1, 1, SQL_ARD_TYPE, &NumStr, 19, &a); 

// Check for null indicator.

   if ( SQL_NULL_DATA == a )
   {
   printf( "The final value: NULL\n" );
   continue;
   }

// Call to convert the little endian mode data into numeric data.

   myvalue = strtohextoval();

// The returned value in the above code is scaled to the value specified
//in the scale field of the numeric structure. For example 25.212 would
//be returned as 25212. The scale in this case is 3 hence the integer 
//value needs to be divided by 1000.

   divisor = 1;
   if(NumStr.scale > 0)
     {
    for (i=0;i< NumStr.scale; i++)   
         divisor = divisor * 10;
     }
   final_val =  (float) myvalue /(float) divisor;

// Examine the sign value returned in the sign field for the numeric
//structure.
//NOTE: The ODBC 3.0 spec required drivers to return the sign as 
//1 for positive numbers and 2 for negative number. This was changed in the
//ODBC 3.5 spec to return 0 for negative instead of 2.

      if(!NumStr.sign) sign = -1;
      else sign  =1;

   final_val *= sign;
   printf("The final value: %f\n",final_val);
    }

   while ( ( retcode = SQLMoreResults(hstmt1) ) != SQL_NO_DATA_FOUND);

   /* clean up */ 
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);
   SQLDisconnect(hdbc1);
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
   return(0);
}
```


### <a name="interim-results"></a>过渡结果：


```console
//  C  ==> 12 * 1    =     12
//  7  ==> 07 * 16   =    112
//  2  ==> 02 * 256  =    512
//  6  ==> 06 * 4096 =  24576
===============================
                 Sum =  25212
```


在数字结构中，val 字段是16个元素的字符数组。 例如，25.212 缩放为25212，小数位数为3。 在十六进制格式中，此数字应为627C。

驱动程序返回以下项：

- 7C 的等效字符，即 "|" (管道) 在字符数组的第一个元素中。
- 等效于62，后者为第二个元素中的 "b"。
- 数组元素的余数包含零，因此缓冲区包含 "| b\0"。

现在，问题在于构造此字符串数组中的已缩放整数。 字符串中的每个字符都对应于两个十六进制数字，即最小有效数字 (LSD) ，最高有效位 (MSD) 。 可以通过将每个数字 (LSD & MSD) 乘以16的倍数（从1开始）来生成缩放的整数值。

实现从 little endian 模式到缩放整数的转换的代码。 应用程序开发人员可以实现此功能。 下面的代码示例只是其中一种可能的方法。


```c
long strtohextoval()
{
    long val=0,value=0;
    int i=1,last=1,current;
    int a=0,b=0;

        for(i=0;i<=15;i++)
        {
         current = (int) NumStr.val[i];
         a= current % 16; //Obtain LSD
         b= current / 16; //Obtain MSD
            
         value += last* a;   
         last = last * 16;   
         value += last* b;
         last = last * 16;   
        }
     return value;
}
```


### <a name="applies-to-versions"></a>适用于版本


有关 SQL 数值结构的上述信息 \_ \_ 适用于以下产品版本：

- 用于 Microsoft SQL Server 3.7 的 Microsoft ODBC 驱动程序
- Microsoft 数据访问组件2。1
- Microsoft 数据访问组件2。5
- Microsoft 数据访问组件2。6
- Microsoft 数据访问组件2。7


## <a name="sql_c_numeric-overview"></a>SQL \_ C \_ 数字概述


下面的示例程序 \_ \_ 通过在表中插入123.45，阐释了 SQL C 数字的用法。 在表中，列定义为数值或小数点，精度为5，小数位数为2。

用于运行此程序的 ODBC 驱动程序必须支持 ODBC 3.0 功能。


```c
#include <windows.h>
#include <sql.h>
#include <sqlext.h>

void main() {

   SQLHENV    henv  = NULL;
   SQLHDBC    hdbc  = NULL;
   SQLHSTMT   hstmt = NULL;

   SQL_NUMERIC_STRUCT   NumStr;
   SQLINTEGER           cbNumStr = sizeof (NumStr);

   SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv);

   /* Set the ODBC behavior version. */ 
   SQLSetEnvAttr(henv,
         SQL_ATTR_ODBC_VERSION,
         (SQLPOINTER) SQL_OV_ODBC3,
         SQL_IS_INTEGER);

   SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);

   /* Substitute your own connection information */ 
   SQLConnect(hdbc,
      (SQLCHAR *) "MyDSN", 5,
      (SQLCHAR *) "UserID", 6,
      (SQLCHAR *) "Password", 8);

   SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);

   /*
      Set up the SQL_NUMERIC_STRUCT, NumStr, to hold "123.45".

      First, we need to scale 123.45 to an integer: 12345
      One way to switch the bytes is to convert 12345 to Hex:  0x3039
      Since the least significant byte will be stored starting from the
      leftmost byte, "0x3039" will be stored as "0x3930".

      The precision and scale fields are not used for input to the driver,
      only for output from the driver. The precision and scale will be set
      in the application parameter descriptor later.
   */ 

   NumStr.sign = 1;   /* 1 if positive, 2 if negative */ 

   memset (NumStr.val, 0, 16);
   NumStr.val [0] = 0x39;
   NumStr.val [1] = 0x30;

   /* SQLBindParameter needs to be called before SQLSetDescField */ 
   SQLBindParameter(hstmt,
          1,
          SQL_PARAM_INPUT,
          SQL_C_NUMERIC,
          SQL_NUMERIC,
          5,
          2,
          &NumStr,
          0,
          (SQLINTEGER *) &cbNumStr);

   /* Modify the fields in the implicit application parameter descriptor */ 
   SQLHDESC   hdesc = NULL;

   SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
   SQLSetDescField(hdesc, 1, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_PRECISION, (SQLPOINTER) 5, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_SCALE, (SQLPOINTER) 2, 0);
   SQLSetDescField(hdesc, 1, SQL_DESC_DATA_PTR, (SQLPOINTER) &NumStr, 0);

   SQLExecDirect(hstmt,
         (SQLCHAR *) "INSERT INTO table (numeric_column) VALUES (?)",
         SQL_NTS);

   SQLFreeHandle(SQL_HANDLE_STMT, hstmt);

   SQLDisconnect (hdbc);

   SQLFreeHandle(SQL_HANDLE_DBC, hdbc);
   SQLFreeHandle(SQL_HANDLE_ENV, henv);
}
```


<!--
GeneMi historical notes, 2017/July/12. Per Jason.C

https://go.microsoft.com/fwlink/?LinkId=147596

https://support.microsoft.com/help/222831

https://web.archive.org/web/20140319133434/http:/support.microsoft.com:80/kb/222831

https://web.archive.org/web/20080505073901/http:/support.microsoft.com:80/kb/181254

https://docs.microsoft.com/sql/odbc/reference/appendixes/c-data-types
-->

