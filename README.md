# SecuRadar Azure Deployment

Version: v0.9.1

Choose the deployment scope that matches the customer subscription:

## Create a resource group

Creates the SecuRadar resource group, workspace, Sentinel configuration, and Lighthouse delegation at subscription scope. Choose the deployment region in the Azure portal.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsystem4u-com%2FSecuRadar-Test-Deployment%2Fv0.9.1%2Fazuredeploy.subscription.json)

## Use an existing resource group

Deploys the workspace and Sentinel configuration into an existing resource group. The resource group's location is used. Lighthouse assignment is scoped to that resource group.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsystem4u-com%2FSecuRadar-Test-Deployment%2Fv0.9.1%2Fazuredeploy.resourceGroup.json)

## Parameters

Provide the customer shortcut, release channel (`prod` or `test`), deployment type (`securityAssessment` or `fullDeployment`), and whether SOC is active. Microsoft 365 and Defender XDR connectors are enabled by default. Entra ID diagnostic log export is opt-in and requires additional tenant-level permissions.

The deploying identity needs permission to create the resources at the selected scope and to create the required Lighthouse resources. Subscription Owner is recommended for initial integration testing. For an existing-resource-group deployment, resource-group Owner alone is not sufficient because the template also creates a subscription-level Lighthouse registration definition.

Review licensing, permissions, and connector prerequisites before deployment. See [Microsoft's Deploy to Azure button documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-to-azure-button) for details about the Azure portal flow.
