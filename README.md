# Terraform Child Module Template

README

This terraform script deploys the following resources (The script deploys resources in the 'europe-west2' a.k.a London DC region):

1) Custom VPC with 2 subnets (1 subnet for Bastion Host and other administrative machines, the other subnet for hosting the k8s nodes).

2) Firewall rule to allow IAP (Identity Aware Proxy) for securely logging in to Bastion Host VM

3) Bastion Host for communicating with Kube API server

4) Security Hardened Private GKE Cluster with minimal permissions and privileges. This cluster will only be accessible via the Bastion host which is whitelisted to use it. (

Notes:

-> This script does not include the creation of service accounts nor roles
-> This script does not include the creation of Key Rings and Encryption Keys
-> This script does not include the creation and configuration of NAT Gateway, it is recommended to setup CloudNAT and configure it for the VPC where your cluster is hosted, else the private K8s nodes will not be able to access the internet.
-> Any access from pods/jobs to Google Cloud Services that are not part of the K8s nodes service account permissions need to be granted granular permissions via Workload Identity

Pre-requisites:

1) service account for terraform with the following roles:

-> Editor

2) service account for bastion host with the following roles:

-> Monitoring Viewer
-> Monitoring Metric Writer
-> Logs Writer
-> Storage Object Viewer
-> Kubernetes Engine Developer

3) service account for k8s nodes with the following roles:

-> Monitoring Viewer
-> Monitoring Metric Writer
-> Logs Writer
-> Storage Object Viewer

4) encryption key in cloud KMS for encrypting ETCD

Edits required before terraform apply:

1) root/variables.tf:

-> Line 4: Add path to credentials file
-> Line 16: Add project ID
-> Line 122: Add service acount ID of k8s-nodes
-> Line 123 (optional): Change node pool machine type
-> line 130: Add encryption key name for ETCD

2) modules/kubernetes/main.tf

-> Line 31 (optional): Change GKE master version
-> Line 57: Add key ID where encryption key for ETCD is contained (the full resource ID must be mentioned for this, not just the key name)
-> Set maintenance window and maintenance exclusions based on your time zone (I have intentionally left this out consdiering that different organizations have different peak traffic hours and varying time zones)

3) modules/Bastion_Host/main.tf

-> Line 30: Add service account ID of Bastion Host VM




