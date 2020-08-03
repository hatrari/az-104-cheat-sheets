# Deploy and Manage Virtual Machines

## Create and configure a VM for Windows and Linux

### Configure high availability
* One of the __best advantages__ of using a cloud provider like Microsoft Azure is the affordable, __high availability__.
* To ensure __resiliency__, Azure's going to have a __minimum of three separate zones__ in all of the enabled __regions__.
* To get resiliency at a __more granular level__, we can use __availability sets__ (fault domains & update domains).
* `New-AzAvailabilitySet -ResourceGroupName group -Name name -Location location -PlatformUpdateDomainCount 2 -PlateformFaultDomainCount 2 -Sku "Aligned"`
* `az vm availability-set create`

### Configure VM attributes
* VM size: cpu, ram, data disks, max iops, temporary storage
* Azure Resource Manager based VMs make it pretty easy to change the size of a virtual machine after you deployed it.
* `Update-AzVM -VM $vm -ResourceGroupName group`
* `az vm resize -g group -n name --size 'Standard_A2_V2'`
* There are many familier of VM size: standard, general purpose, memory optimized, compute optimized, etc.
* Public inbound ports: http 80, https 443, ssh 22, rdp 3389
* Disk type:
  * __Premium SSD__ would be best for IO intensive enterprise workloads. It delivers a consistent performance with low latency.
  * __Standard SSD__ provides consistent performance for low IOPS workloads.
  * __Standard HDD__ is optimized for low cost mass storage where the access is infrequent.
* Each VM can have __three__ different __types__ of __disks__:
  * OS disk. It'll be persistent. It'll be stored in Azure storage. 
  * Temporary disk, which used for short term storage.
  * Data discs have a maximum capacity of 4095 gigabytes at this point in time.
* Under management tab:
  * Configure monitoring.
  * System assign manage identity allows you to __authenticate__, typically using something in __Azure Key Vault__ without having to store any credentials in code. 
  * You can use Azure Active Directory credentials.
  * Auto shutdown, basically allows you to shut this down daily. You see the shutdown time and the time zone, and a notification before shut down.
  * Enable backup.

### Deploy and configure scale sets
* A virtual machine scale set, enables the deployment and management of a collection of __identical__, __auto-scaling__ virtual machines.
* An Azure __load balancer__, will __distribute traffic__ to the VM __instances in the scale set__.
* You can __scale__ the number of VM's in the scale set __manually__, or define __rules to auto scale__, based on resource usage like, CPU, Memory demand and Network traffic.
* by default, a scale set consists of a __single placement group__, with a __maximum__ size of __100 VM's__, but if you __set that property to false__, on the scale set, they can then have __multiple placement groups__ and then give you a range of 0 to __1000 VM's__. By the way, using multiple placement groups, is often called a __large scale set__.

### Create and configure Azure Kubernetes Service (AKS)
* Kubernetes is container-based application management for the networking and storage component of containers.
* Kubernetes focuses on application container workloads and not the underlying infrastructure by providing a robust set of APIs.
* It allows you to build and run portable microservice applications in both stateful and stateless environments.
* You can also leverage your existing CI/CD tools to schedule and deploy your releases.
* AKS offers a managed service to lower complexity and management while coordinating upgrades.
* Azure Kubernetes Service has two main components: 
  * __Control plane__:
    * control plane will be configured and created automatically for you.
    * There's no cost for the control plane
    * __The scheduler component__: When you create and scale up applications, it's the scheduler that decides what nodes can run the workload and then starts them.
    * __The API server component__ offers the interaction for management tools such as Kube-control or the Kubernetes dashboard.
    * __The database component__ maintains the state of your Kubernetes cluster and configuration. It's a highly available key-value store within Kubernetes.
    * __The controller manager__ manages a number of smaller controllers that basically act as replicating pods and handle node operations. Kubernetes uses pods to run an instance of your application.
  * __The nodes__, each node (VM) contains:
    * __Kubelet__ is the Kubernetes engine that processes the orchestration requests from the control plane and the scheduling of running requested containers.
    * __Kube-proxy__ routes network traffic and manage IP addressing for services and pods.
    * __Container runtime__ allows containerized applications to run and interact with virtual resources.
    * __Container itself__.

