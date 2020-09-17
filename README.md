# FlaskWebApp
A sample flask web applicaiton

1. Create a Virtual environment

        conda create -n webapp python==3.7
        conda activate webapp

2. create requirements.txt file. install dependencies.

        pip install -r requirements.txt
    
4. Instruction to create, test and deploy to azure app service
        
        https://docs.microsoft.com/en-us/azure/app-service/quickstart-python?tabs=powershell
5. Run locally
        
        Set-Item Env:FLASK_APP ".\application.py"
        flask run
6. Install azue CLI using powershell: [official docs](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-powershell)

        Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi

This did not install CLI for me (probably permission issue). So run these commands one by one.

        Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
        
        Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi'
        
        rm .\AzureCLI.msi

Restart powershell

        az login
7. Deploy flask web app to azure app service.

        az webapp up --sku F1 -n krishan-test-flask

This failed for me. Probably because it tries to create a new resource group in a default subscription which requires owner access. 
        
I set default subscription and passed a resource group where I have owner access. 

        az account set --subscription "<subscription id>"
        az webapp up -n krishan-test-flask --resource-group <existing-rg-name>

It takes some time but finally created the webapp. I could view my web app in the azure portal's app service section

        {
        "URL": "http://krishan-test-flask.azurewebsites.net",
        "appserviceplan": "krkusuk_asp_Linux_centralus_0",
        "location": "centralus",
        "name": "krishan-test-flask",
        "os": "Linux",
        "runtime_version": "python|3.7",
        "runtime_version_detected": "-",
        "sku": "PREMIUMV2",
        "src_path": "C:\\Users\\krkusuk\\repos\\flaskWebApp"
        }
The webapp is up and running

8. Add AAD authentication to webapp
This will restrict access to people with from your company.. 
https://docs.microsoft.com/en-us/azure/app-service/configure-authentication-provider-aad#-configure-with-express-settings

Did not understand point 5.

9. Set up continuous integration pipeline.

We want the app service to be updated whenever we checkin new changes.

First make some change to the hello() function

```python
        return "Hello World Again!"
```

        az webapp up

https://krishan-test-flask.azurewebsites.net/
