# Implement and manage storage

## Create and configure storage accounts

### Configure network access to the storage account
* __Azure storage account__ holds all of your Azure Storage data objects like blobs, files, queues, tables and disks.
* __Storage account__ represents a unique namespace for your Azure Storage data accessible over HTTP or HTTPS from anywhere.
* Data in the Azure storage account is durable, highly available, secure and vastly scalable.
* The main account types are:
  * general-purpose v2 (GPv2)
  * general-purpose v1 (GPv1)
  * block blob storage accounts
  * FileStorage accounts
  * blob storage accounts
* (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName group -AccountName account).DefaultAction
* Update-AzStorageAccountNetworkRuleSet -ResourceGroupName group -Name account -DefaultAction Allow
* az storage account show
* az storage account update

### Create and configure storage accounts
* New-AzStorageAccount -ResourceGroupName group -Name name -Location location -SkuName Standard_RAGRS -Kind StorageV2
* az storage account create --name name --resource-group group --location location --sku Standard_RAGRS --kind StorageV2
* New-AzResourceGroupDeployment -ResourceGroupName name -TemplateUri "https//.../template.json"
* az group deployment create --resource-group group --tempalate-file "https//.../template.json"

### Generate a shared access signature (SAS)
* __Shared Access Signature (SAS)__ lets you grant limited access to objects in your storage account to other clients without exposing and possibly compromising your account key.
* SAS token gives you granular control over the type of access you grant to clients who have the SAS, including the SAS validity interval, SAS permissions and IP address or addresses, and you can restrict them to HTTPS as supposed to HTTP.
* New-AzStorageAccountSASToken
* New-AzStorageContainerSASToken
* New-AzStorageBlobSASToken
* New-AzStorageTableSASToken
* New-AzStorageFileSASToken
* New-AzStorageShareSASToken
* New-AzStorageQueueSASToken

### Manage access keys
* Each access storage account has two keys, a __primary__ key and a __secondary__ key.
* These two keys are used to __authenticate__ application request to your storage accounts.
* Whoever has access to these keys has __unlimited access__ to the entire storage account.
* It's highly recommended that you store the access keys in a secure location, like Azure Key Vault.
* __Never share__ the keys. Whenever possible, let's use Shared Access Signatures (SAS).
* __Regenerate__ your keys on a regular basis.
* __Azure Storage Explorer__ is a standalone application that lets us effortlessly work with Azure Storage data on __Windows__, __MacOS__, and __Linux__ system.
* To regenerate keys:
  * New-AzStorageAccountKey
  * az storage account keys renew

### Monitor activity logs using Log Analytics
* __Azure Monitor log__ data is still __stored__ in the __Log Analytics workspace__ and is still collected and analyzed by the same Log Analytics service, but Microsoft is changing the term "Log Analytics" in many places to Azure Monitor Logs.
* Types of data collected by Azure Monitor: __metrics__ or __logs__.
* Advantages of connecting the Activity Log to a Log Analytics workspace:
  * __Consolidate__ the Activity Log from multiple Azure subscriptions into a single location for analysis.
  * Store Activity Log entries for __more than 90__ days.
  * __Correlate__ Activity Log data with other monitoring data collected by Azure Monitor.
  * Use __log queries__ to perform __complex analysis__ and gain deep insights on Activity Log entries.

### Implement Azure storage replication
* Data in your storage account is __always replicated__ to guarantee durability and high availability.
* Azure Storage copies your data so that it is protected from planned and unplanned events, including:
  * transient hardware failures
  * network outages
  * power outages
  * or substantial natural disasters
* Four options to replicate your data:
  * locally redundant storage (LRS)
  * zone-redundant storage (ZRS)
  * geo-redundant storage (GRS) (only supported by GPv2 storage account)
  * read-access geo-redundant storage (RA-GRS)

