# Using Catalog Apps

## Deploying
If the application that you're launching is a Helm app, you can view the instructions and manually enter the key/value pairs to configure the app.

If the application is a Rancher app, you will be presented with a form that contains sane defaults for the application and allows you to make adjustments visually.

> **NOTE:** When installing a Rancher App like Chartmuseum, the installation forms will show a more comprehensive interface where it's possible to answer questions using a form with default values that make sense.  

### Rancher Apps deploying benefits
The Rancher interfaces not only makes it faster and more accessible for users to launch applications, but it will also show the difference in files between one version of a chart and another.  
When you perform an upgrade you know exactly what to expect and if there is a pronlem with the app all of its components and dependencies are visible in a single troubleshooting page.  
You will be able to quickly diagnose the status of every component that powers it.

Let's look at the installations forms provided by Longhorn.  
Many of the configuration options require additional explanation and the format of the Rancher Apps makes it possible for you to provide that additional context beyond what you can do in a helm chart.

### Creating Custom Catalogs Apps
The extra steps required to extend a helm chart to be a rancher app are clearly explained in [Rancher documentations](https://rancher.com/docs/rancher/v2.x/en/helm-charts/legacy-catalogs/adding-catalogs/): it includes a summary of the directory structure, an explanations of the additional files, variable reference and then a tutorial on how to build your own Rancher app.

## Cloning
The same application can run multiple times in any project.  
Each successive version of the application will run it its own namespace.  
Rancher names the namespace dynamically by appending five characters to the default name, but you can override this and choose your own name.

If you wish to run more copies of an application that you've already launched, cloning it will populate the form or Helm values with the values from the app you're cloning.

### Testing new version
When cloning a Rancher Apps it's also possible to change the data insered in the source installation, indeed it's possible to change the App version before clone it.  
This is really useful to test a new release of the app before rolling out in a production environment.

After test, it's possible to do this 2 choice:
- Mark the cloned app as the new production app and delete the source one.
- Perform the same upgrade on the current version of the app.

## Upgrading
When newer versions of application templates are made available by the maintainers, Rancher detects this and changes the icon of the installed application to reflect that an upgrade is available.  
When you choose to upgrade the app, you're presented with the values you set when you first deployed the app.  
You can change these values, add new ones, or in the case of a Helm chart, add new key/value pairs to reflect any change to the application.

Some applications will not allow you to change certain fields. If you select "Delete and recreate resources if needed", then Rancher will respond to a failure to update an immutable field by deleting the application and redeploying it with the upgrade values.  
This is not without risk.  
If any other dependencies are missing when the app is redeployed, the upgrade will fail.  
When you're ready, click Launch, and Rancher will upgrade the app.

## Rolling Back
Any application can be rolled back to a previous version of the template by selecting Rollback from the menu and then following the prompts.

## Deleting
When you delete a catalog app, Rancher does not automatically delete the namespace into which the app was installed.  
This prevents Rancher from accidentally deleting other workloads they may have been installed through other means.  
After deleting an application, if you wish to delete the namespace, you can do so manually from the project menu.
