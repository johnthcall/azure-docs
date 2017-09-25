---
title: Get started with Apache Spark cluster in Azure HDInsight | Microsoft Docs
description: Step-by-step instructions on how to quickly create an Apache Spark cluster in HDInsight and then use Spark SQL from Jupyter notebooks to run interactive queries.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal

ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/01/2017
ms.author: nitinme

---
# Get started: Create Apache Spark cluster in Azure HDInsight and run interactive queries using Spark SQL

Learn how to create an [Apache Spark](hdinsight-apache-spark-overview.md) cluster in HDInsight and then use [Jupyter](https://jupyter.org) notebook to run Spark SQL interactive queries on the Spark cluster.

   ![Get started using Apache Spark in HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; create a cluster; run Spark SQL statements")

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Prerequisites
* **An Azure subscription**. Before you begin this tutorial, you must have an Azure subscription. See [Create your free Azure account today](https://azure.microsoft.com/free).

* **A Secure Shell (SSH) client**: Linux, Unix, and OS X systems provied an SSH client through the `ssh` command. For Windows clients, see [Use SSH with Hadoop on HDInsight from Windows with PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md); for Linux, Unix or OS X, see [Use SSH with Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

> [!NOTE]
> This article uses an Azure Resource Manager template to create a Spark cluster that uses [Azure Storage Blobs as the cluster storage](hdinsight-hadoop-use-blob-storage.md). You can also create a Spark cluster that uses [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) as an additional storage, in addition to Azure Storage Blobs as the default storage. For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

### Access control requirements
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## Create Spark cluster
In this section, you create a Spark cluster in HDInsight using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). For information about HDInsight versions and their SLAs, see [HDInsight component versioning](hdinsight-component-versioning.md). For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).

1. Click the following image to open the template in the Azure portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Enter the following values:

    ![Create Spark cluster in HDInsight using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")

   * **Subscription**: Select your Azure subscription for this cluster.
   * **Resource group**: Create a new resource group or select an existing one. Resource group is used to manage Azure resources for your projects.
   * **Location**: Select a location for the resource group.  This location is also used for the default cluster storage and the HDInsight cluster.
   * **ClusterName**: Enter a name for the Hadoop cluster that you will create.
   * **Cluster login name and password**: The default login name is admin.
   * **SSH user name and password**.

   Please write down these values.  You will need them later in the tutorial.

3. Select **I agree to the terms and conditions stated above**, select **Pin to dashboard**, and then click **Purchase**. You can see a new tile titled Submitting deployment for Template deployment. It takes about around 20 minutes to create the cluster.

## Run Spark SQL queries using a Jupyter notebook
In this section, you use Jupyter notebook to run Spark SQL queries against the Spark cluster. HDInsight Spark clusters provide two kernels that you can use with the Jupyter notebook. These are:

* **PySpark** (for applications written in Python)
* **Spark** (for applications written in Scala)

In this article, you use the PySpark kernel. For more information about the two kernels, see [Use Jupyter notebooks kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md). Some of the key benefits of using the PySpark kernel are:

* The contexts for Spark and Hive are set automatically.
* Use cell magics, such as `%%sql`, to directly run SQL or Hive queries, without any preceding code snippets.
* The output for the SQL or Hive queries is automatically visualized.

### Create Jupyter notebook with PySpark kernel

1. Open the [Azure portal](https://portal.azure.com/).
2. From the left menu, click **Resource groups**.
3. Click the resource group you created in the last section. You can use the search function if there are too many resource groups. You can see two resources in the group, the HDInsight cluster, and the default storage account.
4. Click the cluster to open it.
 
2. From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**. If prompted, enter the admin credentials for the cluster.

   ![HDInsight cluster dashboards](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-azure-portal-cluster-dashboards.png "HDInsight cluster dashboards")

   > [!NOTE]
   > You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser. Replace **CLUSTERNAME** with the name of your cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Create a new notebook. Click **New**, and then click **PySpark**.

   ![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")

   A new notebook is created and opened with the name Untitled(Untitled.pynb). 

4. Click the notebook name at the top, and enter a friendly name if you want.

    ![Provide a name for the notebook](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Provide a name for the notebook")
5. Paste the following code in an empty cell, and then press **SHIFT + ENTER** to execute the code. The code imports the types required for this scenario:

        from pyspark.sql.types import *

    Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly. The Spark and Hive contexts will be automatically created for you when you run the first code cell.

    ![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status of a Jupyter notebook job")

    Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with the notebook title. You will also see a solid circle next to the **PySpark** text in the top-right corner. After the job is completed, this will change to a hollow circle.

6. Run the following code to register some sample data into a temporary table called **hvac**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

    Spark clusters in HDInsight come with a sample data file, **hvac.csv**, under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.
    
7. Run the following code to query the data:

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Because you are using a PySpark kernel, you can now directly run a SQL query on the temporary table **hvac** that you just created by using the `%%sql` magic. For more information about the `%%sql` magic, as well as other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-pyspark-or-spark-kernels).

   The following tabular output is displayed by default.

     ![Table output of query result](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Table output of query result")

    You can also see the results in other visualizations as well. For example, an area graph for the same output would look like the following.

    ![Area graph of query result](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Area graph of query result")

9. After you have finished running the application, you can shutdown the notebook to release the resources. To do so, from the **File** menu on the notebook, click **Close and Halt**. This will shutdown and close the notebook.

## Delete the cluster
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## See also
* [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md)

### Scenarios
* [Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Use Spark in HDInsight for building real-time streaming applications](hdinsight-apache-spark-eventhub-streaming.md)
* [Website log analysis using Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight telemetry data analysis using Spark in HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### Create and run applications
* [Create a standalone application using Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Run jobs remotely on a Spark cluster using Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Tools and extensions
* [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use Zeppelin notebooks with a Spark cluster on HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Kernels available for Jupyter notebook in Spark cluster for HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Use external packages with Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Install Jupyter on your computer and connect to an HDInsight Spark cluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Manage resources
* [Manage resources for the Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
