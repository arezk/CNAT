# Cisco DNA Automation Tool
A Python script that will automate the administration of NFVIS. 
This program will enable the retrieval of platform information 
as well as automate the deployment and deletion of bridges, networks,
and virtual network functions to NFVIS. This tool works in an interactive
and non-interactive mode.

  * [Supported platforms](#supported-platforms)
  * [Dependencies](#dependencies)
  * [Usage](#usage)
  * [Operation](#operation)

## Supported platforms
NFVIS 3.9.1\
NFVIS 3.9.2\
NFVIS 3.10.1\
NFVIS 3.10.2

## Dependencies

PyNFVSDK\
tabulate\
getpass

## Usage

Interactive Mode

```
usage: CDAT.py
```

Non-Interactive Mode

```
usage: CDAT.py [method] [option] [ip-address] [file.xml]
```

## Operation

###Interactive Mode

```
CDAT.py 

#################################
####Cisco DNA Automation Tool####
################################# 

Select an option from the menu below. 

 Options: 

 '1' List NFVIS system information
 '2' List running VNF's from NFVIS
 '3' Delete Virtual Switch from NFVIS
 '4' Delete VNF from NFVIS
 '5' Deploy Virtual Switch to NFVIS from file
 '6' Deploy VNF to NFVIS from file
 '7' Deploy Service Chained VNFs to NFVIS
 '8' Reset demo environment
 'p' print options
 'q' quit the program

Option: 1
What is the IP address of the NFVIS system: 
10.10.10.10
Enter your username and password. 
Username: admin
Password: 
API Response Code: 200 :
https://10.10.10.10/api/operational/platform-detail
Platform Details: 

╒════════════════════╤════════════════════════════════════════════════╕
│ Manufacturer       │ Cisco Systems, Inc.                            │
├────────────────────┼────────────────────────────────────────────────┤
│ PID                │ ENCS5412/K9                                    │
├────────────────────┼────────────────────────────────────────────────┤
│ SN                 │ FGL211310QJ                                    │
├────────────────────┼────────────────────────────────────────────────┤
│ hardware-version   │ M3                                             │
├────────────────────┼────────────────────────────────────────────────┤
│ UUID               │ 70DB98C0-1B64-0000-FE49-E01BEBC65375           │
├────────────────────┼────────────────────────────────────────────────┤
│ Version            │ 3.10.2-FC3                                     │
├────────────────────┼────────────────────────────────────────────────┤
│ Compile_Time       │ Friday, January 25, 2019 [15:25:34 PST]        │
├────────────────────┼────────────────────────────────────────────────┤
│ CPU_Information    │ Intel(R) Xeon(R) CPU D-1557 @ 1.50GHz 12 cores │
├────────────────────┼────────────────────────────────────────────────┤
│ Memory_Information │ 65768572 kB                                    │
├────────────────────┼────────────────────────────────────────────────┤
│ Disk_Size          │ 64.0 GB                                        │
├────────────────────┼────────────────────────────────────────────────┤
│ CIMC_IP            │ NA                                             │
├────────────────────┼────────────────────────────────────────────────┤
│ Entity-Name        │ ENCS                                           │
├────────────────────┼────────────────────────────────────────────────┤
│ Entity-Desc        │ Enterprise Network Compute System              │
├────────────────────┼────────────────────────────────────────────────┤
│ BIOS-Version       │ ENCS54_2.6.071220181123                        │
├────────────────────┼────────────────────────────────────────────────┤
│ CIMC-Version       │ NA                                             │
╘════════════════════╧════════════════════════════════════════════════╛

 
```


###Non-Interactive Mode

Non-Interactive mode supports the following arguments:
```
g = platform-detail, bridges, networks, deployments, flavors, images

p = bridges, networks, deployments
	
d = bridges, networks, deployments

h = help menu
```
g = get\
p = post\
d = delete

To retrieve information about the system use the get method. 
To deploy a bridge, network, or VNF use the post method. 
To delete an existing bridge, network, or VNF use the delete method.

When using the post method a .xml template is required for configuration
of the bridge, network, or VNF. Examples of the templates can be found
in the XML folder.

If the creds.json file is not present in the working directory the user
will be prompted to enter device credentials. The program will then store
the ip address and credentials in the creds.json file for future use providing
a true non interactive experience. The creds.json file has been added to
.gitignore to ensure it is not tracked by git.

**Examples**

**GET:**
```
CDAT.py g networks 10.10.10.10
Username: admin
Password: 
API Response Code: 200 :

Request URI: https://10.10.10.10/api/config/networks?deep

JSON Reponse:

{'network:networks': {'network': [{'name': 'wan-net', 'bridge': 'wan-br'}, {'name': 'lan-net', 'bridge': 'lan-br'}]}}


############################################################

Hierarchical Config:

network:networks
        |
        -->network
                |
                -->wan-net
                |
                -->wan-br
                |
                -->lan-net
                |
                -->lan-br

```

```
python3 CDAT.py g deployments 10.10.10.10

API Response Code: 200 :

Request URI: https://10.10.10.10/api/config/vm_lifecycle/tenants/tenant/admin/deployments

JSON Reponse:

{'vmlc:deployments': {'deployment': [{'name': 'FIREWALL'}, {'name': 'ROUTER'}]}}


############################################################

Hierarchical Config:

vmlc:deployments
        |
        -->deployment
                |
                -->FIREWALL
                |
                -->ROUTER

```
**POST:**
```
CDAT.py p deployments 10.10.10.10 ASAv_ENCS.xml 
Username: admin
Password: 
API Response Code: 201 :

Request URI: https://10.10.10.10/api/config/vm_lifecycle/tenants/tenant/admin/deployments

JSON Reponse:

<Response [201]>


VNF deployment successful

```
**DELETE:**
```
CDAT.py d deployments 10.10.10.10 ASAv
Username: admin
Password: 

https://10.10.10.10/api/config/vm_lifecycle/tenants/tenant/admin/deployments/deployment/ASAv 
API Status Code: 204

VNF deletion successful


```
