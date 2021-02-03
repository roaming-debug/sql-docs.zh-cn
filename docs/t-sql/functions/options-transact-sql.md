---
description: '@@OPTIONS (Transact-SQL)'
title: '@@OPTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@OPTIONS'
- '@@OPTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- SET statement, current SET options
- '@@OPTIONS function'
- current SET options
ms.assetid: 3d5c7f6e-157b-4231-bbb4-4645a11078b3
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 43da748085a31a9d9f25ef20bdb690f2b94e4cb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183946"
---
# <a name="x40x40options-transact-sql"></a>@@OPTIONS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关当前 SET 选项的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
@@OPTIONS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **integer**  
  
## <a name="remarks"></a>注解  
 使用 SET 命令或 sp_configure user options 值可获得这些选项。 使用 SET 命令配置的会话值会替代 sp_configure 选项。 许多工具（例如 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 自动配置设置选项。 每个用户都有一个表示配置的 @OPTIONS 函数。  
  
 可以使用 SET 语句更改特定用户会话的语言和查询处理选项。 “\@\@ 选项”只能检测到设置为 ON 或 OFF 的选项。  
  
 “\@\@ 选项”函数返回选项的位图，转换为基数为 10 的（十进制）整数。 主题[配置 user options 服务器配置选项](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)提供的表中介绍了位设置的存放位置。  
  
 要解码“\@\@ 选项”值，将“\@\@选项返回的整数转换为二进制，然后查找[配置用户选择服务器配置选项](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)中的表中的值 。 例如，如果 `SELECT @@OPTIONS;` 返回值 `5496`，使用 Windows 编程人员计算器 (calc.exe) 将十进制 `5496` 转换为二进制。 结果为 `1010101111000`。 最右边的字符（二进制 1、2 和 4）为 0，指示表中的前三项为关闭状态。 查询该表，可以看到这三项是 DISABLE_DEF_CNST_CHK、IMPLICIT_TRANSACTIONS 和 CURSOR_CLOSE_ON_COMMIT。 下一项（`1000` 位置中的 ANSI_WARNINGS）为启用状态。 继续向左处理位图并向下处理选项列表。 如果最左边的选项是 0，则它们被类型转换截断。 位图 `1010101111000` 实际上是 `001010101111000`，代表全部 15 个选项。  
  
## <a name="examples"></a>示例  
  
### <a name="a-demonstration-of-how-changes-affect-behavior"></a>A. 更改如何影响行为的演示  
 下面的示例演示采用 CONCAT_NULL_YIELDS_NULL 选项的两个不同设置的连结行为的区别。  
  
```sql  
SELECT @@OPTIONS AS OriginalOptionsValue;  
SET CONCAT_NULL_YIELDS_NULL OFF;  
SELECT 'abc' + NULL AS ResultWhen_OFF, @@OPTIONS AS OptionsValueWhen_OFF;  
  
SET CONCAT_NULL_YIELDS_NULL ON;  
SELECT 'abc' + NULL AS ResultWhen_ON, @@OPTIONS AS OptionsValueWhen_ON;  
```  
  
### <a name="b-testing-a-client-nocount-setting"></a>B. 测试客户端 NOCOUNT 设置  
 以下示例设置 `NOCOUNT``ON` 并测试 `@@OPTIONS` 的值。 `NOCOUNT``ON` 选项可防止将会话中每一个语句的有关受影响行数的消息发送回请求的客户端。 `@@OPTIONS` 的值设置为 `512` (0x0200)。 这表示 NOCOUNT 选项。 下面的示例测试客户端是否启用了 NOCOUNT 选项。 例如，它可以帮助跟踪客户端的性能差异。  
  
```sql  
SET NOCOUNT ON  
IF @@OPTIONS & 512 > 0   
RAISERROR ('Current user has SET NOCOUNT turned on.', 1, 1)  
```  
  
## <a name="see-also"></a>另请参阅  
 [配置函数 (Transact-SQL)](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [配置 user options 服务器配置选项](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md)  
  
  
