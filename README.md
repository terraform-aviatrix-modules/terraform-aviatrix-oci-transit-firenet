# Terraform Aviatrix OCI Transit FireNet

### Description
This module deploys a Aviatrix Transit FireNet VCN and an Aviatrix transit gateways and NGFW. Defining the Aviatrix Terraform provider is assumed upstream and is not part of this module.

**_Note_** Ensure that you are subscribed to the Firewall offering in OCI Marketplace

**_OCI Regions containing multiple Availabilty Domains_** ```us-ashburn-1, us-phoenix-1, uk-london-1, eu-frankfurt-1```

### Compatibility
Module version | Terraform version | Controller version | Terraform provider version
:--- | :--- | :--- | :---
v5.0.1 | 0.13 - 1.x | >=6.6.5404 | >=2.21.1
v5.0.0 | 0.13 - 1.x | >=6.6 | >=2.21.0
v4.0.4 | 0.13, 0.14, 0.15 | >=6.4 | 2.19.5
v4.0.3 | 0.13, 0.14, 0.15 | >=6.4 | 2.19.4


### Diagram
<img src="https://github.com/terraform-aviatrix-modules/terraform-aviatrix-oci-transit-firenet/blob/master/img/oci-transit-firenet-diagram.png?raw=true"  height="250">

### Usage Example

```
# OCI Transit FireNet Module
module "oci_transit_firenet_1" {
  source      = "terraform-aviatrix-modules/oci-transit-firenet/aviatrix"
  version     = "5.0.0"
  cidr        = "10.10.0.0/16"
  region      = "us-ashburn-1"
  account     = "My-OCI-Access-Account" 
}
```

The following variables are required:

key | value
--- | ---
region | OCI region to deploy the transit firenet VCN in
account | The OCI account name on the Aviatrix controller, under which the controller will deploy this VCN
cidr | The IP CIDR wo be used to create the VCN

The following variables are optional:

key | default | value
--- | --- | ---
bgp_ecmp  | false | Enable Equal Cost Multi Path (ECMP) routing for the next hop
bgp_manual_spoke_advertise_cidrs | null | Intended CIDR list to advertise via BGP. Example: "10.2.0.0/16,10.4.0.0/16" 
bgp_polling_time  | 50 | BGP route polling time. Unit is in seconds
connected_transit | true | Set to false to disable connected_transit
enable_advertise_transit_cidr  | false | Switch to enable/disable advertise transit 
enable_multi_tier_transit | false | Switch to enable multi tier transit VPC network CIDR for a VGW connection
enable_segmentation | false | Switch to true to enable transit segmentation
firewall_image | Palo Alto Networks VM-Series Next-Generation Firewall (BYOL) | The firewall image to be used to deploy the NGFW's
firewall_image_version | 10.0.4 | The firewall image version specific to the NGFW vendor image
fw_instance_size | VM.Standard2.4 | Size of the firewall instances
ha_gw | true | Set to false te deploy a single transit GW.
insane_mode | false | Set to true to enable insane mode encryption
inspection_enabled | true | Set to false to disable inspection on the firewall instances
instance_size | VM.Standard2.4 | Size of the transit gateway instances
keep_alive_via_lan_interface_enabled | false | Enable Keep Alive via Firewall LAN Interface
learned_cidr_approval | false | Switch to true to enable learned CIDR approval
name | null | When this string is set, user defined name is applied to all infrastructure supporting n+1 sets within a same region or other customization
prefix | true | Boolean to enable prefix name with avx-
single_az_ha | true | Set to false if Controller managed Gateway HA is desired
single_ip_snat | false | Enable single_ip mode Source NAT for this container
suffix | true | Boolean to enable suffix name with -transit

Outputs
This module will return the following objects:

key | description
--- | ---
[vcn](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_vpc) | The created VCN as an object with all of it's attributes. This was created using the aviatrix_vpc resource.
[transit_gateway](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_transit_gateway) | The created Aviatrix transit gateway as an object with all of it's attributes.
[aviatrix_firenet](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firenet) | The created Aviatrix firenet object with all of it's attributes.
[aviatrix_firewall_instance](https://registry.terraform.io/providers/AviatrixSystems/aviatrix/latest/docs/resources/aviatrix_firewall_instance) | A list of the created firewall instances and their attributes.
