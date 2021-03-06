---
title: Known issues with Azure Data Lake Storage Gen2 | Microsoft Docs
description: Learn about the limitations and known issues with Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: normesta
ms.reviewer: jamesbak
---
# Known issues with Azure Data Lake Storage Gen2

This article lists the features and tools that are not yet supported or only partially supported with storage accounts that have a hierarchical namespace (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## Issues and limitations with using Blob APIs

Blob APIs and Data Lake Storage Gen2 APIs can operate on the same data.

This section describes issues and limitations with using blob APIs and Data Lake Storage Gen2 APIs to operate on the same data.

* You can't use both Blob APIs and Data Lake Storage APIs to write to the same instance of a file. If you write to a file by using Data Lake Storage Gen2 APIs, then that file's blocks won't be visible to calls to the [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list) blob API. You can overwrite a file by using either Data Lake Storage Gen2 APIs or Blob APIs. This won't affect file properties.

* When you use the [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) operation without specifying a delimiter, the results will include both directories and blobs. If you choose to use a delimiter, use only a forward slash (`/`). This is the only supported delimiter.

* If you use the [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) API to delete a directory, that directory will be deleted only if it's empty. This means that you can't use the Blob API delete directories recursively.

These Blob REST APIs aren't supported:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob)
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url)

Unmanaged VM disks are not supported in accounts that have a hierarchical namespace. If you want to enable a hierarchical namespace on a storage account, place unmanaged VM disks into a storage account that doesn't have the hierarchical namespace feature enabled.

## Support for other Blob Storage features

The following table lists all other features and tools that are not yet supported or only partially supported with storage accounts that have a hierarchical namespace (Azure Data Lake Storage Gen2).

| Feature / Tool    | More information    |
|--------|-----------|
| **Data Lake Storage Gen2 APIs** | Partially supported <br><br>In the current release, you can use Data Lake Storage Gen2 **REST** APIs to interact with directories and set access control lists (ACLs), but there are no other SDKs (For example: .NET, Java, or Python) to perform those tasks. To perform other tasks such as uploading and downloading files, you can use the Blob SDKs.  |
| **AzCopy** | Version-specific support <br><br>Use only the latest version of AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Earlier versions of AzCopy such as AzCopy v8.1, are not supported.|
| **Azure Blob Storage lifecycle management policies** | All access tiers are supported. The archive access tier is currently in preview. The deletion of blob snapshots is not yet supported. |
| **Azure Content Delivery Network (CDN)** | Not yet supported|
| **Azure search** |Supported (Preview)|
| **Azure Storage Explorer** | Version-specific support <br><br>Use only versions `1.6.0` through `1.10.0`. <br> Version `1.10.0` is available as a [free download](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-relnotes). Version `1.11.0` is not yet supported.|
| **Blob container ACLs** |Not yet supported|
| **Blobfuse** |Not yet supported|
| **Custom domains** |Not yet supported|
| **Storage Explorer in the Azure portal** | Limited support. ACLs are not yet supported. |
| **Diagnostic logging** |Diagnostic logs are supported (Preview).<br><br>Enabling logs in the Azure portal is not currently supported. Here's an example of how to enable the logs by using PowerShell. <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`. <br><br>Make sure to specify `Blob` as the value of the `-ServiceType` parameter as shown in this example. <br><br>Currently, Azure Storage Explorer can't be used for viewing diagnostic logs. To view logs, please use AzCopy or SDKs.
| **Immutable storage** |Not yet supported <br><br>Immutable storage gives the ability to store data in a [WORM (Write Once, Read Many)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) state.|
| **Object-level tiers** |Cool and archive tiers are supported. The archive tier is in preview. All other access tiers are not yet supported.|
| **Powershell and CLI support** | Limited functionality <br><br>Blob operations are supported. Working with directories and setting access control lists (ACLs) is not yet supported. |
| **Static websites** |Not yet supported <br><br>Specifically, the ability to serve files to [Static websites](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Third party applications** | Limited support <br><br>Third party applications that use REST APIs to work will continue to work if you use them with Data Lake Storage Gen2. <br>Applications that call Blob APIs will likely work.|
|**Soft Delete** |Not yet supported|
| **Versioning features** |Not yet supported <br><br>This includes  [soft delete](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete), and other versioning features such as [snapshots](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob).|


