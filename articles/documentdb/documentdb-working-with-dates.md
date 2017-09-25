---
title: Working with dates in Azure DocumentDB | Microsoft Docs
description: Learn about how to work with dates in Azure DocumentDB.
services: documentdb
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: ''

ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: arramac

---
# Working with Dates in Azure DocumentDB
DocumentDB delivers schema flexibility and rich indexing via a native [JSON](http://www.json.org) data model. All DocumentDB resources including databases, collections, documents, and stored procedures are modeled and stored as JSON documents. As a requirement for being portable, JSON (and DocumentDB) supports only a small set of basic types: String, Number, Boolean, Array, Object, and Null. However, JSON is flexible and allow developers and frameworks to represent more complex types using these primitives and composing them as objects or arrays. 

In addition to the basic types, many applications need the [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type to represent dates and timestamps. This article describes how developers can store, retrieve, and query dates in DocumentDB using the .NET SDK.

## Storing DateTimes
By default, the [DocumentDB SDK](documentdb-sdk-dotnet.md) serializes DateTime values as [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) strings. Most applications can use the default string representation for DateTime for the following reasons:

* Strings can be compared, and the relative ordering of the DateTime values is preserved when they are transformed to strings. 
* This approach doesn't require any custom code or attributes for JSON conversion.
* The dates as stored in JSON are human readable.
* This approach can take advantage of DocumentDB's index for fast query performance.

For example, the following snippet stores an `Order` object containing two DateTime properties - `ShipDate` and `OrderDate` as a document using the .NET SDK:

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

This document is stored in DocumentDB as follows:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

Alternatively, you can store DateTimes as Unix timestamps, that is, as a number representing the number of elapsed seconds since January 1, 1970. DocumentDB's internal Timestamp (`_ts`) property follows this approach. You can use the [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) class to serialize DateTimes as numbers. 

## Indexing DateTimes for range queries
Range queries are common with DateTime values. For example, if you need to find all orders created since yesterday, or find all orders shipped in the last five minutes, you need to perform range queries. To execute these queries efficiently, you must configure your collection for Range indexing on strings.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

You can learn more about how to configure indexing policies at [DocumentDB Indexing Policies](documentdb-indexing-policies.md).

## Querying DateTimes in LINQ
The DocumentDB .NET SDK automatically supports querying data stored in DocumentDB via LINQ. For example, the following snippet shows a LINQ query that filters orders that were shipped in the last three days.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on DocumentDB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

You can learn more about DocumentDB's SQL query language and the LINQ provider at [Querying DocumentDB](documentdb-sql-query.md).

In this article, we looked at how to store, index, and query DateTimes in DocumentDB.

## Next Steps
* Download and run the [Code samples on Github](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Learn more about [DocumentDB Query](documentdb-sql-query.md)
* Learn more about [DocumentDB Indexing Policies](documentdb-indexing-policies.md)
