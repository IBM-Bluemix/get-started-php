---

copyright:
  years: 2017
lastupdated: "2017-02-21"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:app_name: data-hd-keyref="app_name"}

# Getting started with PHP on Bluemix
{: #getting_started}

  Congratulations.  You have deployed a Hello World sample application  on {{site.data.keyword.Bluemix}}.  Download the sample code to get started or go through the step-by-step guide.
    <a class="xref" href="http://bluemix.net" target="_blank" title="(Opens in a new tab or window)"><img class="image" src="/docs/runtimes/images/btn_starter-code.svg" alt="Download application code" /> </a>
  {: download}

This guide will take you through the steps to get started with a simple PHP application in {{site.data.keyword.Bluemix}} and help you:
  * set up a development environment
  * download sample code
  * run locally
  * run on {{site.data.keyword.Bluemix_notm}}, and
  * add a {{site.data.keyword.Bluemix_notm}} service to the sample


## Prerequisites
{: #prereqs}

You'll need the following:
* [{{site.data.keyword.Bluemix_notm}} account](https://console.ng.bluemix.net/registration/)
* [Cloud Foundry CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}
* [Git ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://git-scm.com/downloads){: new_window}
* [PHP ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://php.net/downloads.php){: new_window}
* [Composer ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://getcomposer.org/download/){: new_window}

## 1. Clone the sample app
{: #clone}

Now you're ready to start working with the app. Clone the repo and change the directory to where the sample app is located.
  ```
git clone https://github.com/IBM-Bluemix/get-started-php
  ```
  {: pre}
  ```
cd get-started-php
  ```
  {: pre}

## 2. Run the app locally

Install dependencies
```
php composer.phar install
```
{: pre}

Run the app
  ```
php -S localhost:8000
  ```
  {: pre}

View your app at: http://localhost:8000

## 3. Prepare the app for deployment
{: #prepare}

To deploy to {{site.data.keyword.Bluemix_notm}}, it can be helpful to set up a manifest.yml file. One is provided for you with the sample. Take a moment to look at it.

The manifest.yml includes basic information about your app, such as the name, how much memory to allocate for each instance and the route. In this manifest.yml **random-route: true** generates a random route for your app to prevent your route from colliding with others. You can replace **random-route: true** with **host: myChosenHostName**, supplying a host name of your choice. [Learn more...](/docs/manageapps/depapps.html#appmanifest)
 ```
 applications:
 - name: GetStartedPHP
   random-route: true
   memory: 128M
 ```
 {: codeblock}

Edit the manifest.yml file and change the `name` from `GetStartedPHP` to your app name, <var class="keyword varname" data-hd-keyref="app_name">app_name</var>.
{: download}

## 4. Deploy the app
 {: #deploy}

You can use the Cloud Foundry CLI to deploy apps.

Choose your API endpoint
   ```
cf api <API-endpoint>
   ```
   {: pre}

Replace the *API-endpoint* in the command with an API endpoint from the following list.

| URL                            | Region         |
|:-------------------------------|:---------------|
| https://api.ng.bluemix.net     | US South       |
| https://api.eu-gb.bluemix.net  | United Kingdom |
| https://api.au-syd.bluemix.net | Sydney         |

Login to your {{site.data.keyword.Bluemix_notm}} account

   ```
cf login
   ```
   {: pre}

 From within the *get-started-php* directory push your app to {{site.data.keyword.Bluemix_notm}}
   ```
cf push
   ```
   {: pre}

 This can take a minute. If there is an error in the deployment process you can use the command `cf logs <Your-App-Name> --recent` to troubleshoot.

 When deployment completes you should a message indicating that your app is running.  View your app at the URL listed in the output of the push command.  You can also issue the
  ```
cf apps
  ```
  {: pre}
  command to view your apps status and see the URL.

## 5. Add a database
{: #add_database}

Next, we'll add a NoSQL database to this application and set up the application so that it can run locally and on {{site.data.keyword.Bluemix_notm}}.

1. Log in to {{site.data.keyword.Bluemix_notm}} in your Browser. Browse to the `Dashboard`. Select your application by clicking on its name in the `Name` column.
2. Click on `Connections` then `Connect new`.
3. In the `Data & Analytics` section, select `Cloudant NoSQL DB` and `Create` the service.
4. Select `Restage` when prompted. {{site.data.keyword.Bluemix_notm}} will restart your application and provide the database credentials to your application using the `VCAP_SERVICES` environment variable. This environment variable is only available to the application when it is running on {{site.data.keyword.Bluemix_notm}}.

Environment variables enable you to separate deployment settings from your source code. For example, instead of hardcoding a database password, you can store this in an environment variable which you reference in your source code. [Learn more...](/docs/manageapps/depapps.html#app_env)
{: tip}

## 6. Use the database
{: #use_database}
We're now going to update your local code to point to this database. We'll create a json file that will store the credentials for the services the application will use. This file will get used ONLY when the application is running locally. When running in {{site.data.keyword.Bluemix_notm}}, the credentials will be read from the VCAP_SERVICES environment variable.

1. Create a file called `.env` in the `get-started-php` directory with the following content:
  ```
  CLOUDANT_HOST=
  CLOUDANT_USERNAME=
  CLOUDANT_PASSWORD=
  ```
  {: pre}

2. Back in the {{site.data.keyword.Bluemix_notm}} UI, select your App -> Connections -> Cloudant -> View Credentials

3. Copy and paste just the `url` from the credentials to the `CLOUDANT_URL` field of the `.env` file and save the changes.  The result will be something like:
  ```
  CLOUDANT_HOST=abc...yz.cloudant.com
  CLOUDANT_USERNAME=abc...yz
  CLOUDANT_PASSWORD=445d...d1a
  ```

4. Run your application locally.
  ```
php -S localhost:8000
  ```
  {: pre}

  View your app at: http://localhost:8080. Any names you enter into the app will now get added to the database.

  Your local app and  the {{site.data.keyword.Bluemix_notm}} app are sharing the database.  View your {{site.data.keyword.Bluemix_notm}} app at the URL listed in the output of the push command from above.  Names you add from either app should appear in both when you refresh the browsers.



Remember if you don't need your app live, stop it so you don't incur any unexpected charges.
{: tip}  
