Heroku Jenkins Plugin
======================

A plugin for interacting with [Heroku](http://heroku.com) during [Jenkins](http://jenkins-ci.org/) builds.

Installing
----------

This plugin is available in the Jenkins plugin manager. 
To install in Jenkins, go to Manage Jenkins | Manage Plugins | Available | Heroku Plugin for Jenkins | Install. 
You must restart Jenkins to complete the installation.

Configuration
-------------

The Heroku API key used by build steps can be configured either with a global default or for individual build steps.
To configure a global default API key, go to Manage Jenkins | Heroku | Default API Key. This key will be used
by Heroku build steps unless otherwise overridden. If no global default is specified, individual build steps must specify
their own API keys under their respective Advanced settings. Your Heroku API key can be obtained from your [Heroku account page](https://api.heroku.com/account).

All build steps also include the following fields:

 - API Key: Your Heroku API key to use for deployment. This is only required if a global default is not already configured. Your Heroku API key can be obtained from your [Heroku account page](https://api.heroku.com/account).
 - App Name: The app to which to deploy. If the app exists, you must have access to deploy new releases. If the app does not exist and the name is not already taken, it will be created for you.

Buildsteps
==========

WAR Deployment
--------------

The `Deploy WAR Artifact` build step deploys a WAR file generated by your build directly to a Heroku app.

To deploy a WAR file, first make sure your build is successfully creating a deployable WAR file.
If you are using Maven with `<packaging>war</packaging>`, the `mvn package` command will output the WAR file into its `target` directory.
Otherwise, create the WAR file in whatever way is approiate for your build.

After the WAR file is created, add the `Heroku: Deploy WAR Artifact` build step to your under the Jenkins build configuration
and specify the relative path to the WAR file created in a previous build step to deploy.

Set Configuration
-----------------

The `Set Configuration` build step adds or updates [config vars](https://devcenter.heroku.com/articles/config-vars) to the specified app.

Scale Process
-------------

The `Scale Process` build step [scales](https://devcenter.heroku.com/articles/scaling) a process type to a specified quantity.

Run Process
-----------

The `Run Process` build step [runs a one-off process](https://devcenter.heroku.com/articles/cedar#oneoff_processes) on a dyno.

Rollback
--------

The `Rollback` build step [rolls back](https://devcenter.heroku.com/articles/releases#rollback) to the previous release.

Restart
--------

The `Restart` build step [restart](https://devcenter.heroku.com/articles/ps#process_restarts) the specified app.

Maintenance Mode
----------------

The `Maintenance Mode` build step [toggle maintenance mode](https://devcenter.heroku.com/articles/maintenance-mode) for a given app.


Building & Installing from Source
=================================

1. Follow instructions on [setting up your environment](https://wiki.jenkins-ci.org/display/JENKINS/Plugin+tutorial#Plugintutorial-SettingUpEnvironment)
   from Jenkins

2. This plugin has a dependency on [direct-to-heroku-client](https://github.com/heroku/direct-to-heroku-client-java),
   which is not yet in Maven central. To build this plugin, you must first build the client from source:

     `git clone git://github.com/heroku/direct-to-heroku-client-java.git`

     `mvn clean install -DskipTests`

3. Build the plugin:

     `git clone git://github.com/heroku/heroku-jenkins-plugin.git`

     `mvn clean package -DskipTests`

     or with tests:

     `mvn clean package -Dheroku.apiKey="<test user api key>" -Dheroku.appName="<test app>"`

4. This will create a `*.hpi` file in the `target` directory. 

5. On your Jenkins instance, go to Manage Jenkins | Manage Plugins | Advanced | Upload Plugin.

6. Choose the generated `*.hpi` file and click upload.

7. Restart Jenkins.


Changelog
=========

Version 0.7.1 (October 28, 2012)
--------------------------------
 - Fixed [issue](https://github.com/heroku/heroku-jenkins-plugin/issues/14) with app name not appearing in Maintenance Mode build step

Version 0.7 (August 23, 2012)
-----------------------------
 - API Key field in Advanced section of build steps
 - Send Feedback links in build step
 - Improved error handling of authentication issues
 - Standardization of build output and alignment with the Heroku CLI client
 - Deprecated Workspace Deploy build step
 - Changed to Jersey Client HTTP/JSON provider
 - Upgraded to heroku-api 0.12

Version 0.6 (July 27, 2012)
---------------------------
 - New Restart build step
 - New Set Configuration build step
 - Upgrade to direct-to-heroku-client 0.6-BETA

Version 0.5 Beta (June 27, 2012)
--------------------------------
 - New Maintenance Mode build step
 - Upload message during artifact deployment
 - Upgrade to heroku-api 0.11
 - Upgrade to direct-to-heroku-client 0.5-BETA

Version 0.4 Beta (June 22, 2012)
-------------------------------
 - New Build Steps
   - Workspace Deployment
   - Scale Process
   - Run Process
   - Rollback
 - Upgrade to heroku-api 0.9

Version 0.3 Beta (June 2, 2012)
-------------------------------
 - Support for master-slave setups

Version 0.2 Beta (May 15, 2012)
-------------------------------
 - Upgrade to heroku-api 0.8
 - Upgrade to direct-to-heroku-client 0.4-BETA
 - First release to jenkins-ci.org

Version 0.1 Beta (May 8, 2012)
------------------------------
- WAR Deployment
 