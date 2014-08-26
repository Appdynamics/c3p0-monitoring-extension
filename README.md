# AppDynamics C3P0 Monitoring Extension

This extension works only with the standalone machine agent.

##Use Case

c3p0 is a connection pooling library for making traditional JDBC drivers "enterprise-ready" by augmenting them with functionality defined by the jdbc3 spec and the optional extensions to jdbc2. c3p0 registers MBeans under `com.mchange.v2.c3p0`, one with statistics about the library as a whole (called C3P0Registry), and an MBean for each PooledDataSource you deploy. Please refer [here](http://www.mchange.com/projects/c3p0/#jmx_configuration_and_management) for details.
This eXtension inspects and collects c3p0 datasource statistics exposed through MBeans and reports them to AppDynamics Controller.

##Prerequisites

Please enable JMX in the container if not already enabled that hosts the application using c3p0 connection pooling mechanism. 

##Installation

1. Run 'mvn clean install' from the c3p0-monitoring-extension directory and find the C3P0Monitor.zip in the 'target' directory.
2. Unzip C3P0Monitor.zip and copy the 'C3P0Monitor' directory to `<MACHINE_AGENT_HOME>/monitors/`
3. Configure the extension by referring to the below section.
5. Restart the Machine Agent.

In the AppDynamics Metric Browser, look for: Application Infrastructure Performance  | \<Tier\> | Custom Metrics | C3P0 in case of default metric path

## Configuration

Note : Please make sure not to use tab (\t) while editing yaml files. You can validate the yaml file using a [yaml validator](http://yamllint.com/)

1. Configure the C3P0 Extension by editing the config.yml file in `<MACHINE_AGENT_HOME>/monitors/C3P0Monitor/`.
2. Specify the container instance host, JMX port, username and password in the config.yml. Configure the MBeans for this extension to report the metrics to Controller. By default, `com.mchange.v2.c3p0` is the domain name. The extension by default reports the metrics of MBean C3P0Registry, one with statistics about the c3p0 library. Specify the dataSourceName you want to exclude from monitoring as a comma separated values in the keyproperty 'excludePooledDataSources'. PooledDataSource MBean ObjectName is of the form 'com.mchange.v2.c3p0:type=PooledDataSource\[z8kfsx944ur8uu1osma2k|1b1a3c12]\'. Please refer [here](http://www.mchange.com/projects/c3p0/#dataSourceName) for details.
You can also add excludePatterns (regex) to exclude any metric tree from showing up in the AppDynamics controller.

   For eg.
   ```
        # Server hosting the C3P0 application specifics
        server:
            host: "localhost"
            port: 9044
            username: ""
            password: ""
            

        # C3P0 MBeans. Exclude patterns with regex can be used to exclude any unwanted metrics.
        mbeans:
            domainName: "com.mchange.v2.c3p0"
            excludePooledDataSources: [z8kfsx944ur8uu1osma2k|1b1a3c12]
            excludePatterns: [.*AllIdentityTokenized$, .*AllPooledDataSources$]

        #prefix used to show up metrics in AppDynamics
        metricPrefix:  "Custom Metrics|C3P0|"

   ```
   
3. Configure the path to the config.yml file by editing the <task-arguments> in the monitor.xml file in the `<MACHINE_AGENT_HOME>/monitors/TomcatMonitor/` directory. Below is the sample

     ```
     <task-arguments>
         <!-- config file-->
         <argument name="config-file" is-required="true" default-value="monitors/C3P0Monitor/config.yml" />
          ....
     </task-arguments>
    ```



##Metrics

* C3P0Registry: AllIdentityTokenCount, AllIdentityTokenizedCount, AllPooledDataSourcesCount, NumPooledDataSources, NumPoolsAllDataSources
* PooledDataSource: acquireIncrement, acquireRetryAttempts, acquireRetryDelay, checkoutTimeout, effectivePropertyCycleDefaultUser, idleConnectionTestPeriod, initialPoolSize, loginTimeout, maxAdministrativeTaskTime, maxConnectionAge, maxIdleTime, maxIdleTimeExcessConnections, maxPoolSize, maxStatements, maxStatementsPerConnection, minPoolSize, numBusyConnections, numBusyConnectionsAllUsers, numBusyConnectionsDefaultUser, numConnections, numConnectionsAllUsers, numConnectionsDefaultUser, numFailedCheckinsDefaultUser, numFailedCheckoutsDefaultUser, numFailedIdleTestsDefaultUser, numHelperThreads, numIdleConnections, numIdleConnectionsAllUsers, numIdleConnectionsDefaultUser, numThreadsAwaitingCheckoutDefaultUser, numUnclosedOrphanedConnections, numUnclosedOrphanedConnectionsAllUsers, numUnclosedOrphanedConnectionsDefaultUser, numUserPools, propertyCycle, startTimeMillisDefaultUser, statementCacheNumCheckedOutDefaultUser, statementCacheNumCheckedOutStatementsAllUsers, statementCacheNumConnectionsWithCachedStatementsAllUsers, statementCacheNumConnectionsWithCachedStatementsDefaultUser, statementCacheNumStatementsAllUsers, statementCacheNumStatementsDefaultUser, threadPoolNumActiveThreads, threadPoolNumIdleThreads, threadPoolNumTasksPending, threadPoolSize, unreturnedConnectionTimeout, upTimeMillisDefaultUser


## Custom Dashboard
![](https://github.com/Appdynamics/c3p0-monitoring-extension/raw/master/Dashboard.png)

##Contributing

Always feel free to fork and contribute any changes directly here on GitHub.

##Community

Find out more in the [AppSphere](http://community.appdynamics.com/t5/AppDynamics-eXchange/C3P0-Monitoring-Extension/idi-p/9070) community.

##Support

For any questions or feature request, please contact [AppDynamics Center of Excellence](mailto:ace-request@appdynamics.com).
