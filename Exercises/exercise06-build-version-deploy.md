# Exercise 6 Build, Version, Deploy, & Debug


## Learnings

1. Setting up automated build
1. Connecting VSTS and Azure
1. Deploying to your Development Location
1. Basic configuration (here: enable remote debugging) of web apps
1. Connecting to Azure using Visual Studio Server Explorer
1. Remote debugging of web apps

> Note we will stick with using the wizard to create the build. Later we will deep dive into how team build works

## Setup Build

1. Open open a web browser and point to https://practicaldevops.visualstudio.com/

2. Navigate to your team project and click on `Code` to open the code hub. You should now be seeing your repository

1. Click on *build setup now* in VSTS.<br/>
   ![Setup Build](img/vsts-setup-build.png)

1. Accept the suggestions of the Build Setup Wizard.

1. **Discussion points:**
   * Build process walk-through
   * Overview about additional build steps that would be possible

1. Add the following arguments to MSBuild in step *Visual Studio Build* (necessary for creating the Web Deploy package): `/p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="$(build.stagingDirectory)"`<br/>
   ![MSBuild Arguments](img/vsts-msbuild-arguments.png)

1. Save the generated build definition with the name `PracticalDevOps Build`
   
1. Setup Continuous Integration (CI) by creating a trigger.<br/>
   ![Continuous Integration](img/vsts-trigger-build.png)

1. Queue a new build.<br/>
   ![Queue Build](img/vsts-queue-build.png)

1. Watch how the hosted build controller builds your code. You should not get any errors.

1. Take a look at the build results.<br/>
   ![Build results](img/vsts-build-results.png)
   
1. Take a look at the Test Results, you can click `Detail report` to see details
   
1. Click at the `Artifacts` link in the build detail and select `explore`
   
## Setup CD to Azure

1. In VSTS project options, goto *Services* area. Add a service endpoint for Azure<br/>
   ![VSTS Services](img/vsts-connect-azure.png)

1. Select *Certificate Based* and follow the link *download publish settings file*. Open the publish settings file and copy the required data into VSTS.
   
1. Add a `Azure Web App Deployment` task. Use `$(build.artifactstagingdirectory)\**\*.zip` in the web deploy package
or click on the picker and manually select the package full path<br/>
   ![Azure Web App Deployment](img/vsts-azure-web-app-deployment.png)

1. Now you can test the CD process. Change someting in your code (e.g. appending a `!` to the title) and check your code in. The build should be triggered automatically. The deployment should be created after the successful build. The new version should be immediately published to Azure App Services. 

## Remote Debugging

1. **Discussion points:**
   * Troubleshooting options for Web Apps

1. Enable remote debugging for your web app in the Azure portal.<br/>
   ![Enable Remote Debugging](img/enable-remote-debugging.png)

1. Open Visual Studio's *Server Explorer* and attach a debugger to your deployed web app. This operation may take a few moments.<br/>
   ![Attach Debugger](img/attach-debugger-server-explorer.png)

1. Open `Controllers/BooksController.cs` and set a breakpoint (F9) to the first line of the `Get` method. This operation may take a few moments.

1. Open `http://practicaldevops-dev.azurewebsites.net/api/books` in a browser and note how you can debug the deployed version of your web app with your local Visual Studio.

   
## Further Ideas

If you have time left, you could additionally cover topics like:

* Show Kudu *scm* service at `https://<sitename>.scm.azurewebsites.net/`
* Show FTP access to Web Apps (e.g. for downloading logs)
* Show Visual Studio Online for interactive access to Web App content
* Demonstrate deployment slots and VIP swaps

## Further Ideas

If you have time left, you could additionally cover topics like:

* Setup an additional build agent in a VM

