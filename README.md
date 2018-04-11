# Stage and Deploy an Application using PowerShell locally

This demo will describe how to take an application, stage it from GitHub and then deploy it live using PowerShell ISE locally.

## Getting Started

Ensure you can access Windows PowerShell ISE on your windows machine, and you have valid credentials to log into your Azure portal.

[Login Azure Portal](https://portal.azure.com)

## Managing your Application

### Prerequisites

Login to your Azure account from PowerShell ISE:

    Login-AzureRmAccount

### Set Parameters

    $gitrepo="https://github.com/JL337/app-service-web-dotnet-get-started"
    $webappname="webappAzure110418"
    $location="West Europe"
    $rg="app01ResourceGroup"

Check the contents of a parameter:

    echo $<parameter>

### Creating and Allocating Resources

Create a new resource group:

    New-AzureRmResourceGroup -Name $rg -Location $location

Create an App Service plan in Free tier:

    New-AzureRmAppServicePlan -Name $webappname -Location $location `
    -ResourceGroupName $rg -Tier Free

Create a web app:

    New-AzureRmWebApp -Name $webappname -Location $location `
    -AppServicePlan $webappname -ResourceGroupName $rg

Upgrade App Service plan to Standard tier (minimum required by deployment slots):

    Set-AzureRmAppServicePlan -Name $webappname -ResourceGroupName $rg `
    -Tier Standard

### Deploying to Staging Environment

Create a deployment slot with the name "staging":

    New-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rg `
    -Slot staging

Configure GitHub deployment to the staging slot from your GitHub repo and deploy once:

    $PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    }
    Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rg `
    -ResourceType Microsoft.Web/sites/slots/sourcecontrols `
    -ResourceName $webappname/staging/web -ApiVersion 2015-08-01 -Force

If you encounter the following issue:

Q: Get exception "Cannot find SourceControlToken with name GitHub".

To setup continuous deployment, An oauth token is required to call GitHub/Bitbucket apis. This token is given to Azure during OAuth consent flow on portal. If you run into this issue, try logon to portal (portal.azure.com) and perform setup continuous deployment once (to any site). The oauth token will be stored for the logon Azure user. Each Azure user will need to go through OAuth consent flow once. Then: [Set up Continous Deployment via GitHub to Auzure](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-build-continuous-deployment)

### Deploy to Live Production

Swap the verified/warmed up staging slot into production:

    Switch-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rg `
    -SourceSlotName staging -DestinationSlotName production


## Removing Resource Group

Remove the resource group, web app, and all related resources:

    Remove-AzureRmResourceGroup -Name $rg -Force
