
```Powershell
$NAME="album"
$LOCATION="canadacentral"
$API_NAME="api"
$FRONTEND_NAME="ui"
$ACR_NAME="albumdan"
$VERSION="1.0.2"

az group create `
  --name $NAME `
  --location $LOCATION

az acr create `
  --resource-group $NAME `
  --name $ACR_NAME `
  --sku Basic `
  --admin-enabled true

az acr build --registry $ACR_NAME --image $API_NAME:$VERSION .

az containerapp env create `
  --name $NAME `
  --resource-group $NAME `
  --location $LOCATION


az containerapp create `
  --name $API_NAME `
  --resource-group $NAME `
  --environment $NAME `
  --image 'albumdan.azurecr.io/api:latest' `
  --target-port 3500 `
  --ingress 'external' `
  --registry-server 'albumdan.azurecr.io' `
  --query properties.configuration.ingress.fqdn

az containerapp up `
  --name $API_NAME `
  --image 'albumdan.azurecr.io/api:1.0.2'

```