---
title: 'Azure portal: Export an Azure SQL database to a BACPAC file | Microsoft Docs'
description: Export an Azure SQL database to a BACPAC file  using the Azure Portal
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: ''

ms.assetid: 41d63a97-37db-4e40-b652-77c2fd1c09b7
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 12/20/2016
ms.author: sstein;carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA

---
# Export an Azure SQL database to a BACPAC file using the Azure portal
> [!div class="op_single_selector"]
> * [Azure portal](sql-database-export.md)
> * [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)
> * [PowerShell](sql-database-export-powershell.md)
> 

This article provides directions for exporting your Azure SQL database to a BACPAC file (stored in Azure blob storage) using the [Azure portal](https://portal.azure.com).

When you need to create an archive of an Azure SQL database, you can export the database schema and data to a BACPAC file. A BACPAC file is simply a ZIP file with an extension of BACPAC. A BACPAC file can later be stored in Azure blob storage or in local storage in an on-premises location and later imported back into Azure SQL Database or into a SQL Server on-premises installation. 

> [!IMPORTANT]
> Azure SQL Database Automated Export is now in preview and will be retired on March 1, 2017. Starting December 1, 2016, you will no longer be able to configure automated export on any SQL database. All your existing automated export jobs will continue to work until March 1, 2017. After December 1, 2016, you can use [long-term backup retention](sql-database-long-term-retention.md) or [Azure Automation](../automation/automation-intro.md) to archive SQL databases periodically using PowerShell periodically according to a schedule of your choice. For a sample script, you can download the [sample script from Github](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-automation-automated-export). 
>

## Considerations

* For an archive to be transactionally consistent, you must ensure either that no write activity is occurring during the export, or that you are exporting from a [transactionally consistent copy](sql-database-copy.md) of your Azure SQL database.
* The maximum size of a BACPAC file archived to Azure Blob storage is 200 GB. To archive a larger BACPAC file to local storage, use the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-prompt utility. This utility ships with both Visual Studio and SQL Server. You can also [download](https://msdn.microsoft.com/library/mt204009.aspx) the latest version of SQL Server Data Tools to get this utility.
* Archiving to Azure premium storage by using a BACPAC file is not supported.
* If the export operation exceeds 20 hours, it may be canceled. To increase performance during export, you can:
  * Temporarily increase your service level.
  * Cease all read and write activity during the export.
  * Use a [clustered index](https://msdn.microsoft.com/library/ms190457.aspx) with non-null values on all large tables. Without clustered indexes, an export may fail if it takes longer than 6-12 hours. This is because the export service needs to complete a table scan to try to export entire table. A good way to determine if your tables are optimized for export is to run **DBCC SHOW_STATISTICS** and make sure that the *RANGE_HI_KEY* is not null and its value has good distribution. For details, see [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [!NOTE]
> BACPACs are not intended to be used for backup and restore operations. Azure SQL Database automatically creates backups for every user database. For details, see [Business Continuity Overview](sql-database-business-continuity.md).  
> 

To complete this article, you need the following:

* An Azure subscription.
* An Azure SQL Database. 
* An [Azure Standard Storage account](../storage/storage-create-storage-account.md) with a blob container to store the BACPAC in standard storage.

## Export your database
Open the SQL Database blade for the database you want to export.

> [!IMPORTANT]
> To guarantee a transactionally consistent BACPAC file, you should first [create a copy of your database](sql-database-copy.md) and then export the database copy. 
> 
> 

1. Go to the [Azure portal](https://portal.azure.com).
2. Click **SQL databases**.
3. Click the database to archive.
4. In the SQL Database blade, click **Export** to open the **Export database** blade:
   
   ![export button][1]
5. Click **Storage** and select your storage account and blob container to store the BACPAC:
   
   ![export database][2]
6. Select your authentication type. 
7. Enter the appropriate authentication credentials for the Azure SQL server containing the database you are exporting.
8. Click **OK** to archive the database. Clicking **OK** creates an export database request and submits it to the service. The length of time the export takes depends on the size and complexity of your database, and your service level. View the notification you receive.
   
   ![export notification][3]

## Monitor the progress of the export operation
1. Click **SQL servers**.
2. Click the server containing the original (source) database you archived.
3. Scroll down to Operations.
4. In the SQL server blade click **Import/Export history**:
   
   ![import export history][4]

## Verify the BACPAC is in your storage container
1. Click **Storage accounts**.
2. Click the storage account where you stored the BACPAC archive.
3. Click **Containers** and select the container you exported the database into for details (you can download and save the BACPAC from here).
   
   ![.bacpac file details][5]    

## Next steps
* To learn about importing a BACPAC to an Azure SQL Database, see [Import a BACPCAC to an Azure SQL database](sql-database-import.md)
* To learn about importing a BACPAC to a SQL Server database, see [Import a BACPCAC to a SQL Server database](https://msdn.microsoft.com/library/hh710052.aspx)

<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

