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

![VNet and Subnets](screenshots/01-vnet-review-and-subnets.png)

## Network Security

The web subnet was protected with a Network Security Group allowing HTTPS traffic on TCP 443.

![Web NSG HTTPS Rule](screenshots/02-web-nsg-https-rule.png)

## VM Configuration

The Ubuntu VM was deployed into the web subnet and configured with cost-control and diagnostic settings.

![VM Cost and Diagnostics Settings](screenshots/03-vm-cost-and-diagnostics-settings.png)

![VM Overview](screenshots/04-vm-overview-sanitized.png)

## Nginx Web Server

Nginx was installed and validated as an active service.

![Nginx Running](screenshots/05-nginx-service-running.png)

Nginx was then configured to use HTTPS on TCP 443 with a self-signed certificate for lab testing.

![Nginx HTTPS Configuration](screenshots/06-nginx-https-config.png)

## Troubleshooting

During testing, the site initially returned a `403 Forbidden` response.

This confirmed that the network path was working because the browser successfully reached Nginx. The issue was isolated to the web-server content configuration rather than Azure networking.

![Nginx 403 Troubleshooting](screenshots/07-nginx-403-troubleshooting.png)

After creating the expected `index.html` file, HTTPS was validated locally from the VM.

![Local HTTPS Validation](screenshots/08-local-https-validation.png)

## Successful External Validation

The custom landing-zone webpage was successfully accessed externally over HTTPS through the Azure public IP.

![Public HTTPS Webpage](screenshots/09-public-https-webpage.png)

Traffic path:

Internet → Azure Public IP → NSG → Web Subnet → Ubuntu VM → Nginx → HTTPS Webpage

## Monitoring

Azure Monitor was used to review host-level CPU metrics.

![Azure Monitor CPU](screenshots/10-azure-monitor-cpu.png)

The Azure Activity Log was used to review management-plane operations such as Run Command and VM updates.

![Azure Activity Log](screenshots/11-azure-activity-log.png)

## RBAC

Created the Microsoft Entra security group:

`Cloud-Lab-Readers`

Assigned the Azure `Reader` role at the `rg-workload-lab` resource-group scope.

This demonstrates least-privilege access by allowing users to view workload resources without modifying them.

## Cost Management

Automatic VM shutdown was enabled.

The VM was manually stopped and deallocated after testing to avoid unnecessary compute charges.

## Result

Successfully deployed and validated a segmented Azure environment with:

- Resource groups
- VNet and subnet design
- Network Security Groups
- Linux VM deployment
- Nginx
- HTTPS/TLS
- RBAC
- Azure Monitor
- Activity Log analysis
- Troubleshooting
- Cost management
