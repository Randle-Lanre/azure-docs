---
title: App status in Azure Spring Cloud
description: Learn the app status categories in Azure Spring Cloud
author: karlerickson
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 03/30/2022
ms.author: karler
ms.custom: devx-track-java
---

# App status in Azure Spring Cloud

**This article applies to:** ✔️ Java ✔️ C#

**This article applies to:** ✔️ Basic/Standard tier ✔️ Enterprise tier

This article shows you how to view app status for Azure Spring Cloud.

The Azure Spring Cloud UI delivers information about the status of running applications. There's an **Apps** option for each resource group in a subscription that displays general status of application types. For each application type, there's a display of **Application instances**.

## Apps status

To view general status of an application type, select **Apps** in the left navigation pane of a resource group. The result displays the status of the deployed app:

* **Provisioning Status**: Shows the deployment’s provisioning state.
* **Running instance**: Shows how many app instances are running and how many app instances you desire. If you stop the app, this column shows **stopped**.
* **Registered Instance**: Shows how many app instances are registered to Eureka and how many app instances you desire. If you stop the app, this column shows **stopped**. Eureka isn't applicable to enterprise tier. For more information if you're using the enterprise tier, see [Use Service Registry](how-to-enterprise-service-registry.md).

![Apps status](media/spring-cloud-concept-app-status/apps-ui-status.png)

## Deployment status

The deployment status shows the running state of the deployment. The status is reported as one of the following values:

| Value   | Definition                        |
|---------|-----------------------------------|
| Running | The deployment SHOULD be running. |
| Stopped | The deployment SHOULD be stopped. |

## Provisioning status

The *deployment provisioning* status describes the state of operations of the deployment resource. This status shows the comparison between the functionality and the deployment definition.

The provisioning state is accessible only from the CLI.  It's reported as one of the following values:

| Value     | Definition                                              |
|-----------|---------------------------------------------------------|
| Creating  | The resource is creating and isn't ready.              |
| Updating  | The resource is updating and the functionality may be different from the deployment definition until the update is complete.                               |
| Succeeded | Successfully supplied resources and deploys the binary. The deployment's functionality is the same as the definition and all app instances are working. |
| Failed    | Failed to achieve the *Succeeded* goal.                 |
| Deleting  | The resource is being deleted which prevents operation, and the resource isn't available in this status. |

## App instances status

The *app instance* status represents every instance of the app. To view the status of a specific instance of a deployed app, select the **Name** of the app in the **Apps** UI. The results will display:

* **Status**: Whether the instance is running or its current state
* **DiscoveryStatus**: The registered status of the app instance in the Eureka server

![App instances status](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

### App instance status

The instance status is reported as one of the following values:

| Value       | Definition |
|-------------|------------|
| Starting    | The binary is successfully deployed to the given instance. The instance booting the jar file may fail because the jar can't run properly. Azure Spring Cloud will restart the app instance in 60 seconds if it detects that the app instance is still in the *Starting* state. |
| Running     | The instance works. The instance can serve requests from inside Azure Spring Cloud. |
| Failed      | The app instance failed to start the user’s binary after several retries. The app instance may be in one of the following states:<br/>- The app may stay in the *Starting* status and never be ready for serving requests.<br/>- The app may boot up but crashed in a few seconds. |
| Terminating | The app instance is shutting down. The app may not serve requests and the app instance will be removed. |

### App discovery status

The discovery status of the instance is reported as one of the following values:

| Value          | Definition |
|----------------|------------|
| UP             | The app instance is registered to Eureka and ready to receive traffic |
| OUT_OF_SERVICE | The app instance is registered to Eureka and able to receive traffic. but shuts down for traffic intentionally. |
| DOWN           | The app instance is registered but not able to receive traffic. |
| UNREGISTERED   | The app instance isn't registered to Eureka. |
| N/A            | The app instance is running with custom container or service discovery is not enabled. |

## App registration status

The *app registration* status shows the state in service discovery. Azure Spring Cloud uses Eureka for service discovery. For more information on how the Eureka client calculates the state, see [Eureka's health checks](https://cloud.spring.io/spring-cloud-static/Greenwich.RELEASE/multi/multi__service_discovery_eureka_clients.html#_eureka_s_health_checks).
## Next steps

* [Prepare a Spring or Steeltoe application for deployment in Azure Spring Cloud](how-to-prepare-app-deployment.md)
