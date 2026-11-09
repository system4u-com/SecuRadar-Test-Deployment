# SecuRadar Azure Deployment

Version: v0.9.3

Choose the deployment scope that matches the customer subscription:

## Prerequisities

Before deploying, check, that `Microsoft.OperationsManagement` is registered provider on the subscription where you are deploying.

In Azure Portal, go to Subscriptions → select the target subscription → Resource providers → Microsoft.OperationsManagement → Register. The registering identity needs /register/action permission; subscription Contributor and Owner include it.

## Create a resource group

Creates the SecuRadar resource group, workspace, Sentinel configuration, and Lighthouse delegation at subscription scope. Choose the deployment region in the Azure portal.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsystem4u-com%2FSecuRadar-Test-Deployment%2Fv0.9.3%2Fazuredeploy.subscription.json)

## Use an existing resource group

Deploys the workspace and Sentinel configuration into an existing resource group. The resource group's location is used. Lighthouse assignment is scoped to that resource group.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsystem4u-com%2FSecuRadar-Test-Deployment%2Fv0.9.3%2Fazuredeploy.resourceGroup.json)

## Parameters

| Parameter | Values and default | What it does |
| --- | --- | --- |
| **Customer Shortcut** (`customerShortcut`) | Required; 1-44 characters | Identifies the customer in resource names. The value is converted to lowercase. The workspace is named `law-{releaseChannel}-securadar-{customerShortcut}`. The subscription template also creates the resource group `rg-{releaseChannel}-securadar-{customerShortcut}`. Use a distinct shortcut for each customer environment. |
| **Release Channel** (`releaseChannel`) | `prod` (default) or `test` | Selects the naming channel and matching provider Lighthouse identities. Use `prod` for customer production deployments. Use `test` only for development or test deployments. |
| **Deployment Type** (`deploymentType`) | Required: `securityAssessment` or `fullDeployment` | Selects the Lighthouse authorization set. `securityAssessment` uses the assessment set; `fullDeployment` adds the full-deployment authorizations. Both choices deploy the workspace and configured Sentinel resources. |
| **SoC Active** (`socActive`) | `false` (default) | Set to `true` when the customer has the managed SOC service. This adds the channel-specific SOC Lighthouse roles. Leave `false` when SOC roles are not needed. |
| **Redeploy** (`skipEntityAnalytics`) | `false` (default) | Normally leave this `false`, especially on the first deployment. Set `true` only when redeploying an existing workspace and its Entity Analytics setting fails to update with an ETag error. This skips changing that setting while allowing the rest of the deployment to continue; it does not enable Entity Analytics on a new workspace. |
| **L3 Only** (`l3Only`) | `false` (default) | Set `true` to limit direct Lighthouse authorizations to the shared L3 set, regardless of deployment type or SOC selection. The configured eligible/JIT authorizations are still included. Leave `false` for the normal authorization composition. |

For the subscription-scope template, choose the Azure region in the portal's **Region** selector; there is no separate location parameter. The existing-resource-group template uses the selected resource group's location.

The deploying identity needs permission to create the resources at the selected scope and to create the required Lighthouse resources. Subscription Owner is recommended for initial integration testing. For an existing-resource-group deployment, resource-group Owner alone is not sufficient because the template also creates a subscription-level Lighthouse registration definition.
