# Azure Mini Landing Zone Lab

## Project Overview

Built a small Azure landing-zone-style environment to practice core Azure administration, networking, security, RBAC, monitoring, workload deployment, troubleshooting, and cost management.

## Architecture

Azure Subscription

├── rg-network-lab
│   ├── vnet-cloudlab - 10.10.0.0/16
│   │   ├── snet-web - 10.10.1.0/24
│   │   ├── snet-app - 10.10.2.0/24
│   │   └── snet-management - 10.10.3.0/24
│   ├── nsg-web-lab
│   ├── nsg-app-lab
│   └── nsg-management-lab
│
├── rg-workload-lab
│   └── vm-web-01
│
└── rg-monitoring-lab

## Skills Practiced

- Azure Resource Groups
- Azure Virtual Networks
- Subnetting and CIDR
- Network Security Groups
- NSG rule priorities
- Linux virtual machines
- Public and private IP addressing
- Nginx
- HTTPS/TLS
- Self-signed certificates
- Azure RBAC
- Microsoft Entra security groups
- Azure Monitor
- Azure Activity Log
- Cost management and VM deallocation

## Network Design

The virtual network uses:

`10.10.0.0/16`

Subnets:

- `snet-web` - `10.10.1.0/24`
- `snet-app` - `10.10.2.0/24`
- `snet-management` - `10.10.3.0/24`

The web subnet contains the internet-facing workload.

The application subnet allows application traffic from the web subnet over TCP 8080 while denying other unnecessary virtual network traffic.

The management subnet is reserved for administrative resources.

## Security Design

### Web Tier

The web NSG allows:

- HTTPS TCP 443

Public SSH access was intentionally not enabled.

### Application Tier

The application NSG allows:

- `10.10.1.0/24` → application subnet on TCP 8080

Other VNet traffic is denied unless explicitly allowed.

## Workload Deployment

Deployed an Ubuntu Server VM:

`vm-web-01`

Installed Nginx and configured it as an HTTPS web server.

A temporary self-signed TLS certificate was created for the lab.

The final traffic path was:

Internet → Azure Public IP → NSG → Web Subnet → Ubuntu VM → Nginx → HTTPS Webpage

## RBAC

Created:

`Cloud-Lab-Readers`

Assigned the Azure `Reader` role at the `rg-workload-lab` resource-group scope.

This demonstrates least-privilege access by allowing users to view workload resources without modifying them.

## Monitoring

Used Azure Monitor to review:

- Percentage CPU
- VM health
- Azure Activity Log
- Run Command operations

This demonstrated the difference between performance metrics and Azure management-plane events.

## Troubleshooting

Several issues were encountered and resolved during the lab:

- Azure VM family quota limitations
- VM architecture and size compatibility
- Incorrect NSG-to-subnet associations
- Nginx 403 Forbidden response
- Self-signed TLS certificate warning

The Nginx 403 response showed that network connectivity was working because the request successfully reached the web server.

The issue was resolved by creating the expected `index.html` file.

## Cost Management

Automatic shutdown was enabled.

The virtual machine was manually stopped and deallocated after testing to avoid unnecessary compute charges.

## Result

Successfully deployed and validated a segmented Azure environment with networking, NSGs, RBAC, monitoring, Linux administration, Nginx, HTTPS, and cost controls.
