logentries-wildfly
=================

Simple adapter to use LogEntries' AsyncLogger with Wildfly

Why you don't use the com.logentries.jul.LogentriesHandler directly?
------

The class com.logentries.jul.LogentriesHandler have the token property as a byte[] and I dont know how to
set the token as a byte[] in the Wildfly configuration


To Use
------

1. On logentries website, create a new log and have the token available.
1. Build the jar:

        mvn clean install

1. Create an a new Wildfly module with the Wildfly CLI:

     module add --name=com.upplication.logentries --resources=$LOGENTRIES_JAR_PATH_WITH_DEPENDENCIES --dependencies=javax.api,org.jboss.logging

1. Create a custom handler:

     /subsystem=logging/custom-handler=logentries:add(class=us.bigd.logentries.LogEntriesAdapterHandler, module=com.upplication.logentries, formatter="%d{HH:mm:ss,SSS} %-5p [%c] (%t) %s%E%n", properties={token="$YOUR_TOKEN"})

1. Add  handler to root-logger

    /subsystem=logging/root-logger=ROOT:root-logger-assign-handler(name="logentries")
    /subsystem=logging/root-logger=ROOT:write-attribute(name="level", value="INFO")


1. Fire up Wildfly and monitor logs to confirm the module loads without errors.
1. Monitor logentries log for incoming data.
    