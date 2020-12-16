---
description: 临时表安全性
title: 临时表安全性 | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 60e5d6f6-a26d-4bba-aada-42e382bbcd38
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 244a625cb41d05f0d2f7cf61e8d6631a85dc8a4e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462558"
---
# <a name="temporal-table-security"></a>临时表安全性


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


要了解适用于临时表的安全性，理解适用于临时表的安全主体尤为重要。 在了解这些安全原则后，你就可以准备深入研究关于 **CREATE TABLE**、 **ALTER TABLE** 和 **SELECT** 语句的安全性了。

## <a name="security-principles"></a>安全原则

 下表描述适用于临时表的安全原则：

|原则|说明|
|---------------|-----------------|
|启用/禁用系统版本控制需要有对受影响对象的最高权限|启用和禁用 SYSTEM_VERSIONING 需要有对当前和历史记录表的 CONTROL 权限|
|不能直接修改历史记录数据|当 SYSTEM_VERSIONING 为 ON 时，用户不能更改历史记录数据，不管他们对当前和历史记录表的实际权限是什么。 这包括数据和架构修改。|
|查询历史记录数据需要有对历史记录表的 **SELECT** 权限。|仅仅因为用户具有对当前表的 **SELECT** 权限并不意味着他们有对历史记录表的 **SELECT** 权限。|
|审核曲面操作会以特定方式影响历史记录表：|当前表中的审核设置不会自动应用于历史记录表。 需要为历史记录表显式启用审核。<br /><br /> 启用后，对历史记录表的审核会定期捕获对访问数据的所有直接尝试（不管它们是否成功）。<br /><br /> 具有临时查询扩展的 **SELECT** 显示历史记录表受到了此操作的影响。<br /><br /> **CREATE/ALTER** 临时表也会公开历史记录表上的权限检查信息。 审核文件将包含历史记录表的其他记录。<br /><br /> 对当前表曲面进行 DML 操作，历史记录表受到了影响，但 additional_info 提供必要的上下文（DML 是 system_versioning 的结果）。|

## <a name="performing-schema-operations"></a>执行架构操作

当 SYSTEM_VERSIONING 设置为 ON 时，架构修改操作将受限制。

### <a name="disallowed-alter-schema-operations"></a>禁止的 ALTER 架构操作

|Operation|当前表|历史记录表|
|---------------|-------------------|-------------------|
|**DROP TABLE**|已禁止|已禁止|
|**ALTER TABLE...SWITCH PARTITION**|仅 SWITCH IN（请参阅 [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)）|仅 SWITCH OUT（请参阅 [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)）|
|**ALTER TABLE…DROP PERIOD**|已禁止|-|
|**ALTER TABLE…ADD PERIOD**|-|已禁止|

## <a name="allowed-alter-table-operations"></a>允许的 ALTER TABLE 操作

|Operation|当前|历史记录|
|---------------|-------------|-------------|
|**ALTER TABLE...REBUILD**|已允许（独立）|已允许（独立）|
|**CREATE INDEX**|已允许（独立）|已允许（独立）|
|**CREATE STATISTICS**|已允许（独立）|已允许（独立）|

## <a name="security-of-the-create-temporal-table-statement"></a>CREATE Temporal TABLE 语句的安全性

| Feature | 创建新的历史记录表 | 重用现有的历史记录表 |
| ------- | ------------------------ | ---------------------------- |
|所需的权限|数据库中的 **CREATE TABLE** 权限<br /><br /> 在其中创建当前和历史记录表的架构上的 **ALTER** 权限|数据库中的 **CREATE TABLE** 权限<br /><br /> 将在其中创建当前表的架构上的 **ALTER** 权限<br /><br /> 指定其作为创建临时表的 **CONTROL** 语句的一部分的历史记录表上的 **CONTROL** 权限|
|审核|审核显示用户尝试创建两个对象。 操作可能由于缺少在数据库中创建表的权限或缺少改变任一表的架构的权限而失败。|审核显示临时表已创建。 操作可能由于缺少在数据库中创建表的权限、缺少改变临时表的架构的权限或缺少对历史记录表的权限而失败。|

## <a name="security-of-the-alter-temporal-table-set-system_versioning-onoff-statement"></a>ALTER Temporal TABLE SET (SYSTEM_VERSIONING ON/OFF) 语句的安全性

| Feature | 创建新的历史记录表 | 重用现有的历史记录表 |
| ------- | ------------------------ | ---------------------------- |
|所需的权限|数据库中的 **CONTROL** 权限<br /><br /> 数据库中的 **CREATE TABLE** 权限<br /><br /> 在其中创建历史记录表的架构上的 **ALTER** 权限|被更改的原始表上的 **CONTROL** 权限<br /><br /> 指定其作为 **CONTROL** 语句的一部分的历史记录表上的 **CONTROL** 权限|
|审核|审核显示临时表已更改，同时创建了历史记录表。 操作可能由于缺少在数据库中创建表的权限、缺少改变历史记录表的架构的权限或缺少修改临时表的权限而失败。|审核显示临时表已更改，但操作需要对历史记录表的访问权限。 操作可能由于缺少对历史记录表或当前表的权限而失败。|

## <a name="security-of-select-statement"></a>SELECT 语句的安全性

**SELECT** 权限对不影响历史记录表的 **SELECT** 语句不变。 对于影响历史记录表的 **SELECT** 语句，在当前表和历史记录表上都需要 **SELECT** 权限。

## <a name="see-also"></a>另请参阅

- [临时表](../../relational-databases/tables/temporal-tables.md) 
- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
