<h1 align="center">The Azure🌍 hub-and-spoke-playground </h1>

<div align="center">
  A well-documented, easy-to-deploy network topology for testing, studying, inventing network configurations
</div>

<br/>

<div align="center">
  <sub>Built with ❤︎ by
  <a href="https://github.com/nicolgit">nicolgit</a> and
  <a href="https://github.com/nicolgit/hub-and-spoke-playground/contributors">
    contributors
  </a>
</div>

<br/>

<p align="center">
  <img src="images/hl-architecture.png" width="60%" />
</p>

_Download a [draw.io file](images/architecture.drawio) of this schema._

This repo contains a preconfigured Azure hub-and-spoke network topology, aligned to the Azure enterprise-scale landing zone reference architecture, deployable with a click on your subscription, useful for testing and studying network configurations in a controlled, repeatable environment.

As bonus many scenarios with step-by-step solutions for studying and learning are also available.

> _Read also this [blog post](https://nicolgit.github.io/azure-hub-and-spoke-playground/) for more info on this project._

The "playground" is composed by:
  * two hub and spoke network topologies aligned with with <a href="https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/ready/enterprise-scale/architecture" target="_blank">Microsoft Enterprise scale landing zone</a> reference architecture
  * two simulated on-premise architectures, deployed in 2 different regions, composed by network, client machine(s) and a gateway

## Deploy to Azure
You can use the following button to deploy the demo to your Azure subscription:

| | Available playgrounds| &nbsp; |
|---|---|---|
|1| the **HUB** playground<br/><sub>deploys `hub-lab-net` and spokes `01`-`02`-`03` | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fhub-01-bicep%2Fhub-01.json)
|2| deploys the **ON PREMISES** (France) playground | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fon-prem-bicep%2Fon-prem.json) |
|3| deploys the **ON PREMISES-2** (Germany) playground | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fon-prem-2-bicep%2Fon-prem-2.json) |
|4| deploys any-to-any routing and firewall rules<br/><sub>requires the HUB playground deployed</sub> | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fany-to-any-bicep%2Fany-to-any.json) |
|5| deploys a S2S VPN between on-prem and HUB<br/><sub>requires the HUB and one of the ON-PREMISES playgrounds deployed</sub>| [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fs2s-vpn-bicep%2Fconnect-on-prem.json)  |
|5| the HUB 02 playground<br/><sub>deploys `hub-lab-02-net` and spoke `04`</sub> | [![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fnicolgit%2Fhub-and-spoke-playground%2Fmain%2Fhub-02-bicep%2Fhub-02.json)  |

## Architecture
This diagram shows a detailed version with also all subnets, virtual machines, NVAs, IPs and Firewalls.
<br/>![Architecture](images/architecture.png)

_Download a [Visio file](images/hub-and-spoke-arc-drawing(s).vsdx) of this architecture._

the ARM template [hub-01-bicep](hub-01-bicep/hub-01.json) deploys:
* 4 Azure Virtual Networks:
    * `hub-lab-net` with 4 subnets:
        * default subnet: this subnet is used to connect the hub-vm-01 machine
        * AzureFirewallSubet: this subnet is used by Azure Firewall
        * AzureBastionSubnet: this subnet is used bu Azure Bastion
        * GatewaySubnet: this subnet is used by Azure Gateway
    * `spoke-01` with 2 subnets used to connect spoke-01-vm machine
    * `spoke-02` with 2 subnets used to connect spoke-02-vm machine
    * `spoke-03`, with 2 subnets and located in North Europe, used to connect spoke-03-vm machine
* An Azure Bastion resource that provides secure and seamless SSH connectivity to the jumpbox virtual machine directly in the Azure portal over SSL
* An Azure Firewall **premium** resource that provide a con-premiseic inspection.
* An Azure VPN Gateway resource that is used to send encrypted traffic between the hub virtual network to the on-premises simulated location.
* `hub-vm-01`: a Windows Server virtual machine that simulates a server located in the hub location
* `spoke-01-vm`: a Windows Server virtual machine that simulates a server located in the spoke-01 landing zone
* `spoke-02-vm`: a Windows Server virtual machine that simulates a server located in the spoke-02 landing zone
* `spoke-03-vm`: a Linux virtual machine that simulates a server located in the spoke-01 landing zone

The ARM template [on-prem](on-prem-bicep/on-prem.json) deploys:
* `on-prem-net`: an Azure Virtual Network located in France with 3 subnets
    * default subnet: this subnet is used to connect the w10-onprem-vm machine
    * AzureBastionSubnet: this subnet is used bu Azure Bastion
    * GatewaySubnet: this subnet is used by Azure Gateway
* An Azure Bastion resource that provides secure and seamless SSH connectivity to the jumpbox virtual machine directly in the Azure portal over SSL
* An Azure VPN Gateway resource that is used to send encrypted traffic between the hub virtual network to the on-premises simulated location.
* `w10-onprem-vm`: A Windows 10 VM with the objective to simulate a desktop client in an on-premise location

The ARM template [on-prem-2](on-prem-2-bicep/on-prem-2.json) deploys:
* `on-prem-2-net`: an Azure Virtual Network located in Germany with 3 subnets
    * default subnet: this subnet is used to connect the w10-onprem-vm machine
    * AzureBastionSubnet: this subnet is used bu Azure Bastion
    * GatewaySubnet: this subnet is used by Azure Gateway
* An Azure Bastion resource that provides secure and seamless SSH connectivity to the jumpbox virtual machine directly in the Azure portal over SSL
* An Azure VPN Gateway resource that is used to send encrypted traffic between the hub virtual network to the on-premises simulated location.
* `lin-onprem-vm`: A linux VM with the objective to simulate a linux client in an on-premise location

The ARM template [any-to-any](any-to-any-bicep/any-to-any.json) deploys:
* 2 routing tables that forward all spokes traffic to the firewall
* 1 IP Group and one Azure Firewall policy that:
  * allows spoke-to-spoke communication
  * block certain sites (2 web categories (nudity, Child Inappropriate, pornography)
  * allows all remaining HTTP(S) outbound traffic

the ARM template [hub-02](hub-02-bicep/hub-02.json) deploys:
* 2 Azure Virtual Networks:
    * `hub-lab-02-net` with 4 subnets:
        * default subnet: this subnet is empty
        * AzureFirewallSubet: this subnet is used by Azure Firewall
        * AzureBastionSubnet: this subnet is used bu Azure Bastion
        * GatewaySubnet: this subnet is used by Azure Gateway
    * `spoke-04` with 2 subnet used to connect spoke-04-vm machine
* An Azure Bastion resource that provides secure and seamless SSH connectivity to the jumpbox virtual machine directly in the Azure portal over SSL
* An Azure Firewall **standard** resource that provide a con-premiseic inspection.
* An Azure VPN Gateway resource that is used to send encrypted traffic between the hub virtual network to the on-premises simulated location.
* `spoke-04-vm`: a Windows Server virtual machine that simulates a server located in the spoke-01 landing zone

The site to site VPN connection shown in the architecture is not automatically deployed and configure: its configuration is covered by one of the playground scenarios.**est solution**
All machines have the same account parameters (as following):
* username: `nicola`
* password: `password.123`

## Playground's scenarios
Here there is a list of tested scenarios usable on this playground.

For each scenario you have:

* **prerequisites**: component to deploy required to implement the solution (only the hub, also one on-prem playground or both)
* **solution**: a step-by-step sequence to implement the solution
* **test solution**: a procedure to follow, to verify if the scenario is working as expected


| | scenario description | solution |
|---|---|---|
| 1 | Configure the environment to allow VM in any spoke to communicate with any VM in any other spoke | solution using [azure firewall](scenarios/ping-any-to-any-firewall.md)<br/> solution using [azure virtual gateway](scenarios/ping-any-to-any-gateway.md) 
| 2| Expose on a public IP, through the Firewall, `spoke-01-vm` and `spoke-02-vm `RDP port (3389) | solution using [azure firewall dnat](scenarios/dnat-01-02.md)
| 3 | Connect `on-prem-net` with `hub-lab-net` using a vNet-to-vNet Azure Gateway's Connection | solution [on-premise vnet-to-vnet](scenarios/vnet-to-vnet.md)<br/>solution [on-premise2 vnet-to-vnet-2](scenarios/vnet-to-vnet.md)
| 4 | Connect `on-prem-net` with `hub-lab-net` using a Site-to-Site (IPSec) Connection | solution with [gateway-ipsec](scenarios/ipsec.md)<br/> solution with [gateway-ipsec active-active](scenarios/ipsec-active-active.md)<br/> solution with [gateway-ipsec in dual redundancy](scenarios/ipsec-dual-redundancy.md)<br/> solution with [multiple VPN devices](scenarios/ipsec-multiple-vpn-device.md) [ * DRAFT * ]
| 5 | Configure a DNS on the cloud, so that all machines are reachable via FQDN |  solution with [azure-dns](scenarios/dns.md)
| 6 | Configure and use Azure Firewall logs for troubleshooting | configure  [log-analytics-on-firewall](scenarios/logs.md)
| 7 | Install a test web server on `spoke-03-vm` | install [web-server](scenarios/web.md) |
| 8 | Connect `on-prem-net` and `on-prem2-net` to `hub-lab-net` via S2S IPSEC and allow cross-on-premises communication | solution [cross-on-premise-routing](scenarios/cross-on-premise-routing.md) |
| 9 | Use Azure Firewall for traffic inspection between `on-prem-net` and `spoke-01` networks  (North/South Traffic Inspection) | solution [north-south-inspection](scenarios/solution-north-south-inspection.md)
| 10 | Use Network Watcher for logging and network troubleshooting | solution [network watcher](scenarios/network-watcher.md)
| 11 | Resolve from on-prem, names of all cloud machines | solution with [Azure Firewall](/scenarios/dns-on-prem.md) | 
| 12 | Secure a WEB workload with both Azure Firewall Premium and Azure Web Application Firewall | Solution with [Azure Firewall and  WAF](scenarios/publish-waf-fw.md)
| 13 | Configure a P2S VPN | Solution with [Certificate Authentication](scenarios/p2s-vpn-certificate.md)<br/>Solution with [CA and always-on](scenarios/p2s-vpn-certificate-always-on.md)
| 14 | Routing cross hubs with BGP | Solution using [Azure Virtual Network Gateway](scenarios/routing-with-bgp.md)
| 15 | Routing cross hubs without BGP | Solution with [Azure Firewall](scenarios/routing-without-bgp-fw.md) | 

Scenarios I will implement in the future:

* Resolve from on-prem, names of all cloud machines, and vice-versa
* configure firewall so that (1) traffic outbound from spoke01 goes hrough public IP1 (traffic outbound from spoke02 goes through public IP2 

Whould you like to see more scenarios? Open [an issue](https://github.com/nicolgit/hub-and-spoke-playground/issues])!
