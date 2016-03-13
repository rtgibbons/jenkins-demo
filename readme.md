# Jenkins Demo

The purpose of this is a simple demo to test out jenkins configs. This was built to show case [Jenkins Atlassian Theme](https://github.com/djonsson/jenkins-atlassian-theme) from [Daniel Jonsson](https://github.com/djonsson)

You can view the plugins installed at [jenkins/plugins](jenkins/plugins). The main ones installed Simple Theme, jQuery, Gravatar and Git Plugin.

Two jobs have been created, both pull from a github repo, and one is set to randomly fail and the other just does an `ls -la` of the $WORKSPACE

## To remove security restrictions

Be deafult this jenkins install is read-only for everyone with no users defined. To enable access to managed jenkins, you'll need to edit as follows

````
diff --git a/jenkins/config.xml b/jenkins/config.xml
index d258ddc..6e24f15 100644
--- a/jenkins/config.xml
+++ b/jenkins/config.xml
@@ -5,7 +5,17 @@
   <numExecutors>2</numExecutors>
   <mode>NORMAL</mode>
   <useSecurity>true</useSecurity>
-  <authorizationStrategy class="hudson.security.LegacyAuthorizationStrategy"/>
+  <authorizationStrategy class="hudson.security.GlobalMatrixAuthorizationStrategy">
+    <permission>hudson.model.Hudson.Read:anonymous</permission>
+    <permission>hudson.model.Item.Build:anonymous</permission>
+    <permission>hudson.model.Item.Discover:anonymous</permission>
+    <permission>hudson.model.Item.Read:anonymous</permission>
+    <permission>hudson.model.Item.Workspace:anonymous</permission>
+    <permission>hudson.model.View.Configure:anonymous</permission>
+    <permission>hudson.model.View.Create:anonymous</permission>
+    <permission>hudson.model.View.Delete:anonymous</permission>
+    <permission>hudson.model.View.Read:anonymous</permission>
+  </authorizationStrategy>
   <securityRealm class="hudson.security.SecurityRealm$None"/>
   <disableRememberMe>false</disableRememberMe>
   <projectNamingStrategy class="jenkins.model.ProjectNamingStrategy$DefaultProjectNamingStrategy"/>
````

## To run locally

The below will use the jenkins home directory inside the git repo, and will start jenkins on PORT 8080

    export JENKINS_HOME="jenkins"
    java -jar jenkins.war --httpPort=8080 --ajp13Port=-1 --httpsPort=-1

## To run on Heorku

    heroku apps:create example
    heroku config:add JENKINS_HOME="jenkins"
    git push heroku master