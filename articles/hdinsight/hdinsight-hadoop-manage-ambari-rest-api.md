---
title: Monitor and manage Azure HDInsight using Ambari REST API | Microsoft Docs
description: Learn how to use Ambari to monitor and manage Linux-based HDInsight clusters. In this document, you will learn how to use the Ambari REST API included with HDInsight clusters.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/02/2016
ms.author: larryfr

---
# Manage HDInsight clusters by using the Ambari REST API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API. Ambari is included on HDInsight clusters that use the Linux operating system, and is used to monitor the cluster and make configuration changes. In this document, you learn the basics of working with the Ambari REST API by performing common tasks using cURL.

> [!IMPORTANT]
> The examples in this document were tested using PowerShell on Windows 10 and Bash. In many cases, the same command works on both. In cases where there is a difference, both a PowerShell and Bash example is provided.

## Prerequisites

* An HDInsight cluster that uses the Linux operating system. See [Get started with HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) for more information on creating a cluster...

  > [!IMPORTANT]
  > Linux is the only operating system used on HDInsight version 3.4 or greater. For more information, see [HDInsight Deprecation on Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* [cURL](http://curl.haxx.se/): cURL is a cross-platform utility that can be used to work with REST APIs from the command-line. In this document, it is used to communicate with the Ambari REST API.

    > [!WARNING]
    > If you are using PowerShell, you must remove the default alias for the `curl` command by using the `remove-item alias:curl` command. If you do not remove the alias, you will receive errors such as "Parameter cannot be processed because the parameter name 'u' is ambiguous."

* [jq](https://stedolan.github.io/jq/): jq is a cross-platform command-line utility for working with JSON documents. In this document, it is used to parse the JSON documents returned from the Ambari REST API.

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (preview): a cross-platform command-line utility for working with Azure services.
  
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

## <a id="whatis"></a>What is Ambari?

[Apache Ambari](http://ambari.apache.org) makes Hadoop management simpler by providing an easy-to-use web UI that can be used to provision, manage, and monitor Hadoop clusters. Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari is provided by default with Linux-based HDInsight clusters.

## REST API

The base URI for the Ambari REST API on HDInsight is https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, where **CLUSTERNAME** is the name of your cluster.

> [!IMPORTANT]
> While the cluster name in the fully qualified domain name (FQDN) part of the URI (CLUSTERNAME.azurehdinsight.net) is case-insensitive, other occurrences in the URI are case-sensitive. For example, if your cluster is named MyCluster, the following are valid URIs:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> The following URIs return an error because the second occurrence of the name is not the correct case.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Connecting to Ambari on HDInsight requires HTTPS. When authenticating the connection, you must use the admin account name (the default is **admin**,) and password you provided when the cluster was created.

The following example uses cURL to make a GET request against the REST API. Replace **PASSWORD** with the admin password for the cluster. Replace **CLUSTERNAME** with the name of the cluster:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"

The response is a JSON document that begins with information similar to the following:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Since this is JSON, it is easier to use a JSON parser to work with the data. For example, the following example uses jq to display only the `health_report` element.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

## Example: Get the FQDN of cluster nodes

When working with HDInsight, you may need to know the fully qualified domain name (FQDN) of a cluster node. You can easily retrieve the FQDN for the various nodes in the cluster using the following:

* **Head nodes**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`

* **Worker nodes**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`

* **Zookeeper nodes**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Note that each of these examples follow the same pattern:

1. Query a component that we know runs on those nodes.

2. Retrieve the `host_name` elements, which contain the FQDN for these nodes.

The `host_components` element of the return document contains multiple items. Using `.host_components[]`, and then specifying a path within the element will loop through each item and pull out the value from the specific path. If you only want one value, such as the first FQDN entry, you can return the items as a collection and then select a specific entry:

    jq '[.host_components[].HostRoles.host_name][0]'

This returns the first FQDN from the collection.

## Example: Get the default storage account and container

When you create an HDInsight cluster, you must use an Azure Storage Account and a blob container as the default storage for the cluster. You can use Ambari to retrieve this information after the cluster has been created. For example, if you want to programmatically write data directly to the container.

The following will retrieve the WASB URI of the clusters default storage:

```bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

PowerShell has slightly different rules for using single and double quotes. Use the following command from PowerShell:

```PowerShell
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["""fs.defaultFS"""] | select(. != null)'
```

> [!NOTE]
> This returns the first configuration applied to the server (`service_config_version=1`,) which contains this information. If you retrieve a value that has been modified after cluster creation, you may need to list the configuration versions and retrieve the latest one.

This returns a value similar to the following example, where **CONTAINER** is the default container and **ACCOUNTNAME** is the Azure Storage Account name:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

You can then use this information with the [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) to upload or download data from the container.

1. Use the following command to get the unique ID for the Storage Account. In the following command, replace **ACCOUNTNAME** with the Storage Account name retrieved from Ambari:

        az storage account list --query "[?name=='ACCOUNTNAME'].id" --out list

2. Use the following to get a key for the storage account. Replace **STORAGEID** with the storage account ID:

        az storage account keys list --ids STORAGEID --query "keys[?keyName=='key1'].value" --out list

    This will return the primary key for the account.

3. Use the upload command to store a file in the container:
   
        az storage blob upload --account-name ACOUNTNAME --account-key ACCOUNTKEY -f FILEPATH -c CONTAINER -n BLOBPATH
   
    Replace **ACCOUNTNAME** with the Storage Account name. Replace **ACCOUNTKEY** with the key retrieved previously. Replace __CONTAINER__ with the container name. **FILEPATH** is the path to the file you want to upload, while **BLOBPATH** is the path in the container.
   
    For example, if you want the file to appear in HDInsight at wasbs://example/data/filename.txt, then **BLOBPATH** would be `example/data/filename.txt`.

## Example: Update Ambari configuration

1. Get the current configuration, which Ambari stores as the "desired configuration":
   
        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
   
    This example returns a JSON document containing the current configuration (identified by the *tag* value) for the components installed on the cluster. The following example is an excerpt from the data returned from a Spark cluster type.
   
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }
   
    From this list, you need to copy the name of the component (for example, **spark\_thrift\_sparkconf** and the **tag** value.

2. Retrieve the configuration for the component and tag by using the following command. Replace **spark-thrift-sparkconf** and **INITIAL** with the component and tag that you want to retrieve the configuration for.
   
    ```bash
    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```PowerShell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```
   
    Curl retrieves the JSON document, then jq is used to make modifications to the data in order to create a template. The template is then used to add/modify configuration values. Specifically it does the following:
   
    * Creates a unique value containing the string "version" and the date, which is stored in **newtag**.

    * Creates a root document for the new desired configuration.

    * Gets the contents of the `.items[]` array and adds it under the **desired_config** element.

    * Deletes the **href**, **version**, and **Config** elements, as these elements aren't needed to submit a new configuration.

    * Adds a new **tag** element and sets its value to **version#################**. The numeric portion is based on the current date. Each configuration must have a unique tag.
     
        Finally, the data is saved to the **newconfig.json** document. The document structure should appear similar to the following example:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Open the **newconfig.json** document and modify/add values in the **properties** object. The following example changes the value of **"spark.yarn.am.memory"** from **"1g"** to **"3g"** and, and adds a new element for **"spark.kryoserializer.buffer.max"** with a value of **"256m"**.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Save the file once you are done making modifications.
4. Use the following command to submit the updated configuration to Ambari.
   
        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
   
    This command pipes the contents of the **newconfig.json** file to the curl request, which submits it to the cluster as the new desired configuration. The cURL request returns a JSON document. The **versionTag** element in this document should match the version you submitted, and the **configs** object will contain the configuration changes you requested.

### Example: Restart a service component

At this point, if you look at the Ambari web UI, the Spark service will indicate that it needs to be restarted before the new configuration can take effect. Use the following steps to restart the service.

1. Use the following to enable maintenance mode for the Spark service:
   
        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
   
    This command sends a JSON document to the server (contained in the `echo` statement,) which turns maintenance mode on.
    You can verify that the service is now in maintenance mode using the following request:
   
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
   
    This will return a value of `"ON"`.

2. Next, use the following to turn the service off:
    
    ```
    echo '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
    ```
    
    This command returns a response similar to the following.
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    The `href` value returned by this URI is using the internal IP address of the cluster node. To use it from outside the cluster, replace the `10.0.0.18:8080' portion with the FQDN of the cluster. For example, the following command retrieves the status of the request.
    
    ```
    curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    ```
    
    If this returns a value of `"COMPLETED"` then the request has finished.

3. Once the previous request completes, use the following to start the service.
   
        echo '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
   
    Once the service restarts, it is the new configuration settings.

4. Finally, use the following to turn off maintenance mode.
   
        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

## Next steps

For a complete reference of the REST API, see [Ambari API Reference V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

