---
description: 记录和流
title: 记录和流 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- streams [ADO]
- streams [ADO], about streams
- records [ADO]
ms.assetid: 4d68868e-2611-4b5c-9a89-7caa5f753151
author: rothja
ms.author: jroth
ms.openlocfilehash: 2de6347377cbdeb464941d766d732c23443efa2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032559"
---
# <a name="records-and-streams"></a>记录和流
ADO 目前提供 [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) 对象作为访问数据源中的信息（例如关系数据库）的主要方式。 但是，某些提供程序支持 [记录](../../../ado/reference/ado-api/record-object-ado.md) 和 [流](../../../ado/reference/ado-api/stream-object-ado.md) 对象作为替代对象或补充对象，这些对象可用于处理提供程序的数据。 有关 **记录** 行为的详细信息，请参阅提供程序的文档。  
  
## <a name="records"></a>记录  
 **记录** 对象实质上是一个行 **记录集**。 但是，与记录 **集** 相比，**记录** 的功能有限，它们具有不同的属性和方法。**记录** 对象中数据的源可以是一个命令，该命令从提供程序返回一行数据。 使用 **记录** 对象（而不是 **记录集** 对象）接收来自返回一行数据的查询的结果，从而消除了实例化更复杂的 **Recordset** 对象的开销。  
  
 **记录** 对象可用于其他目的，尤其是对于传统关系数据库之外的数据源的提供程序，例如 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 必须处理的大多数信息都存在，而不是数据库中的表，而是在电子邮件系统中的消息和新式文件系统中的文件。 **记录** 和 **流** 对象有助于访问存储在关系数据库以外的源中的信息。  
  
 **Record** 对象可以表示和管理数据，如文件系统中的目录和文件、电子邮件系统中的文件夹和邮件。 出于这些目的， **记录** 源可以是打开的 **记录集** 的当前行、绝对 url 或与打开的 [连接](../../../ado/reference/ado-api/connection-object-ado.md) 对象关联的相对 url。  
  
 通常， **记录集** 可用于表示层次结构（如文件夹或目录）中的容器或父项。 **记录** 可用于返回父容器中的一个节点（如文件或文档）的特定信息。 主要原因 **记录** 用于表示此类信息，这是因为这些数据源是异类数据源。 这意味着每个 **记录** 可能具有不同的字段集和数量。 包含来自数据库的行的传统 **记录集** 是同源的，这意味着每一行都具有相同的字段数量和类型。  
  
 有关使用 **Record** 对象处理来自 Internet 发布提供程序等提供程序的异类数据的详细信息，请参阅 [使用 ADO 进行 internet 发布](../../../ado/guide/data/using-ado-for-internet-publishing.md)。  
  
## <a name="streams"></a>流  
 **Stream** 对象提供读取、写入和管理字节流的方法。 此字节流可以是文本或二进制，并且仅限于系统资源。 通常，ADO **流** 对象用于以下目的：  
  
-   包含以 XML 格式保存的 **记录集** 的数据。 打开新 **记录集** 时，可以将来自保存的 **记录集** 的这些 XML 流用作源。 有关详细信息，请参阅 [流和暂留](../../../ado/guide/data/streams-and-persistence.md)。  
  
-   包含要对提供程序执行的 [CommandStreams](../../../ado/reference/ado-api/commandstream-property-ado.md) 作为 [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md)的替代项。 例如，XML Updategram 可用作针对 SQL Server 的 Microsoft OLE DB 提供程序的命令源。  
  
-   表示从不是 **记录集** 的格式接收来自提供程序的结果，如 Microsoft OLE DB provider for SQL SERVER 的 XML 结果。 有关详细信息，请参阅 [检索流中的结果](../../../ado/guide/data/retrieving-resultsets-into-streams.md)集。  
  
-   若要包含包含文件或消息的文本或字节，通常与提供程序（例如用于 Internet 发布的 Microsoft OLE DB 提供程序）一起使用。 有关 **流** 对象使用情况的详细信息，请参阅 [使用 ADO 进行 Internet 发布](../../../ado/guide/data/using-ado-for-internet-publishing.md)。  
  
 **流** 对象可以在上打开：  
  
-   使用 URL 指定的简单文件。  
  
-   包含 **流** 对象的 **记录** 或 **记录集** 的字段。  
  
-   表示目录或复合文件的 **记录** 或 **记录集** 对象的默认流。  
  
-   包含简单文件的 URL 的资源字段。  
  
-   根本没有特定的源。 在这种情况下，将在内存中打开一个 **流** 对象。 可以向其中写入数据，然后将其保存在另一个 **流** 或文件中。  
  
-   **记录集中** 的 BLOB 字段。  
  
 本部分包含以下主题。  
  
-   [流和暂留](../../../ado/guide/data/streams-and-persistence.md)  
  
-   [命令流](../../../ado/guide/data/command-streams.md)  
  
-   [检索流中的结果集](../../../ado/guide/data/retrieving-resultsets-into-streams.md)  
  
-   [使用 ADO 进行 Internet 发布](../../../ado/guide/data/using-ado-for-internet-publishing.md)