### Configure Azure AD Authentication for a storage account
* Azure AD lets you implement RBAC to give permissions to users, groups or application service principals.
* Azure storage defines a set of built-in RBAC roles, made up of common permission sets that we use to access Blob and Queue data.
* Principals are authenticated by AD to return an OAuth 2.0 security token, which then authorizes a request against Blob or Queue storage.
* Only storage accounts created with the ARM deployment model, support Azure AD authorization.
* Azure files supports authorization with Azure AD over server message block SMB for domain-joined VMs only.

## Import and export data to Azure

### Create imports and exports with Azure jobs
* Azure Import/Export service will import large amounts of data to Azure Blob storage and Azure Files by shipping disk drives to the Azure datacenter.
* The service can also be used to transfer data from Azure Blob storage to disk drives and ship to your on-premises sites.
* Examples of use cases:
  * __Migration__: Move large amounts of data to Azure quickly and cost effectively.
  * __Content distribution__: to quickly send data to your customer sites.
  * __Backup__: Take backups of your on-premises data to store in Azure Storage.
  * __Data recovery__: recover large amounts of data in storage and have it delivered to your on-premises location.
* Process of the import job flow (creating, shipping, transferring, packaging, complete):
  1. The customer prepares the hard drives using the import/export client tool and encrypt the drive with BitLocker
  2. the customer creates an import job using the Azure Portal
  3. the customer ships the hard drivers to the datacenter
  4. the carrier delivers the hard drivers to the datacenter
  5. the hard driver are processed at the datacenter
  6. the data is copied from the hard drivers to the storage account
  7. the hard drives are packaged for return shipping
  8. the hard drivers are shipped back to the customer
* Process of the export job flow (creating, shipping, transferring, packaging, complete):
  1. the customer creates an export job using the Azure portal
  2. the customer ships the hard drives to the datacenter
  3. the carrier delivers the hard drives to the datacenter
  4. the hard drives are processed at the datacenter
  5. the data is copied from the storage account to the hard drives 
  6. the hard drives are encrypted with BitLocker
  7. the hard drives are packaged for return shipping
  8. the hard drives are shipping back to the customer

### Use Azure Data Box
* Data Box lets you move stored or in-flight data to Azure quickly and cost-effectively.
* It offers __online__ appliances (Data Box Gateway or Data Box Edge) to transfer data to and from Azure over the network.
* It offers __offline__ data transfer products to move __large amounts__ of data to Azure.

### Configure and use Azure blob storage
* Blob storage is scalable, cost-effective cloud storage for all __unstructured__ data.
* There's four storage tiers:
  * __Premium__ is for performance-sensitive data
  * __Hot__ is for frequently accessed data
  * __Cool__ is for infrequently accessed data
  * __Archive__ is for data that's rarely accessed
* The storage service offers three types of blobs:
  * __Block blobs__ give you up to 100 megabytes per block, times 50,000, or 4.75 terabytes, they are optimized for efficient uploads and downloads
  * __Page blobs__ are 512 byte pages optimized for __random read and write ops__
  * __Append blobs__ which are blocks added to the end of a blob, they are optimized for append operations. They don't support modification of existing blob contents.
* The type of blob is set when you create it. And __I can't go and change__ this after the fact.

### Configure Azure CDN endpoints
* Azure Content Delivery Network (CDN) let's you reduce load times, save bandwidth, and speed responsiveness. 
* It's used for websites, mobile apps, encoding and distributing streaming media, gaming software, firmware updates, and IoT endpoints.
* CDN gives you global coverage and immense scalability.
* It integrates into your other Azure services, allowing for scaling in minutes.
* You get easy setup and no commitment, and of course pay-as-you-go pricing.
* CDN endpoints allow you to deliver content to your customers. Basically, you'll just first create a CDN profile, which is the logical container for the endpoints, then you can create the endpoint in the Azure Portal, or you can do it programmatically.
* You must create a CDN Profile first, before you can create endpoints.

