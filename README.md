# AI Shopping Cart - App Template for Java, Azure OpenAI and Azure Spring Apps

[![Open in GitHub Codespaces](https://img.shields.io/badge/Github_Codespaces-Open-black?style=for-the-badge&logo=github
)](https://codespaces.new/Azure-Samples/app-templates-java-openai-springapps)
[![Open in Remote - Dev Containers](https://img.shields.io/badge/Dev_Containers-Open-blue?style=for-the-badge&logo=visualstudiocode
)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/Azure-Samples/app-templates-java-openai-springapps)

AI Shopping Cart is a sample application that supercharges your shopping experience with the power of AI. It leverages Azure OpenAI and Azure Spring Apps to build a recommendation engine that is not only scalable, resilient, and secure, but also personalized to your needs. Taking advantage of Azure OpenAI, the application performs nutrition analysis on the items in your cart and generates the top 3 recipes using those ingredients. With Azure Developer CLI (azd), you’re just a few commands away from having this fully functional sample application up and running in Azure. Let's get started!

![AI Shopping Cart](./assets/ai-shopping-cart.png)

## Pre-requisites

- [Install the Azure Developer CLI](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd)
- An Azure account with an active subscription. [Create one for free](https://azure.microsoft.com/free).
- [OpenJDK 17](https://learn.microsoft.com/en-us/java/openjdk/install)
- [Node.js 20.5.0+](https://nodejs.org/en/download/)
- [Docker](https://docs.docker.com/get-docker/)
- [Azure OpenAI with gpt-4](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview#how-do-i-get-access-to-azure-openai) <sup>[\[Note\]](#note-on-azure-openai)</sup>
- Review the [architecture diagram and the resources](#application-architecture) you'll deploy

## Quickstart

To learn how to get started with any template, follow [this quickstart](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/get-started?tabs=localinstall&pivots=programming-language-java). For this template `Azure-Samples/app-templates-java-openai-springapps`, you need to execute a few additional steps as described below.

This quickstart will show you how to authenticate on Azure, enable Spring Apps [alpha feature](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/feature-versioning#alpha-features) for azd, initialize using a template, set the [environment variables](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/manage-environment-variables) for Azure OpenAI, provision the infrastructure, and deploy the code to Azure:

```bash
# Log in to azd if you haven't already
azd auth login

# Enable Azure Spring Apps alpha feature for azd
azd config set alpha.springapps on

# First-time project setup. Initialize a project in the current directory using this template
azd init --template Azure-Samples/app-templates-java-openai-springapps

# Set the environment variables for Azure OpenAI
azd env set azureOpenAiApiKey <replace-with-Azure-OpenAi-API-key> 
azd env set azureOpenAiEndpoint <replace-with-Azure-OpenAi-endpoint>
azd env set azureOpenAiDeploymentId <replace-with-Azure-OpenAi-deployment-id>

# Provision and deploy to Azure
azd up
```

> Note: Replace the placeholders with the values from your Azure OpenAI resource.

At the end of the deployment, you will see the URL of the front-end. Open the URL in a browser to see the application in action.

## Application Architecture

This sample application uses the following Azure resources:

- [Azure Container Apps (Envirornment)](https://learn.microsoft.com/en-us/azure/container-apps/) to host the frontend as a Container App and Azure Spring Apps [Standard comsumption and dedicated plan](https://learn.microsoft.com/en-us/azure/spring-apps/overview#standard-consumption-and-dedicated-plan)
- [Azure Spring Apps](https://learn.microsoft.com/azure/spring-apps/) to host the AI Shopping Cart Service as a Spring App
- [Azure Container Registry](https://learn.microsoft.com/azure/container-registry/) to host the Docker image for the frontend
- [Azure Database for PostgreSQL (Flexible Server)](https://learn.microsoft.com/azure/postgresql/) to store the data for the AI Shopping Cart Service
- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/) for monitoring and logging
- [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) to perform nutrition analysis and generate top 3 recipes. It is not deployed with the sample app<sup>[\[Note\]](#note-on-azure-openai)</sup>.

Here's a high level architecture diagram that illustrates these components. Excepted Azure OpenAI, all the other resources are provisioned in a single [resource group](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) that is created when you create your resources using `azd up`.

![Architecture diagram](./assets/architecture-diagram.png)

> This template provisions resources to an Azure subscription that you will select upon provisioning them. Please refer to the [Pricing calculator for Microsoft Azure](https://azure.microsoft.com/pricing/calculator/) and, if needed, update the included Azure resource definitions found in `infra/main.bicep` to suit your needs.

## Application Code

This template is structured to follow the [Azure Developer CLI template convetions](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/make-azd-compatible?pivots=azd-create#azd-conventions). You can learn more about `azd` architecture in [the official documentation](https://learn.microsoft.com/azure/developer/azure-developer-cli/make-azd-compatible?pivots=azd-create#understand-the-azd-architecture).

## Next Steps

At this point, you have a complete application deployed on Azure. But there is much more that the Azure Developer CLI can do. These next steps will introduce you to additional commands that will make creating applications on Azure much easier. Using the Azure Developer CLI, you can setup your pipelines, monitor your application, test and debug locally.

- [`azd pipeline config`](https://learn.microsoft.com/azure/developer/azure-developer-cli/configure-devops-pipeline?tabs=GitHub) - to configure a CI/CD pipeline (using GitHub Actions or Azure DevOps) to deploy your application whenever code is pushed to the main branch. 

- [`azd monitor`](https://learn.microsoft.com/azure/developer/azure-developer-cli/monitor-your-app) - to monitor the application and quickly navigate to the various Application Insights dashboards (e.g. overview, live metrics, logs)

- [Run and Debug Locally](https://learn.microsoft.com/azure/developer/azure-developer-cli/debug?pivots=ide-vs-code) - using Visual Studio Code and the Azure Developer CLI extension

- [`azd down`](https://learn.microsoft.com/azure/developer/azure-developer-cli/reference#azd-down) - to delete all the Azure resources created with this template 

### Additional `azd` commands

The Azure Developer CLI includes many other commands to help with your Azure development experience. You can view these commands at the terminal by running `azd help`. You can also view the full list of commands on our [Azure Developer CLI command](https://aka.ms/azure-dev/ref) page.

## Note on Azure OpenAI

This sample application uses Azure OpenAI. It is not part of the automated deployment process. You will need to create an Azure OpenAI resource and configure the application to use it. Please follow the instructions in the [Azure OpenAI documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/overview#how-do-i-get-access-to-azure-openai) to get access to Azure OpenAI. This sample app was developped and test using `gpt-4` model. You need the following information from the Azure OpenAI resource to configure the application:

- `azureOpenAiApiKey` - Azure OpenAI API key
- `azureOpenAiEndpoint` - Azure OpenAI endpoint
- `azureOpenAiDeploymentId` - Azure OpenAI deployment ID of `gpt-4` model

> Note: The sample application could be used with `gpt-3.5` model as well. However, it is currently not tested.

- [Pre-requisites](#pre-requisites) :arrow_heading_up:
- [Application Architecture](#application-architecture) :arrow_heading_up:

## Resources

These are additional resources that you can use to learn more about the sample application and its underlying technologies.

- [Start from zero and scale to zero – Azure Spring Apps consumption plan](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/)
