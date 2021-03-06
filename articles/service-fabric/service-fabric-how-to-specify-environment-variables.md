---
title: 指定服务的环境变量
description: 演示如何在 Service Fabric 中使用应用程序的环境变量
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f4c4f2a1c140e3d0f181c4fd55482056f9f91b62
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614309"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>如何在 Service Fabric 中指定服务的环境变量

本文演示如何在 Service Fabric 中指定服务或容器的环境变量。

## <a name="procedure-for-specifying-environment-variables-for-services"></a>指定服务的环境变量的过程

在此示例中，可以为容器设置环境变量。 本文假设你已有一个应用程序和服务清单。

1. 打开 ServiceManifest.xml 文件。
2. 在 `CodePackage` 元素中，为每个环境变量添加新的 `EnvironmentVariables` 元素和 `EnvironmentVariable` 元素。

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   可在应用程序清单中重写环境变量。

3. 若要替代应用程序清单中的环境变量，请使用 `EnvironmentOverrides` 元素。

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>使用 Docker Compose 动态指定环境变量

Service Fabric 支持[使用 Docker Compose 进行部署](service-fabric-docker-compose.md#supported-compose-directives)的功能。 编写文件可以来自 shell 的源环境变量。 此行为可用于动态替换所需的环境值：

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>后续步骤
若要详细了解本文中讨论的一些核心概念，请参阅文章[管理多个环境的应用程序](service-fabric-manage-multiple-environment-app-configuration.md)。

有关 Visual Studio 中其他可用应用管理功能的信息，请参阅[在 Visual Studio 中管理 Service Fabric 应用程序](service-fabric-manage-application-in-visual-studio.md)。
