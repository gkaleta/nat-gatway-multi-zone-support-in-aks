# Quick n' dirty way to run a NAT Gateway - one per AZ/Nodepool in AKS 
## How to run NAT Gateway with AZ's in AKS


### Create first NAT gateway and subnet

```bash
az network public-ip create --resource-group gustav-test --name test2 --location westus --sku standard
az network nat gateway create --resource-group gustav-test --name test2 --location westus --public-ip-addresses test2 
az network vnet create --resource-group gustav-test --name test2 --location westus --address-prefixes 172.16.0.0/20 SUBNET_ID1=\((az network vnet subnet create --resource-group gustav-test --vnet-name test2 --name subnet1 --address-prefixes 172.16.0.0/22 --nat-gateway test2 --query id --output tsv)<br></p><p>&nbsp;</p><p>#create cluster with first nodepool with the subnet</p><p>IDENTITY_ID=\)(az identity create --resource-group gustav-test --name test2 --location westus --query id -output tsv)
```
### Cluster creations
```bash
az aks create --resource-group gustav-test --name test2 --location westus --network-plugin azure  --vnet-subnet-id $SUBNET_ID1 --outbound-type userAssignedNATGateway --enable-managed-identity --assign-identity \(IDENTITY_ID</p><p>&nbsp;</p><p>#create a second subnet with another nat gateway</p><p>az network public-ip create --resource-group gustav-test --name test2-2 --location westus --sku standard<br>az network nat gateway create --resource-group gustav-test --name test2-2 --location westus --public-ip-addresses test2-2<br>SUBNET_ID2=\)(az network vnet subnet create --resource-group gustav-test --vnet-name test2 --name subnet2 --address-prefixes 172.16.4.0/22 --nat-gateway test2-2 --query id --output tsv)
```
 
### add scecond nodepool with the second subnet
```bash
az aks nodepool add --resource-group gustav-test --cluster-name test2 --name np2 --node-count 3 --vnet-subnet-id $SUBNET_ID2
```
