<<<<<<< HEAD
# Automated configuration of a cluster of VM series managed by Panorama for Multi Pod PBR insertion

### Pre-Requisite :
- Panorama 8.1.X and 2 VM series 8.1.X connected to panorama but not attached to any template nor device group yet.

- Each of your VM series must have a minimum of 3 vNics for that setup and vNic must be defined as describe bellow :
        - eth0 (vNic1) connected to the management network
        - eth1 (vNic2) connected to a dedicated port group of your DVS and it will be used for HA2
        - eth2 (vNic3) connected to a dedicated port group of your DVS and it will be used for HA3
        - eth3 (vNic4) connected to a dedicated port group of your DVS and it will be used for PBR connection to the fabric.

- A PBR policy MUST be configured in parallel in the Fabric for your pod with filters on a Service Graph and that Service Graph must be applied to a contract to start redirecting some traffic to our cluster of VM Series. 

- You must have a Multi Pod (2 pod) with "Enable Pod Aware redirection" option checked and a functionnal IP SLA option (icmp) for the heath check in your L4-L7 Policy-Based Redirect configuration for the setup to work as expected


### This skillet will automate these tasks for you on Panorama :
- create a Template Stack and a Template dedicated for each of your 2 VM Series all Network and Device policies configured with webpage variables.
- create a Device Group dedicated for that specific cluster of VM series configured for a Multi Pod PBR insersion.
- move and attach your 2 VM series to these respective Template Stack and Device Group adn trigger a commit to have a fully functionnal setup.
 

Notice that 

## Variables
- STACK_DEVICE_A (Template Stack name Device A)
- STACK_DEVICE_B (Template Stack name Device B)
- DEVICEGROUP (Device Group name for your VM Series for ACI)
- HA2_DEVICE_A (Device A IP address for HA2)
- HA2_DEVICE_B (Device B IP address for HA2)
- HA3_DEVICE_A (Device A IP address for HA3)
- HA3_DEVICE_B (Device B IP address for HA3)
- MGT_DEVICE_A (MGT IP address of Device A)
- MGT_DEVICE_B (MGT IP address of Device B)
- DNS (DNS Server IP address)
- NTP (NTP Server IP address)
- ZONE (Zone creation for PBR interconnection)
- TARGET_IP_PBR (Target IP of the fabric for PBR without Netmask)
- LOCAL_IP_PBR (Local IP of your VM interface connected to the the fabric for PBR with Netmask)
- TAGCOLOR (Color of the TAG for your Zone)



## Caution  
- That skillet will not configure ACI Fabric to redirect traffic. That step must be done in parallel.
- Promiscious mode is not mandatory vNIC4 port group to have a functionnal cluster A/A
- That skillet shoud work with physical devices instead of VM series but it has not been tested.

## Support Policy

Have been tested with a single instance of panorama 8.1.X but should work as well with 9.0.X release.
Works with ACI 3.2 and higher releases.
=======
# Automated configuration of NSX-V for Panorama

### Pre-Requisite :
- Panorama 8.1.X with one of the NSX plugin 2.0.X installed


### This skillet will automate these tasks for you on Panorama :
- create a Template Stack and a Template dedicated for NSX-V with DNS and NTP server configured with webpage variables.
- create a Device Group dedicated for NSX-V with the authcode configured with webpage variables.
- create a Service Definition and a Service Manager profile with a single Service Profile "Tenant" attached to the service Definition. 
- create a intrazone pre-rulebase security for your tenant with Security Profiles, Log Forwarding Profile and a dedicated TAG for that tenant. 

Notice that if you want a second ("Tenant"), you just need to launch the Skillet a second time and just modify the "Tenant" name variable at the end of the webpage with your second "Tenant" name 

## Variables
- STACK (Template Stack name for NSX)
- DEVICEGROUP (Device Group name for NSX)
- AUTHCODE (AuthCode VM series for NSX)
- URLOVA (URL where ovf/ova package is hosted)
- NSXLOGIN (NSX Manager login)
- NSXMANAGER (NSX Manager URL)
- NSXPASSWORD (NSX Manager password)
- DNS (DNS Server IP address)
- NTP (NTP Server IP address)
- TENANT (Tenant name for Zone creation)
- TAGCOLOR (Color of the TAG for your Tenant)

## Caution  
- That skillet will not deploy our VM agents on NSX manager nor configure it for deployment, it's panorama configuration only. 
- That skillet release does not include creation of automated Security Groups and automated Steering Rules policies. That part will remain a manual step to fit your needs after you properly applied your skillet to panorama. 

## Support Policy

Have been tested with a single instance of panorama 8.1.X and plugin 2.0.X but should work as well with 9.0.X release.
Works with all the release of NSX-V that we support officially
>>>>>>> e51db06bdcbb1043528b147ddd8ee078343d727e

The code and templates in the repo are released under an as-is, best effort,
support policy. These scripts should be seen as community supported and
Palo Alto Networks will contribute our expertise as and when possible.
We do not provide technical support or help in using or troubleshooting the
components of the project through our normal support options such as
Palo Alto Networks support teams, or ASC (Authorized Support Centers)
partners and backline support options. The underlying product used
(the VM-Series firewall) by the scripts or templates are still supported,
but the support is only for the product functionality and not for help in
deploying or using the template or script itself. Unless explicitly tagged,
all projects or work posted in our GitHub repository
(at https://github.com/PaloAltoNetworks) or sites other than our official
Downloads page on https://support.paloaltonetworks.com are provided under
the best effort policy.