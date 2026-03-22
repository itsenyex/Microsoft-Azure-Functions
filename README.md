Azure Functions is Microsoft's serverless compute service that lets you run event-driven code without managing infrastructure. You write small pieces of code (functions) that execute in response to triggers like HTTP requests, timers, database changes, or messages in a queue.
Key Characteristics:

Serverless: No server management required
Event-driven: Functions execute in response to triggers
Pay-per-execution: You're only charged for the time your code runs
Auto-scaling: Automatically scales based on demand
Stateless or stateful: Support for both stateless functions and stateful workflows (Durable Functions)

Function Structure and Components
Triggers: What causes your function to run (HTTP request, timer, blob upload, queue message, etc.)
Bindings: Declarative way to connect to other services. Input bindings bring data in, output bindings send data out. This eliminates boilerplate code for connecting to databases, storage, and other services.
Function App: A container for one or more functions that share the same hosting plan, configuration, and deployment lifecycle.

Hosting Plans
Azure Functions offers three main hosting options: 

Consumption Plan
True serverless, pay only for execution time
Automatic scaling
5-minute default timeout (can extend to 10 minutes)
Best for sporadic workloads

Premium Plan
Pre-warmed instances (no cold starts)
Unlimited execution duration
VNET connectivity
More powerful instances
Predictable pricing

Dedicated (App Service) Plan
Runs on dedicated VMs
Good for long-running functions
Predictable costs
Can leverage existing App Service infrastructure

Supported Languages
Azure Functions supports multiple programming languages:
C#
JavaScript/TypeScript
Python
Java
PowerShell
Custom handlers (Go, Rust, etc.)

Setting Up VS Code for Azure Functions.
Prerequisites
Install VS Code:
Install Azure Functions Core Tools:
Install Azure Functions Extension:
Open VS Code

Go to Extensions (Ctrl+Shift+X)
Search for "Azure Functions"
Install the official Microsoft extension
Install Language-Specific Requirements:
Node.js, Python, or .NET


Creating Your First Function in VS Code
Step 1: Create a New Function Project
Press Ctrl+Shift+P (Command Palette)
Type "Azure Functions: Create New Project"
Select a folder for your project
Choose your language
Choose your trigger type (start with HTTP trigger)
Provide a function name
Choose authorization level (Anonymous for testing, Function for production)


Step 2: Project Structure
After creation, you'll see the codes, function and configuration and also python http trigger.

step 3: : Understanding the Code
For a Python HTTP trigger:

       import azure.functions as func
import logging

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')
    
    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
            name = req_body.get('name')
        except ValueError:
            pass
    
    if name:
        return func.HttpResponse(f"Hello, {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name parameter",
            status_code=400
        )


Step 4: Local Testing
Press F5 or click "Run and Debug"
The function runs locally with Azure Functions runtime
You'll see URLs in the terminal (typically http://localhost:7071/api/function_name)
Test with your browser, Postman, or curl.

Deployment OPtions:

Method 1: Deploy from VS Code (Easiest)

Sign in to Azure:

Click the Azure icon in VS Code sidebar
Click "Sign in to Azure"
Complete authentication


Deploy:

Right-click your project folder
Select "Deploy to Function App"
Choose "Create new Function App in Azure" (or select existing)
Provide a globally unique name
Select runtime stack (Python 3.11, Node.js 18, etc.)
Select region
Wait for deployment to complete


View Deployment:

After deployment, you'll see your function in the Azure extension
Right-click to access logs, settings, or the Azure portal


Method 2: Azure CLI Deployment


# Login to Azure
az login

# Create resource group
az group create --name MyResourceGroup --location eastus

# Create storage account (required for functions)
az storage account create \
  --name mystorageaccount \
  --resource-group MyResourceGroup \
  --location eastus \
  --sku Standard_LRS

# Create function app
az functionapp create \
  --resource-group MyResourceGroup \
  --consumption-plan-location eastus \
  --runtime python \
  --runtime-version 3.11 \
  --functions-version 4 \
  --name MyFunctionApp \
  --storage-account mystorageaccount

# Deploy your code
func azure functionapp publish MyFunctionApp
