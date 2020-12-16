---
title: OLE 自动化返回代码和错误信息 | Microsoft Docs
description: 详细了解 OLE 自动化返回代码，并了解如何使用示例存储过程显示 OLE 自动化错误信息。
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- return codes [SQL Server]
- OLE Automation [SQL Server], return codes
- OLE Automation [SQL Server], errors
ms.assetid: 9696fb05-e9e8-4836-b359-d4de0be0eeb2
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ef7b62123195fbc514350e2e600c3e5c1e5f88b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479138"
---
# <a name="ole-automation-return-codes-and-error-information"></a>OLE 自动化返回代码和错误信息
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  OLE 自动化系统存储过程返回一个 **int** 返回代码，该代码是基础 OLE 自动化操作返回的 HRESULT。 HRESULT 为 0 表示成功。 非零的 HRESULT 是 OLE 错误代码，其形式为十六进制 0x800 *nnnnn*，但是当作为存储过程返回代码中的一个 **int** 值返回时，HRESULT 的形式为 214 *nnnnnnn*。  
  
 例如，向 sp_OACreate 传递一个无效的对象名 (SQLDMO.Xyzzy) 会导致该过程返回一个值为 2147221005 的 **int** HRESULT，该值用十六进制表示则为 0x800401f3。  
  
 可以使用 `CONVERT(binary(4), @hresult)` 将 **int** HRESULT 转换为 **binary** 值。 但是，使用 `CONVERT(char(10), CONVERT(binary(4), @hresult))` 将产生一个不可读的字符串，因为 HRESULT 的每个字节都被转换成一个 ASCII 字符。 可以使用以下示例 HexToChar 存储过程将 **int** HRESULT 转换成包含可读十六进制字符串的 **char** 值。  
  
```  
USE AdventureWorks2012;  
GO  
IF EXISTS(SELECT name FROM sys.objects  
          WHERE name = N'dbo.sp_HexToChar')  
    DROP PROCEDURE HexToChar;  
GO  
CREATE PROCEDURE dbo.sp_HexToChar  
    @BinValue varbinary(255),  
    @HexCharValue nvarchar(255) OUTPUT  
AS  
    DECLARE @CharValue nvarchar(255);  
    DECLARE @Position int;  
    DECLARE @Length int;  
    DECLARE @HexString nchar(16);  
    SELECT @CharValue = N'0x';  
    SELECT @Position = 1;  
    SELECT @Length = DATALENGTH(@BinValue);  
    SELECT @HexString = N'0123456789ABCDEF';  
    WHILE (@Position <= @Length)  
    BEGIN  
        DECLARE @TempInt int;  
        DECLARE @FirstInt int;  
        DECLARE @SecondInt int;  
        SELECT @TempInt = CONVERT(int, SUBSTRING(@BinValue,@Position,1));  
        SELECT @FirstInt = FLOOR(@TempInt/16);  
        SELECT @SecondInt = @TempInt - (@FirstInt*16);  
        SELECT @CharValue = @CharValue +  
            SUBSTRING(@HexString, @FirstInt+1, 1) +  
            SUBSTRING(@HexString, @SecondInt+1, 1);  
        SELECT @Position = @Position + 1;  
    END  
    SELECT @HexCharValue = @CharValue;  
GO  
DECLARE @BinVariable varbinary(35);  
DECLARE @CharValue nvarchar(35);  
  
SET @BinVariable = 123456;  
  
EXECUTE dbo.sp_HexToChar  
    @binvalue = @BinVariable,  
    @HexCharValue = @CharValue OUTPUT;  
  
SELECT @BinVariable AS BinaryValue,  
    @CharValue AS CharacterRep;  
GO  
```  
  
 当其中一个 OLE 自动化过程返回非零的 HRESULT 返回代码时，可以使用以下示例存储过程 **sp_displayoaerrorinfo** 显示 OLE 自动化错误信息。 此示例存储过程使用 **HexToChar**。  
  
```  
CREATE PROCEDURE dbo.sp_DisplayOAErrorInfo  
    @Object int,  
    @HResult int  
AS  
    DECLARE @Output nvarchar(255);  
    DECLARE @HRHex nchar(10);  
    DECLARE @HR int;  
    DECLARE @Source nvarchar(255);  
    DECLARE @Description nvarchar(255);  
    PRINT N'OLE Automation Error Information';  
    EXEC sp_HexToChar @HResult, @HRHex OUT;  
    SELECT @Output = N'  HRESULT: ' + @HRHex;  
    PRINT @Output;  
    EXEC @HR = sp_OAGetErrorInfo  
        @Object,  
        @Source OUT,  
        @Description OUT;  
    IF @HR = 0  
    BEGIN  
        SELECT @Output = N'  Source: ' + @Source;  
        PRINT @Output;  
        SELECT @Output = N'  Description: '  
               + @Description;  
        PRINT @Output;  
    END  
    ELSE  
    BEGIN  
       PRINT N' sp_OAGetErrorInfo failed.';  
       RETURN;  
    END  
GO  
```  
  
## <a name="related-content"></a>相关内容  
 [sp_OAGetErrorInfo (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-oageterrorinfo-transact-sql.md)  
  
  
