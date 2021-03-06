---
title: SQL Server database migration to Azure SQL Database | Microsoft Docs
description: Learn how about on-premises SQL Server database migration to Azure SQL Database in the cloud. Use database migration tools to test compatibility prior to database migration.
keywords: database migration,sql server database migration,database migration tools,migrate database,migrate sql database
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: ''

ms.assetid: 9cf09000-87fc-4589-8543-a89175151bc2
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: sqldb-migrate
ms.date: 11/08/2016
ms.author: carlrab

---
# SQL Server database migration to SQL Database in the cloud
In this article, you learn to how to migrate an on-premises SQL Server 2005 or later database to Azure SQL Database. In this database migration process, you migrate your schema and your data from the SQL Server database in your current environment into SQL Database. To succeed, the existing database must first pass a compatibility test. With SQL Database V12, there we are approaching [feature parity](sql-database-features.md), other than issue related to server-level and cross-database operations. Databases and applications that rely on [partially or unsupported functions](sql-database-transact-sql-information.md) need some re-engineering to fix these incompatibilities before the SQL Server database can be migrated.

To migrate, the following are the steps you to take:

* **Test for Compatibility**: Validate database compatibility with SQL Database. 
* **Fix Compatibility Issues, if any**: If validation fails, you must fix the validation errors.  
* **Perform the migration** Once your database is compatible, you can use one or several methods to perform the migration. 

SQL Server provides several methods to accomplish each of these tasks. This article provides an overview of the available methods for each task. The following diagram illustrates the steps and the methods.

  ![VSSSDT migration diagram](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

> [!NOTE]
> To migrate a non-SQL Server database, including Microsoft Access, Sybase, MySQL Oracle, and DB2 to Azure SQL Database, see [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/).
> 
> 

## Database migration tools test SQL Server database compatibility with SQL Database
To test for SQL Database compatibility issues before you start the database migration process, use one of the following methods:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
> * [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT uses the most recent compatibility rules to detect SQL Database V12 incompatibilities. If incompatibilities are detected, you can fix detected issues directly in this tool. This method is the recommended method to test and fix SQL Database V12 compatibility issues. 
* [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage is a command-line utility that tests for compatibility issues and generates a report containing detected compatibility issues. If you use this tool, make sure you use the most recent version to use the most recent compatibility rules. If errors are detected, you must use another tool to fix any detected compatibility issues - SSDT is recommended.  
* [The Export Data Tier application wizard in SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): This wizard detects and reports errors to the screen. If not errors are detected, you can continue and complete the migration to SQL Database. If errors are detected, you must use another tool to fix any detected compatibility issues - SSDT is recommended.
* [SQL Azure Migration Wizard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW is a codeplex tool that uses the Azure SQL Database V11 compatibility rules to detect Azure SQL Database V12 incompatibilities. If incompatibilities are detected, some issues can be fixed directly in this tool. This tool may find incompatibilities that do not need to be fixed. It was the first Azure SQL Database migration assistance tool available and is actively supported by the SQL Server community. Also, this tool can complete the migration from within the tool itself. 

## Fix database migration compatibility issues
If compatibility issues are detected, you must fix them before proceeding with the SQL Server database migration. There are a wide variety of compatibility issues that you might encounter, depending both on the version of SQL Server in the source database and the complexity of the database you are migrating. Older versions of SQL Server have more compatibility issues. Use the following resources, in addition to a targeted Internet search using your search engine of choices:

* [SQL Server database features not supported in Azure SQL Database](sql-database-transact-sql-information.md)
* [Discontinued Database Engine Functionality in SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
* [Discontinued Database Engine Functionality in SQL Server 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
* [Discontinued Database Engine Functionality in SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
* [Discontinued Database Engine Functionality in SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
* [Discontinued Database Engine Functionality in SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

In addition to searching the Internet and using these resources, use the [MSDN SQL Server community forums](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) or [StackOverflow](http://stackoverflow.com/).

Use one of the following database migration tools to fix the issues detected:

> [!div class="op_single_selector"]
> * [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
> * [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
> * [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
> 
> 

* Use [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): To use SSDT, you import your database schema into SQL Server Data Tools for Visual Studio "SSDT") and build the project for a SQL Database V12 deployment. You then fix all detected compatibility issues in SSDT. When complete, you synchronize the changes back to the source database (or a copy of the source database. SSDT is currently the recommended method to test and fix SQL Database V12 compatibility issues. Follow the link for a [walk-through using SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
* Use [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md): To use SSMS, you execute Transact-SQL commands to fix the errors detected using another tool. This method is primarily for advanced users to modify the database schema directly in the source database. 
* Use [SQL Azure Migration Wizard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): To use SAMW, you generate a Transact-SQL script from the source database. The wizard transforms the script, whenever possible, to make the schema compatible with the SQL Database V12. When complete, SAMW can connect to SQL Database V12 to execute the script. This tool also analyzes trace files to determine compatibility issues. The script can be generated with schema only or can include data in BCP format.

## Migrate a compatible SQL Server database to SQL Database
To migrate a compatible SQL Server database, Microsoft provides several migration methods for various scenarios. The method you choose depends upon your tolerance for downtime, the size and complexity of your SQL Server database, and your connectivity to the Microsoft Azure cloud.  

> [!div class="op_single_selector"]
> * [SSMS Migration Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
> * [Export to BACPAC File](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
> * [Import from BACPAC File](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [Transactional Replication](sql-database-cloud-migrate-compatible-using-transactional-replication.md)
> 
> 

To choose your migration method, the first question to ask is if you can afford to take the database out of production during the migration. Migrating a database while active transactions are occurring can result in database inconsistencies and possible database corruption. There are many methods to quiesce a database, from disabling client connectivity to creating a [database snapshot](https://msdn.microsoft.com/library/ms175876.aspx).

To migrate with minimal downtime, use [SQL Server transaction replication](sql-database-cloud-migrate-compatible-using-transactional-replication.md) if your database meets the requirements for transactional replication. If you can afford some downtime or you are performing a test migration of a production database for later migration, consider one of the following three methods:

* [SSMS Migration Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): For small to medium databases, migrating a compatible SQL Server 2005 or later database is as simple as running the [Deploy Database to Microsoft Azure Database Wizard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) in SQL Server Management Studio.
* [Export to BACPAC File](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) and then [Import from BACPAC File](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): If you have connectivity challenges (no connectivity, low bandwidth, or timeout issues) and for medium to large databases, use a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file. With this method, you export the SQL Server schema and data to a BACPAC file. You then import the BACPAC file into SQL Database using the Export Data Tier Application Wizard in SQL Server Management Studio or the [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) command-prompt utility.
* Use BACPAC and BCP together: Use a [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) file and [BCP](https://msdn.microsoft.com/library/ms162802.aspx) for much larger databases to achieve greater parallelization for increases performance, albeit with greater complexity. With this method, migrate the schema and the data separately.
  
  * [Export the schema only to a BACPAC file](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
  * [Import the schema only from the BACPAC File](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) into SQL Database.
  * Use [BCP](https://msdn.microsoft.com/library/ms162802.aspx) to extract the data into flat files and then [parallel load](https://technet.microsoft.com/library/dd425070.aspx) these files into Azure SQL Database.
    
     ![SQL Server database migration - migrate SQL database to the cloud.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## Next steps
* [Newest version of SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
* [Newest version of SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## Additional resources
* [SQL Database feaures](sql-database-features.md)
  [Transact-SQL partially or unsupported functions](sql-database-transact-sql-information.md)
* [Migrate non-SQL Server databases using SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/)

