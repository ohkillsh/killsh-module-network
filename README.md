# terraform-azurerm-network

## Create a basic network in Azure

This Terraform module deploys a Virtual Network in Azure with a subnet or a set of subnets passed in as input parameters.

The module does not create nor expose a security group. You could use https://github.com/Azure/terraform-azurerm-vnet to assign network security group to the subnets.

## Usage in Terraform 0.13

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "my-resources"
  location = "West Europe"
}

module "network" {
  source              = "git@github.com:ohkillsh/killsh-module-network.git"
  resource_group_name = azurerm_resource_group.example.name
  address_spaces      = ["10.0.0.0/16", "10.2.0.0/16"]
  subnet_prefixes     = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  subnet_names        = ["subnet1", "subnet2", "subnet3"]

  subnet_service_endpoints = {
    "subnet1" : ["Microsoft.Sql"], 
    "subnet2" : ["Microsoft.Sql"],
    "subnet3" : ["Microsoft.Sql"]
  }

  tags = {
    environment = "dev"
    costcenter  = "it"
  }

  depends_on = [azurerm_resource_group.example]
}
```

## Test

### Configurations

### Native (Mac/Linux)

#### Prerequisites

## Authors

Originally created by [Gustavo Kuno](https://github.com/Gustavmk)

## License

[MIT](LICENSE)
