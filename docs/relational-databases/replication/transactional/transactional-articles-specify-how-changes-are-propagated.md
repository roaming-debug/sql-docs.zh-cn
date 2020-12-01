---
title: 指定如何传播更改（事务性）
description: 了解如何为 SQL Server 中的事务发布指定传播更改的方式。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, propagation methods
ms.assetid: a10c5001-22cc-4667-8f0b-3d0818dca2e9
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: e7b267c0bfcfc7e12e2dc17c08bb6b18f2f36bf6
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "86919897"
---
# <a name="transactional-articles---specify-how-changes-are-propagated"></a>事务项目 - 指定如何传播更改
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  通过使用事务复制，可以指定如何将数据更改从发布服务器传播到订阅服务器。 对于每个已发布表，可以指定下列四种方法之一，将每项操作（INSERT、UPDATE 或 DELETE）传播到订阅服务器：  
  
-   指定事务复制应编写出脚本，并随后调用存储过程以将更改传播到订阅服务器（默认方法）。  
  
-   指定使用 INSERT、UPDATE 或 DELETE 语句传播更改（对于非[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 订阅服务器的默认方法）。  
  
-   指定应使用自定义存储过程。  
  
-   指定不应在任何订阅服务器上执行此操作。 不复制这种类型的事务。  
  
 默认情况下，事务复制通过安装在每个订阅服务器上的一组存储过程，将更改传播到订阅服务器。 当发布服务器的表上出现插入、更新或删除操作时，该操作转换为调用订阅服务器上的存储过程。 存储过程接受映射到表中列的参数，从而可以在订阅服务器上对这些列进行更改。  
  
 若要为事务项目的数据更改设置传播方法，请参阅 [为事务项目的数据更改设置传播方法](../../../relational-databases/replication/publish/set-the-propagation-method-for-data-changes-to-transactional-articles.md)。  
  
## <a name="default-and-custom-stored-procedures"></a>默认和自定义存储过程  
 默认情况下，复制为每个表项目创建的三个过程为：  
  
-   sp_MSins_\<** *tablename* **>，用于处理插入。  
  
-   sp_MSupd_\<** *tablename* **>，用于处理更新。  
  
-   sp_MSdel_\<** *tablename* **>，用于处理删除。  
  
 过程中使用的 \<**_tablename_**> 取决于如何将项目添加到发布中，以及订阅数据库是否包含名称相同但所有者不同的表。  
  
 所有这些过程都可以替换为在将项目添加到发布中时指定的自定义过程。 自定义过程用于应用程序需要自定义逻辑的情况，例如在订阅服务器上更新行时将数据插入审核表。 有关指定自定义存储过程的详细信息，请参阅上面列出的“如何”主题。  
  
 如果指定默认复制过程或自定义过程，还要为每个过程指定调用语法（如果使用默认过程，则复制选择默认语法）。 调用语法确定提供给过程的参数的结构以及对于每次数据更改向订阅服务器发送多少信息。 有关详细信息，请参阅本主题中的“存储过程的调用语法”部分。  
  
### <a name="considerations-for-using-custom-stored-procedures"></a>使用自定义存储过程的注意事项  
 请在使用自定义存储过程时注意下列事项：  
  
-   必须支持存储过程中的逻辑； [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 不支持自定义逻辑。  
  
-   为了避免与复制所用的事务产生冲突，不应在自定义过程中使用显式事务。  
  
-   订阅服务器的架构通常与发布服务器的架构相同，但如果使用了列筛选，则订阅服务器的架构也可以是发布服务器架构的子集。 但是，如果需要在移动数据时转换架构以使订阅服务器架构不是发布服务器架构的子集，则建议使用 [!INCLUDE[ssISCurrent](../../../includes/ssiscurrent-md.md)] 这种解决方案。 有关详细信息，请参阅 [SQL Server Integration Services](../../../integration-services/sql-server-integration-services.md)。  
  
-   如果对已发布表进行架构更改，则必须重新生成自定义过程。 有关详细信息，请参阅[重新生成自定义事务过程以反映架构更改](../../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md)。  
  
-   如果将大于 1 的值用于分发代理的 **-SubscriptionStreams** 参数，则必须确保成功更新主键列。 例如：  
  
    ```  
    update ... set pk = 2 where pk = 1 -- update 1  
    update ... set pk = 3 where pk = 2 -- update 2  
    ```  
  
     如果分发代理使用多个连接，则这两次更新可能会通过不同连接进行复制。 如果首先应用更新 1，则不会出现问题；如果首先应用更新 2，就会由于更新 1 尚未执行而返回“0 行受影响”。 在默认程序中，这种情况的处理方法是：如果更新未影响任何行，则产生一个错误：  
  
    ```  
    if @@rowcount = 0  
        if @@microsoftversion>0x07320000  
            exec sys.sp_MSreplraiserror 20598  
    ```  
  
     产生错误后，会强制分发代理通过单连接重试更新，这样更新就会成功。 自定义存储过程必须包括相似的逻辑。  
  
### <a name="call-syntax-for-stored-procedures"></a>存储过程的调用语法  
 用于调用事务复制所用过程的语法有五种：  
  
-   CALL 语法。 可用于插入、更新和删除操作。 默认情况下，复制将此语法用于插入和删除操作。  
  
-   SCALL 语法。 只能用于更新操作。 默认情况下，复制将此语法用于更新操作。  
  
-   MCALL 语法。 只能用于更新操作。  
  
-   XCALL 语法。 可用于更新和删除操作。  
  
-   VCALL。 用于可更新的订阅。 仅限内部使用。  
  
 各方法传播到订阅服务器的数据量不同。 例如，SCALL 仅涉及更新实际影响的列的值。 而 XCALL 则涉及到所有列（无论更新是否影响）以及每个列的所有旧数据值。 在很多情况下，SCALL 适用于更新，但如果应用程序在更新过程中涉及到所有数据值，则使用 XCALL。  
  
#### <a name="call-syntax"></a>CALL 语法  
 INSERT 存储过程  
 处理 INSERT 语句的存储过程将传递所有列的插入值：  
  
```  
c1, c2, c3,... cn  
```  
  
 UPDATE 存储过程  
 处理 UPDATE 语句的存储过程将传递在项目中定义的所有列的已更新值，后跟主键列的原始值（不尝试确定更改了哪些列。）：  
  
```  
c1, c2, c3,... cn, pkc1, pkc2, pkc3,... pkcn  
```  
  
 DELETE 存储过程  
 处理 DELETE 语句的存储过程将传递主键列的值：  
  
```  
pkc1, pkc2, pkc3,... pkcn  
```  
  
#### <a name="scall-syntax"></a>SCALL 语法  
 UPDATE 存储过程  
 处理 UPDATE 语句的存储过程将只传递已更改的那些列的更新值，后面依次是主键列的原始值和一个指明已更改列的位掩码 (**binary(n)** ) 参数。 在下面的示例中，列 2 (c2) 未更改：  
  
```  
c1, , c3,... cn, pkc1, pkc2, pkc3,... pkcn, bitmask  
```  
  
#### <a name="mcall-syntax"></a>MCALL 语法  
 UPDATE 存储过程  
 处理 UPDATE 语句的存储过程将传递在项目中定义的所有列的更新值，后面依次跟主键列的原始值和一个指示已更改列的位掩码 (**binary(n)** ) 参数：  
  
```  
c1, c2, c3,... cn, pkc1, pkc2, pkc3,... pkcn, bitmask  
```  
  
#### <a name="xcall-syntax"></a>XCALL 语法  
 UPDATE 存储过程  
 处理 UPDATE 语句的存储过程将传递在项目中定义的所有列的原始值（前像），后跟在项目中定义的所有列的更新值（后像）：  
  
```  
old-c1, old-c2, old-c3,... old-cn, c1, c2, c3,... cn,  
```  
  
 DELETE 存储过程  
 处理 DELETE 语句的存储过程将传递在项目中定义的所有列的原始值（前像）：  
  
```  
old-c1, old-c2, old-c3,... old-cn  
```  
  
> [!NOTE]  
>  使用 XCALL 时， **text** 列和 **image** 列的前像值应为 NULL。  
  
## <a name="examples"></a>示例  
 下列过程是为 `Vendor Table` 示例数据库中的 [!INCLUDE[ssSampleDBCoShort](../../../includes/sssampledbcoshort-md.md)] 创建的默认过程。  
  
```  
--INSERT procedure using CALL syntax  
create procedure [sp_MSins_PurchasingVendor]   
  @c1 int,@c2 nvarchar(15),@c3 nvarchar(50),@c4 tinyint,@c5 bit,@c6 bit,@c7 nvarchar(1024),@c8 datetime  
as   
begin   
insert into [Purchasing].[Vendor]([VendorID]  
,[AccountNumber]  
,[Name]  
,[CreditRating]  
,[PreferredVendorStatus]  
,[ActiveFlag]  
,[PurchasingWebServiceURL]  
,[ModifiedDate])  
values (   
 @c1  
,@c2  
,@c3  
,@c4  
,@c5  
,@c6  
,@c7  
,@c8  
 )   
end  
go  
  
--UPDATE procedure using SCALL syntax  
create procedure [sp_MSupd_PurchasingVendor]   
 @c1 int = null,@c2 nvarchar(15) = null,@c3 nvarchar(50) = null,@c4 tinyint = null,@c5 bit = null,@c6 bit = null,@c7 nvarchar(1024) = null,@c8 datetime = null,@pkc1 int  
,@bitmap binary(2)  
as  
begin  
update [Purchasing].[Vendor] set   
 [AccountNumber] = case substring(@bitmap,1,1) & 2 when 2 then @c2 else [AccountNumber] end  
,[Name] = case substring(@bitmap,1,1) & 4 when 4 then @c3 else [Name] end  
,[CreditRating] = case substring(@bitmap,1,1) & 8 when 8 then @c4 else [CreditRating] end  
,[PreferredVendorStatus] = case substring(@bitmap,1,1) & 16 when 16 then @c5 else [PreferredVendorStatus] end  
,[ActiveFlag] = case substring(@bitmap,1,1) & 32 when 32 then @c6 else [ActiveFlag] end  
,[PurchasingWebServiceURL] = case substring(@bitmap,1,1) & 64 when 64 then @c7 else [PurchasingWebServiceURL] end  
,[ModifiedDate] = case substring(@bitmap,1,1) & 128 when 128 then @c8 else [ModifiedDate] end  
where [VendorID] = @pkc1  
if @@rowcount = 0  
    if @@microsoftversion>0x07320000  
        exec sp_MSreplraiserror 20598  
end  
go  
  
--DELETE procedure using CALL syntax  
create procedure [sp_MSdel_PurchasingVendor]   
  @pkc1 int  
as   
begin   
delete [Purchasing].[Vendor]  
where [VendorID] = @pkc1  
if @@rowcount = 0  
    if @@microsoftversion>0x07320000  
        exec sp_MSreplraiserror 20598  
end   
go  
```  
  
## <a name="see-also"></a>另请参阅  
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
