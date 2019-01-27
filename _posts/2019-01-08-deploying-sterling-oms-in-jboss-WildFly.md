---
layout: post
title:  "Deploying Sterling OMS in WildFly"
categories: [Sterling OMS Deployment, Install, WildFly]
---
# What is WildFly?
>WildFly is a flexible, lightweight, managed application runtime that helps build amazing applications.
> Source: http://wildfly.org/about/
>

WildFly uses JBoss Modules and is open source unlike Jboss


To deploy in WildFly build the Sterling OMS application as you would build for jboss. e.g.

```.\buildear.cmd -Dappserver=jboss -Dwarfiles=smcfs -Dearfile=smcfs.ear -Dnowebservice=true -Dsupport.multi.war=true -Dnodocear=true -Dasyncejb=false create-ear```

Once the ear is build - Unzip the ear and extract the smcfs.war directory to ```standalone\deployments\smcfs.war``` directory

In ```standalone\deployments\smcfs.war\WEB-INF``` folder edit the ```jboss-web.xml``` file with the below contents.

```
<?xml version="1.0" encoding="UTF-8"?>
<jboss-web>
  <security-domain flushOnSessionInvalidation="false">other</security-domain>
  <context-root>/smcfs</context-root>
</jboss-web>
```

Edit the deployment-scanner tag in standalone.xml file with auto-deploy-exploded attribute set to true. File ```standalone\configuration\standalone.xml``` 

```
<subsystem xmlns="urn:jboss:domain:deployment-scanner:2.0">
	<deployment-scanner path="deployments" relative-to="jboss.server.base.dir" scan-interval="5000" runtime-failure-causes-rollback="${jboss.deployment.scanner.rollback.on.failure:false}" auto-deploy-exploded="true"/>
</subsystem>
```

Edit the file ```bin\standalone.conf.bat``` to append the additional Sterling OMS related JVM arguments

```
set "JAVA_OPTS=%JAVA_OPTS% -Dvendor=shell -DvendorFile=/servers.properties -Dfile.encoding=UTF-8 -DLOGFILE=logs/JBOSS_sci.log -DSECURITY_LOGFILE=logs/JBOSS_security.log"
```

Navigate to WireFly Install directory bin folder and run the script ```standalone.bat```. Once server successfully starts you can see the below log messages.
```
12:01:00,886 INFO  [org.wildfly.extension.undertow] (ServerService Thread Pool -- 107) WFLYUT0021: Registered web context: '/smcfs' for server 'default-server'
12:01:00,901 INFO  [org.jboss.as.server] (ServerService Thread Pool -- 42) WFLYSRV0010: Deployed "smcfs.war" (runtime-name : "smcfs.war")
```

After the startup, navigate to the admin console and validate the deployment.