### Create and configure Azure Container Instances (ACI)
* You can rapidly develop apps without managing virtual machines or having to introduce any new tools.
* You can focus on designing and building applications without the overhead of managing the infrastructure running them.
* You can use ACI to deploy additional compute power for those workloads that are demanding on an ad hoc basis.
* ACI offers hypervisor isolation for each container group for securing containers by running them isolation without a shared kernel.
* If you run out of capacity in your AKS-cluster, you can scale out extra pods in ACI without having to manage additional servers.
* Use cases for Azure Container Instances:
  * You can combine ACI with the ACI Logic Apps connector with Azure queues and Azure Functions, allowing you to build robust infrastructure, so you can easily and rapidly scale out your containers as needed.
  * You can use ACI for data processing, where source data is ingested, processed, and placed in a durable store, such as Azure Blob storage.
  * You can also use Azure Machine Learning to deploy a model as a web service on Azure Container Instances.

## Automate deployment of VMs

### Modify an Azure Resource Manager (ARM) template
* With ARM, you can create a template in JSON format that defines the infrastructure and configuration of your Azure solution.
* You can repeatedly deploy your solution throughout its lifecycle and be confident that your resources are deployed from a single truth.

### onfigure the location of new VMs
* on the json file, you can change location value under resources key.

## Manage Azure VMs

### Add data discs
* The four available disk (virtual hard disks - VHDs) types are:
  * Ultra SSD
  * Premium SSD
  * Standard SSD
  * Standard HDD
* New-AzDisk

### Add network interfaces
* A network interface allows an Azure VM to communicate with the internet, with Azure and on-premises resources.
* A VM can have __one or more__ interfaces.
* You can also __add or remove__ network interfaces from an existing VM as long as it's in the __stopped or deallocated__ state.
* There's reasons for having multiple network interfaces. For example, enabling other appliances like load balancers, firewalls, and various proxies. Or, you might want to isolate networks or isolate network bandwidth.
* get the VM configuration: Get-AzVM -Name -ResourceGroup
* get the info for the backend subnet: Get-AzVirtualNetwork
* create a virtual nic: New-AzNetworkInterface
* get the virtual nic info: Get-AzNetworkInterface
* add a virtual nic to the VM: Add-AzVMNetworkInterface

### Automate configuration management with PowerShell DSC
* __PowerShell Desired State Configuration (DSC)__: is a PowerShell Management platform that lets you manage your I.T and development infrastructure with what we call, Configuration As Code.
* DSC is a declarative platform for configuration, deployment, and management of systems.
* DSC consists of three primary components, the configurations, the resources you're configuring, and the LCM, the Local Configuration Manager.

### Manage VM sizing and locations
* Move-AzResource

### Redeploy VMs
* If you have problems troubleshooting RDP connections, and you will, or application-based access to Windows-based VMs, then redeploying the VM may help and this involves shutting down the VM, moving it to a new node within the infrastructure, and then powering it back on.
* you will lose any data stored on __temporary drives__ used by this VM.
* to redeploy a VM, this VM must be running.
* Set-AzVM -Redeploy -ResourceGroupName -Name

## Manage VM backups

### Configure a VM backup

### Manage Azure VM backups
* In the Azure portal, you can use the Recovery Services Vault dashboard to see a variety of things:
  * you can see the most recent backup
  * you can see the backup policy
  * you can see the total size of all backup snapshots
  * you can see the amount of VMs that are enabled for a backup
* if a VM has four discs, then, it will backup all four discs in __parallel__.

### Perform VM restore
* The restoration process should be tested and affirmed on a regular basis as part of the business continuity and/or disaster recovery plan.
* to restore a VM that __has encrypted disks__ you also have to __provide__ the Azure backup service __access to the key vault__ that's holding those keys.

### Azure Site Recovery
* Azure Site Recovery is Microsoft Azure's built-in disaster recovery as a service (DRaaS).
* The Azure Site Recovery service contributes to your business continuity and disaster recovery, or as they call it, BCDR.
* Site Recovery manages and orchestrates disaster recovery of on-premises machines and Azure VMs, including replication, failover, and recovery.
* You set up a Site Recovery by simply replicating a VM to a different Azure region directly from the portal.
* You can even sequence the order of recovering multi-tier applications running on multiple VMs.