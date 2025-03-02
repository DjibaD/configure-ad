<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

1. Plan Your Architecture
VM Deployment: Create virtual machines in Azure that will run your domain controllers (DCs). These VMs should be of the appropriate size and configured with the necessary resources.
Networking: Set up a Virtual Network (VNet) in Azure to ensure your domain controllers are on the same network for communication. You may also need to configure a VPN or ExpressRoute if you have an on-premises AD and wish to extend your AD across hybrid environments.
Multiple Domain Controllers: To ensure high availability, you should deploy multiple domain controllers across different Availability Zones or regions.
2. Deploy Virtual Machines in Azure
In the Azure portal, create Windows Server VMs that will act as your Domain Controllers.
Ensure that the VM sizes meet the requirements for your AD deployment, depending on the expected load.
Set up Static IP Addresses for each domain controller to ensure stability.
3. Set Up the Domain Controllers
Install Active Directory Domain Services (AD DS) on the Windows Server VMs.
Open Server Manager on the VM.
Add the Active Directory Domain Services role.
After installation, promote the server to a domain controller.
Configure the domain controllers as per your on-premises AD setup, including creating necessary DNS records and ensuring Active Directory replication.
You may replicate an existing on-premises AD or create a new AD forest/domain as required.
4. Establish Network Connectivity (Hybrid)
If you are extending an on-premises AD into Azure, you should configure hybrid connectivity between on-premises and Azure:
Use VPN Gateway or ExpressRoute to connect the on-premises network to the Azure Virtual Network.
Ensure DNS Resolution is properly configured between on-premises and Azure AD environments.
If extending your on-premises domain, ensure that the domain controllers in Azure can contact your on-premises domain controllers for replication.
If you're creating a new forest/domain in Azure, ensure proper network routing for AD communication.
5. Configure Active Directory Replication (if applicable)
For a hybrid environment, ensure that AD replication between on-premises and Azure AD DCs is functioning properly. You can configure Site-to-Site VPN or Azure AD Connect (if integrating with Azure AD) for seamless synchronization.
Replicate necessary Active Directory data (users, groups, GPOs, etc.) between on-premises AD and the AD controllers running in Azure.
6. Configure DNS and Global Catalogs
Active Directory heavily relies on DNS. In Azure, configure DNS settings on your domain controllers to ensure correct name resolution.
Ensure that you’ve set up the Global Catalog (GC) role on one or more domain controllers in Azure, which is essential for certain Active Directory queries across domains.
7. Ensure High Availability and Disaster Recovery
Ensure your domain controllers are distributed across multiple availability zones or regions to protect against failures.
Implement backup and disaster recovery strategies for your domain controllers in Azure.
8. Test the Environment
After setting everything up, test the deployment to make sure replication works, and domain controllers can authenticate and authorize users as expected.
Verify DNS resolution, GPO applications, and AD replication.
9. Maintain and Monitor
Implement monitoring using Azure Monitor, Log Analytics, and Azure Security Center to monitor the health of your domain controllers.
Regularly update your VMs and domain controllers with the latest patches and security updates.
10. Integrating with Azure AD (Optional)
If you want to integrate your on-premises AD with Azure AD, you can use Azure AD Connect to synchronize identities and provide a hybrid identity solution.
Azure AD Connect can sync users and groups from your on-prem AD to Azure AD, allowing you to have a unified login experience.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/CwhZ8Wj.jpeg"/>
</p>
<p>
Setup Domain Controller in Azure
—
Create a Resource Group
Create a Virtual Network and Subnet
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Username: labuser
Password: Cyberlab123!
After VM is created, set Domain Controller’s NIC Private IP address to be static
Log into the VM and disable the Windows Firewall (for testing connectivity)

</p>
<br />

<p>
<img src="https://i.imgur.com/g96wvdk.jpeg"/>
</p>
<p>
Setup Client-1 in Azure
—
Create the Client VM (Windows 10) named “Client-1”
Username: labuser
Password: Cyberlab123!
Attach it to the same region and Virtual Network as DC-1
After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address

</p>
<br />

<p>
<img src="https://i.imgur.com/u4tRLc7.jpeg"/>
</p>
<p>
From the Azure Portal, restart Client-1
Login to Client-1
Attempt to ping DC-1’s private IP address
Ensure the ping succeeded

</p>
<br />


<p>
<img src="https://i.imgur.com/RXwTT4l.jpeg"/>
</p>
<p>
From Client-1, open PowerShell and run ipconfig /all
The output for the DNS settings should show DC-1’s private IP Address

</p>
<br />
