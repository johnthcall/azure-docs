---
title: Manage Azure DC/OS cluster with Marathon UI | Microsoft Docs
description: Deploy containers to an Azure Container Service cluster service by using the Marathon web UI.
services: container-service
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure

ms.assetid: d148ed1e-b582-4d51-944f-1ac7ae3c4fd6
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2016
ms.author: danlep

---
# Manage an Azure Container Service DC/OS cluster through the Marathon web UI
DC/OS provides an environment for deploying and scaling clustered workloads, while abstracting the underlying hardware. On top of DC/OS, there is a framework that manages scheduling and executing compute workloads.

While frameworks are available for many popular workloads, this document describes how you can create and scale container deployments with Marathon. 


## Prerequisites
Before working through these examples, you need a DC/OS cluster that is configured in Azure Container Service. You also need to have remote connectivity to this cluster. For more information on these items, see the following articles:

* [Deploy an Azure Container Service cluster](container-service-deployment.md)
* [Connect to an Azure Container Service cluster](container-service-connect.md)

## Explore the DC/OS UI
With a Secure Shell (SSH) tunnel [established](container-service-connect.md), browse to http://localhost/. This loads the DC/OS web UI and shows information about the cluster, such as used resources, active agents, and running services.

![DC/OS UI](./media/dcos/dcos2.png)

## Explore the Marathon UI
To see the Marathon UI, browse to http://localhost/Marathon. From this screen, you can start a new container or another application on the Azure Container Service DC/OS cluster. You can also see information about running containers and applications.  

![Marathon UI](./media/dcos/dcos3.png)

## Deploy a Docker-formatted container
To deploy a new container by using Marathon, click **Create Application**, and enter the following information into the form tabs:

| Field | Value |
| --- | --- |
| ID |nginx |
| Memory | 32 |
| Image |nginx |
| Network |Bridged |
| Host Port |80 |
| Protocol |TCP |

![New Application UI--General](./media/dcos/dcos4.png)

![New Application UI--Docker Container](./media/dcos/dcos5.png)

![New Application UI--Ports and Service Discovery](./media/dcos/dcos6.png)

If you want to statically map the container port to a port on the agent, you need to use JSON Mode. To do so, switch the New Application wizard to **JSON Mode** by using the toggle. Then enter the following setting under the `portMappings` section of the application definition. This example binds port 80 of the container to port 80 of the DC/OS agent. You can switch this wizard out of JSON Mode after you make this change.

```none
"hostPort": 80,
```

![New Application UI--port 80 example](./media/dcos/dcos13.png)

If you want to enable health checks, set a path on the **Health Checks** tab.

![New Application UI--health checks](./media/dcos/dcos_healthcheck.png)

The DC/OS cluster is deployed with set of private and public agents. For the cluster to be able to access applications from the Internet, you need to deploy the applications to a public agent. To do so, select the **Optional** tab of the New Application wizard and enter **slave_public** for the **Accepted Resource Roles**.

Then click **Create Application**.

![New Application UI--public agent setting](./media/dcos/dcos14.png)

Back on the Marathon main page, you can see the deployment status for the container. Initially you see a status of **Deploying**. After a successful deployment, the status changes to **Running**.

![Marathon main page UI--container deployment status](./media/dcos/dcos7.png)

When you switch back to the DC/OS web UI (http://localhost/), you see that a task (in this case, a Docker-formatted container) is running on the DC/OS cluster.

![DC/OS web UI--task running on the cluster](./media/dcos/dcos8.png)

To see the cluster node that the task is running on, click the **Nodes** tab.

![DC/OS web UI--task cluster node](./media/dcos/dcos9.png)

## Scale your containers
You can use the Marathon UI to scale the instance count of a container. To do so, navigate to the **Marathon** page, select the container that you want to scale, and click **Scale Application**. In the **Scale Application** dialog box, enter the number of container instances that you want, and click **Scale Application**.

![Marathon UI--Scale Application dialog box](./media/dcos/dcos10.png)

After the scale operation finishes, you will see multiple instances of the same task spread across DC/OS agents.

![DC/OS web UI dashboard--task spread across agents](./media/dcos/dcos11.png)

![DC/OS web UI--nodes](./media/dcos/dcos12.png)

## Next steps
* [Work with DC/OS and the Marathon API](container-service-mesos-marathon-rest.md)

* Deep dive on the Azure Container Service with Mesos

    > [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON203/player]
    > 
