# Exercise 7 - Release Management

## Learnings

1. Setting up VSTS Release Management 

## Setup Release Management

1. Open the most recent build in VSTS

1. In build results, follow the link to create a release.<br/>
   ![Create release](img/vsts-setup-release.png)

1. **Discussion points:**
   * Describe concepts of VSTS's release management
   * Release process walk-through
   * Overview about additional steps that would be possible
   
1. Setup deployment to Azure Web App.<br/>
   ![Azure Web App Deployment](img/vsts-azure-web-app-deployment.png)

1. Setup Continuous Deployment by creating a trigger.<br/>
   ![Release CD](img/vsts-release-ci.png)
   
1. In order to automate deployment, create a *Deployment Condition*.<br/>
   ![Deployment Condition](img/vsts-create-deployment-condition.png)

1. Trigger deployment automatically whenever a release has been created.<br/>
   ![Trigger deployment](img/vsts-automatic-deployment-at-release.png)

1. Now you can test the entire pipeline. Change something in your code (like adding an index.html to the root of the project) and check your code in. The build should be triggered automatically. The release should be created after the successful build. The release should be immediately published to Azure App Services.

1. Notice the build will run, and after it is run a new release will be automatically deployed.