<!-- BEGIN_TF_DOCS -->
[![FINOS - Incubating](https://cdn.jsdelivr.net/gh/finos/contrib-toolbox@master/images/badge-incubating.svg)](https://finosfoundation.atlassian.net/wiki/display/FINOS/Incubating)
![website build](https://github.com/finos/cfi-terraform-template-child-module/workflows/Docusaurus-website-build/badge.svg)

# terraform-provider-function

This terraform module produces blah

## Usage example
```hcl
module "iam" {
  source  = "terraform-aws-modules/iam/aws"
  version = "5.3.0"
}
```

### Providers

No providers.

### Requirements

No requirements.

### Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_bastion_host"></a> [bastion\_host](#input\_bastion\_host) | The Bastion host config for production | <pre>object({<br>    internal_ip_address = string<br>    vm_name             = string<br>    machine_type        = string<br>    zone                = string<br>    machine_image       = string<br>    tags                = list(string)<br>    bastion_subnet_name = string<br>    region              = string<br>  })</pre> | <pre>{<br>  "bastion_subnet_name": "admin-subnet",<br>  "internal_ip_address": "10.0.1.2",<br>  "machine_image": "ubuntu-1604-lts",<br>  "machine_type": "n1-standard-1",<br>  "region": "europe-west2",<br>  "tags": [<br>    "allow-iap"<br>  ],<br>  "vm_name": "prod-bastion-host",<br>  "zone": "europe-west2-a"<br>}</pre> | no |
| <a name="input_bastion_subnet_name"></a> [bastion\_subnet\_name](#input\_bastion\_subnet\_name) | name of the subnet to deploy bastion host on | `string` | `"admin-subnet"` | no |
| <a name="input_cluster_name"></a> [cluster\_name](#input\_cluster\_name) | Cluster name for the GCP Cluster. | `string` | `"gke-cluster"` | no |
| <a name="input_cred_url"></a> [cred\_url](#input\_cred\_url) | Your service account full URL | `string` | `"<path to json key credentials of service account that tf uses>"` | no |
| <a name="input_encryption_key_name"></a> [encryption\_key\_name](#input\_encryption\_key\_name) | Name of the encryption key for ETCD | `string` | `"<encryption-key-id>"` | no |
| <a name="input_gke-cluster"></a> [gke-cluster](#input\_gke-cluster) | The GKE app cluster for production | <pre>object({<br>    region                  = string<br>    cluster_name            = string<br>    master_cidr             = string<br>    cluster_ipv4_cidr_block = string<br>    service_account_name    = string<br>    machine_type            = string<br>  })</pre> | <pre>{<br>  "cluster_ipv4_cidr_block": "10.1.0.0/16",<br>  "cluster_name": "gke-cluster",<br>  "machine_type": "e2-standard-4",<br>  "master_cidr": "172.168.10.0/28",<br>  "region": "europe-west2",<br>  "service_account_name": "<k8s-nodes-service-account-id>"<br>}</pre> | no |
| <a name="input_gke-vpc"></a> [gke-vpc](#input\_gke-vpc) | The name of the production VPC | <pre>object({<br>    name = string<br>    subnets = list(object({<br>      name          = string<br>      description   = string<br>      ip_cidr_range = string<br>      region        = string<br>    }))<br>  })</pre> | <pre>{<br>  "name": "gke-vpc",<br>  "subnets": [<br>    {<br>      "description": "Subnet for bastion host and other administrative VMs",<br>      "ip_cidr_range": "10.0.1.0/24",<br>      "name": "admin-subnet",<br>      "region": "europe-west2"<br>    },<br>    {<br>      "description": "Subnet for GKE nodes ",<br>      "ip_cidr_range": "10.0.2.0/24",<br>      "name": "k8s-nodes-subnet",<br>      "region": "europe-west2"<br>    }<br>  ]<br>}</pre> | no |
| <a name="input_master_cidr"></a> [master\_cidr](#input\_master\_cidr) | CIDR block address of GKE master. | `string` | `"172.16.0.0/28"` | no |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | Your project id in GCP | `string` | `"<project-id>"` | no |
| <a name="input_region"></a> [region](#input\_region) | The region of the project resources in GCP | `string` | `"europe-west2"` | no |
| <a name="input_service_account_name"></a> [service\_account\_name](#input\_service\_account\_name) | The service account name | `string` | `"<ID of K8s nodes service account>"` | no |
| <a name="input_zone"></a> [zone](#input\_zone) | The zone of the project resources in GCP | `string` | `"a"` | no |

### Outputs

No outputs.

### Resources

No resources.

## Installation

OS X & Linux:

```sh
npm install my-crazy-module --save
```

Windows:

```sh
edit autoexec.bat
```
## Development setup

Describe how to install all development dependencies and how to run an automated test-suite of some kind. Potentially do this for multiple platforms.

```sh
make install
npm test
```
## Roadmap

List the roadmap steps; alternatively link the Confluence Wiki page where the project roadmap is published.

1. Item 1
2. Item 2
3. ....

## Contributing

1. Fork it (<https://github.com/finos/cfi-terraform-template-child-module/fork>)
2. Create your feature branch (`git checkout -b feature/fooBar`)
3. Read our [contribution guidelines](.github/CONTRIBUTING.md) and [Community Code of Conduct](https://www.finos.org/code-of-conduct)
4. Commit your changes (`git commit -am 'Add some fooBar'`)
5. Push to the branch (`git push origin feature/fooBar`)
6. Create a new Pull Request

\_NOTE:\_ Commits and pull requests to FINOS repositories will only be accepted from those contributors with an active, executed Individual Contributor License Agreement (ICLA) with FINOS OR who are covered under an existing and active Corporate Contribution License Agreement (CCLA) executed with FINOS. Commits from individuals not covered under an ICLA or CCLA will be flagged and blocked by the FINOS Clabot tool (or [EasyCLA](https://community.finos.org/docs/governance/Software-Projects/easycla)). Please note that some CCLAs require individuals/employees to be explicitly named on the CCLA.

*Need an ICLA? Unsure if you are covered under an existing CCLA? Email [help@finos.org](mailto:help@finos.org)*

## License

Copyright 2022 FINOS

Distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

SPDX-License-Identifier: [Apache-2.0](https://spdx.org/licenses/Apache-2.0)
<!-- END_TF_DOCS -->


