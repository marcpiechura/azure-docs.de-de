---
title: Verwenden verwalteter Identitäten in Azure Kubernetes Service
description: Erfahren Sie, wie Sie verwaltete Identitäten in Azure Kubernetes Service (AKS) verwenden.
services: container-service
author: saudas
manager: saudas
ms.topic: article
ms.date: 04/02/2020
ms.author: saudas
ms.openlocfilehash: 00ecc077ba55ab9f91fc58f8a47fcdf7440deea6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82112965"
---
# <a name="use-managed-identities-in-azure-kubernetes-service"></a>Verwenden verwalteter Identitäten in Azure Kubernetes Service

Derzeit erfordert ein AKS-Cluster (Azure Kubernetes Service) – genauer gesagt, der Kubernetes-Cloudanbieter – eine Identität, um zusätzliche Ressourcen wie Lastenausgleichsmodule und verwaltete Datenträger in Azure zu erstellen. Diese Identität kann eine *verwaltete Identität* oder ein *Dienstprinzipal* sein. Wenn Sie einen [Dienstprinzipal](kubernetes-service-principal.md) verwenden, müssen Sie diesen bereitstellen oder in Ihrem Namen durch AKS erstellen lassen. Wenn Sie eine verwaltete Identität verwenden, wird diese automatisch von AKS erstellt. Cluster mit Dienstprinzipalen erreichen irgendwann einen Zustand, in dem der Dienstprinzipal erneuert werden muss, damit der Cluster ordnungsgemäß funktioniert. Das Verwalten von Dienstprinzipalen erhöht die Komplexität, daher ist es einfacher, stattdessen verwaltete Identitäten zu verwenden. Dieselben Berechtigungsanforderungen gelten sowohl für Dienstprinzipale als auch für verwaltete Identitäten.

*Verwaltete Identitäten* stellen im Wesentlichen einen Wrapper um Dienstprinzipale dar und vereinfachen deren Verwaltung. Weitere Informationen finden Sie im Artikel über [verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS erstellt zwei verwaltete Identitäten:

- **Systemseitig zugewiesene verwaltete Identität**: die Identität, die der Kubernetes-Cloudanbieter zum Erstellen von Azure-Ressourcen im Auftrag des Benutzers verwendet. Der Lebenszyklus der systemseitig zugewiesenen Identität ist an den des Clusters gebunden. Die Identität wird gelöscht, wenn der Cluster gelöscht wird.
- **Benutzerseitig zugewiesene verwaltete Identität**: die Identität, die für die Autorisierung im Cluster verwendet wird. Die benutzerseitig zugewiesene Identität wird beispielsweise verwendet, um AKS zum Verwenden von Azure Container Registrys (ACRs) oder das Kubelet zum Abrufen von Metadaten aus Azure zu autorisieren.

Add-Ons authentifizieren auch über eine verwaltete Identität. Für jedes Add-On wird eine verwaltete Identität von AKS erstellt und für die Lebensdauer des Add-Ons beibehalten. 

## <a name="before-you-begin"></a>Voraussetzungen

Die folgenden Ressourcen müssen installiert sein:

- Die Azure CLI, Version 2.2.0 oder höher.

## <a name="create-an-aks-cluster-with-managed-identities"></a>Erstellen eines AKS-Clusters mit verwalteten Identitäten

Sie können jetzt einen AKS-Cluster mit verwalteten Identitäten erstellen, indem Sie die folgenden CLI-Befehle verwenden.

Erstellen Sie als Erstes eine Azure-Ressourcengruppe:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Erstellen Sie anschließend einen AKS-Cluster:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

Bei einer erfolgreichen Clustererstellung mit verwalteten Identitäten sind folgende Dienstprinzipal-Profilinformationen enthalten:

```json
"servicePrincipalProfile": {
    "clientId": "msi",
    "secret": null
  }
```

> [!NOTE]
> Verwenden Sie zum Erstellen und Verwenden Ihres eigenen VNET, Ihrer eigenen statischen IP-Adresse oder Ihres eigenen angeschlossenen Azure-Datenträgers, auf dem sich die Ressourcen außerhalb der Ressourcengruppe „MC_*“ befinden, die Prinzipal-ID der systemseitig zugewiesenen verwalteten Identität des Clusters, um eine Rollenzuweisung auszuführen. Weitere Informationen zur Rollenzuweisung finden Sie unter [Delegieren des Zugriffs auf andere Azure-Ressourcen](kubernetes-service-principal.md#delegate-access-to-other-azure-resources).
>
> Es kann bis zu 60 Minuten dauern, bis die vom Azure-Cloudanbieter verwendeten Berechtigungszuweisungen für die Erstellung von Clustern für verwaltete Identitäten übernommen werden.

Rufen Sie schließlich Anmeldeinformationen für den Zugriff auf den Cluster ab:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

Der Cluster wird in wenigen Minuten erstellt. Sie können Ihre Anwendungsworkloads im neuen Cluster bereitstellen und damit interagieren, wie Sie dies mit dienstprinzipalbasierten AKS-Clustern getan haben.

> [!IMPORTANT]
>
> - AKS-Cluster mit verwalteten Identitäten können nur während der Erstellung des Clusters aktiviert werden.
> - Es ist nicht möglich, AKS-Cluster zu aktualisieren oder ein Upgrade für sie durchzuführen, um verwaltete Identitäten zu aktivieren.
