# Stage and Deploy an Application using Azure CLI and PowerShell

This demo will describe how to take an application, stage it from GitHub and then deploy it live using the Azure CLI locally using PowerShell

## Getting Started

Ensure you can access Windows PowerShell ISE on your windows machine, and you have valid credentials to log into your Azure portal.

[Login Azure Portal](https://portal.azure.com)

[Install Latest Version of Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

Check Azure CLI has been downoaded successfully:

    Az


## Managing your Application

### Prerequisites

Open up Windows PowerShell ISE and log in using your credentials:

    az login

You may need to authenticate your device to log in, so follow the prompt if any.

Enter the code that was displayed into your browser link, then choose your Microsoft account to be associated with Microsoft Azure Cross-platform Command Line Interface. Close the browser window after completed.

Check on your console that you have linked your Azure account successfully, a JSON file should verify this.

Next login to your Azure account from PowerShell using:

    Login-AzureRmAccount

Follow the pop-up window to continue.

### Set Parameters










