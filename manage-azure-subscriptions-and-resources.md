# Manage Azure subscriptions and resources

## Manage Azure subscriptions

### Assign administrator permissions
* Users purchasing online subscriptions will create an organization ID or an OrgID, which is an account ID.
* The OrgID is owned by the account administrator (AA), who can manage accounts and subscriptions as well as perform tasks like deploying services for each subscription.
* Azure accounts have three roles, three core roles, that are applicable to all resources: __owner__, __contributor__, and __reader__.
* Each role corresponds to its own permission level. You can grant access permissions by assigning a role to a specific range of management groups, subscriptions, resource groups, apps, and individual users.
* A __maximum of 2,000 roles__ can be allocated to each __subscription__.

### Configure cost center quotas and tagging
* Microsoft will commonly use the phrase "limits" instead of "quotas". 
* There are service-specific limits (Active Directory, App Service, etc) that are applied with Azure Resource Manager (ARM).
* __Resource__ is a manageable item that is available through Azure.
* __Resource group__ is a container, a logical container, that holds related resources for an Azure solution. 
* __Resource provider__ is a service that supplies Azure resources.
* __Resource Manager template__ is a JSON file that defined resources to deploy to a resource group or a subscription.
* __Tags__ are metadata for organizing and categorizing cloud-based resources (up to __15__ tags per resource and resource group). You can use tags for resource __management__, __automation__, and __accounting__. And your tags can be IT-Aligned or they can be Business-Aligned.

### Configure Azure subscription policies at the subscription level
* Assignment is a policy that's been assigned to take place within a certain scope (subscription or resource group).
* Example of policy: Audit virtual machines without disaster recovery configured.

## Analyze resource utilization and consumption

### Configure diagnostic settings and baselines on resources
* __Tenant logs__: activities on services outside of the subscription such as Azure Active Directory logs.
* __Resource logs__: activities from services within a subscription like Network Security Groups or Storage Accounts.

### Create alerts
* Alerts proactively notify you when significant conditions occur in your monitoring data.
* __Alert rules__ is gonna capture the __target__ and the __criteria__ for alerting.
* the alert itself is only gonna fire, it's only going to __take the action__ when it is __enabled__.
* The __severity__ of the alert is specified in the alert rule set (0-4).
* __Target resource__ basically defines the scope, and it defines the signals that are gonna be available.
* Examples of targets: A virtual machine right, a storage group, a scale set, a virtual machines scale set.
* __Signals__ are emitted by the target resource.
* There are several different types of signals. It could be a metric, it could be an application insight, a signal could be a log, etc.
* __Criteria__ is a combination of a __signal__, and __logic__ that's applied on a target resource.
* We can create alerts from monitor in the menu (azur portal) or from resource blade.
* __Action group__ is a logical container that has one or more actions to be performed.
* Alerts can trigger Azure functions, they can trigger Webhooks, they can do automation Runbooks.

### Analyze alerts and metrics
There is a few things we can do with alerts and metrics. We can:
* __analyze__: use Metrics Explorer to analyze collected metrics on a chart.
* __visualize__: pin a chart from Metrics Explorer to an Azure dashboard.
* __do alerting__: configure a metric alert rule that sends a notification.
* __automate__: use autoscale to increase or decrease resources based on a metric value crossing a threshold.
* __export__: stream metrics to an event hub to route them to external systems.
* __retrieve__: access metric values from the command line or REST API.
* __archive__: archive the performance or health history of your resource for compliance, auditing, or offline reporting purposes.

### Create action groups
* __Action groups__ are sets of notification preferences defined by the Azure subscription owner.
* Azure Monitor and Service Health alerts use action groups to notify users that an alert has been triggered.
* You can configure up to __2,000 action groups__ in a __subscription__.
* You configure an action to notify a person by email or SMS.
* Examples of action type, the action performed: a voice call, SMS, email, or triggering various types of automated actions.
* you can also use __ARM templates__ as your resource manager templates to configure action groups.