### Configure storage tiers for Azure blobs
* You can optimize storage costs by choosing from three available access tiers:
  * Hot: for data that's __frequently__ accessed. It has a __higher storage cost__ than the others, but the __lowest access cost__.
  * Cool: is for data that's accessed __less frequently__ and __stored for at least 30 days__. It has __lower storage costs__ and __higher access costs compared to hot__ storage. This could be used for short term backup and disaster recovery data.
  * Archive: Use this for rarely accessed data and __store it for at least 180 days__. If the data doesn't remain for at least 180 days, it'll be subject to an early-deletion charge. If your blob is in archive storage, the blob data is __offline__ and it can't be read, you can't overwrite it, it can't be changed. So, to read or download a blob in archive, you have to __re-hydrate__ it to an online tier.

## Configure Azure files

### Create Azure file share
* Azure provides fully managed cloud file shares that are available using the server message block, SMB protocol.
* Azure file shares can be mounted concurrently by cloud or on-premises deployments of Windows, Linux, and macOS.
* Also, Azure file shares can be cached on Windows Servers with Azure File Sync for fast access near where the data's being used.
* File shares can replace or append to on-premise file servers.
* They can support lift-and-shift applications.
* They can make cloud development easier.
* Everything that happens in Azure file storage basically is through the Azure storage account.
* Get-AzStorageAccountkey -ResourceGroupName group -Name account
* New-AzStorageContext -StorageAccountName account -StorageAccountKey key
* New-AzStorageShare -Name name -Context context
* az storage account show-connection-string -n account -g group
* az storage share create --name name --quote 5000 --connection-string string

### Create Azure File Sync service
* Azure Files provides two data access methods:
  * direct cloud access
  * Azure File Sync
* With Azure File Sync, shares can be replicated to Windows Servers on-premises or in Azure.
* Users can access the file share using an SMB or NFS share.
* And data can be replicated between multiple Windows Server endpoints.
* Data can be tiered to Azure Files, so that all of the data is still reachable through the server, but the server doesn't have to have a full copy. Instead, the data is seamlessly recalled when opened by the user.

### Troubleshoot Azure File Sync

## Implement Azure backup

### Create recovery services vault
* A __recovery services vault__ is a __storage entity__ that typically holds copies of data of configuration information for VMs, workloads, servers, and workstations.
* It supports various services like IaaS VMs and Azure SQL databases.
* New-AzRecoveryServicesVault -Name name -ResourceGroupName group -Location lacotion
* az backup vault create -n name -g group -l location

### Backup and restore data
* Backup protects and restores data in the Microsoft Cloud.
* It replaces your existing on-premises or off-site backup solution with a dependable, secure, and cost-effective cloud-based solution.
* Azure Backup Advantages include:
   * automatic storage management
   * unlimited scaling
   * multiple storage choices
   * unlimited data transfer
   * data encryption
   * application-consistent backup
   * long-term retention
* Backup strategy is only as solid as your ability to restore.

### Configure and review backup reports
* Azure Backup reports are supported for Azure virtual machine backup and file and folder backup to the cloud by using the Azure Recovery Services Agent.
* You can view reports across vaults and across subscriptions.
* The frequency of scheduled refresh for the reports is 24 hours in Power BI.
* You can also perform an ad hoc refresh of the reports in Power BI where the latest data in the customer storage account is used to render reports.
* The prerequisites are that you create an Azure storage account to configure it for reports, that you create a Power BI account, and then register the resource provider Microsoft.insights, if it's not registered already.

### Create and configure backup policy
* With a __full backup__, each backup copy contains the __entire data source__. Full backups consume a large amount of network bandwidth and storage, each time a backup copy is transferred.
* With a __differential backup__, you're gonna store only the blocks that changed since the last initial full backup. This results in a smaller amount of network and storage consumption. Differential backups however are inefficient.
* An __incremental backup__ offers __high storage and network efficiency__ by only storing data blocks that changed since the previous backup. There's no need for regular full backups. It moves less data and it saves storage and network resources, thereby lowering the total cost of ownership, TCO. 