### Monitor for unused resources
* __Azure Advisor__ offers personalized recommendations and guides you through the best practices to optimize your Azure resources. It provides guidance that helps you improve the __availability__, __security__, __performance__, and __cost__ effectiveness of your Azure resources.
* __Azure Resource Health__ assists you in diagnosing and getting support when an Azure issue impacts your resources. It informs you about the __current__ and __past health__ status of __your__ resources and helps you mitigate issues.

### Utilize log query functions
* Queries can start with either a table name or the search command.
* The __Kusto query language__ used by Azure Monitor is __case-sensitive__.
* Example of query: search in (Event) "error" | take 100

## Manage resource groups

### Use Azure policies for resource group
* You'll typically add resources that share the same life cycle to the same resource group making deployment, updates, and deletions a whole lot easier.
* create and manage policies on resource groups is valuable for maintaining corporate compliance.
* you can create a new policy using policy definition, and the policy rule is a JSON code.

### Configure resource locks on resource policies
* __Locking__ prevents other users in your organization, from accidentally modifying or deleting a resource group, a subscription or a specific resource.
* We have two lock types: Read only and delete.
* Only the __Owner__ and the __User Access Administrator__ roles are actually granted the action or the ability to create or delete locks.

### Implementing resource group tagging
* Tags let you mark resources with name value pairs to categorize and view them across resource groups and within the portal, across subscriptions.
* The tag name can have a __maximum of 512__ characters and is __NOT__ case sensitive.
* Tag names created by Azure have prefixes of Microsoft, Azure, or Windows, and you can't create tags with one of those prefixes.
* The main reasons why we're going to use tags is for __access controls (IAM) __, __compliance issues__, __global governance__, or for __automation purposes__.
* (Get-AzResource -ResourceName name).Tags
* az group show -n name --query tags

## Manage role-based access control (RBAC)

### Implement role-based access control (RBAC)
* Role based access control (RBAC) is an authorization system that let's you manage who has access to Azure resources, what they can do with the resources, and what areas they can access.
* RBAC is built on Azure resource manager to offer fine grained access management of Azure resources.
* When you use RBAC, you have the ability to partition, or group up, different duties or responsibilities with your personnel, and give __only__ the amount of access to users that they need to perform their jobs. We call this the "__least privilege principle__".
* We can assign role to:
  * __User__ is an individual who has a profile in Azure AD.
  * __Group__ is a set of users created in Azure active directory.
  * __Service__ is a security identity that's used by applications, or services, to access certain Azure resources.
  * __Managed identity__ is an identity in AD that's automatically managed by Azure. You typically use these when you're developing cloud applications to manage the credentials for authenticating to Azure services.
* __Role definition__ is a collection of permissions.
* __Scope__ is the set of resources that the access applies to.
* The __management group__ is the highest level in term of scope, then __subscription__, then __resource group__, and then __resources__.
* Only the __Owner__ and the __User Access Administrator__ roles are actually granted the action or the ability to create or delete role assignements.

### Assign RBAC Roles and create a custom role
* __Owner__, which has full access to all resources, including the capabilities to alter security or access rights for the resources that they're managing.
* __Contributor__ can create and manage resources, but they can't manage access rights to the resources.
* __Virtual Machine Contributor__ lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.
* __Readers__ who can view resources, but they can't create, manage or change access rights to resources.
* __User Access Administrator__ who can manage access rights to Azure resources.
* We can actually create our own custom roles for Azure resources.
* Custom roles __CANNOT__ be created through the __Azure portal__.
* We create custom roles using PowerShell, the CLI, and through the REST API.
* Get-AzRoleDefinition
* az role definition list
* New-AzRoleDefinition -InputFile C:\Temp\roleDefinition.json
* az role definition create --role-definition @ad-role.json

### Troubleshoot RBAC