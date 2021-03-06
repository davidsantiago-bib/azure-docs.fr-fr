---
title: Méthodes d’authentification pour Azure Security Center pour la version préliminaire IoT | Microsoft Docs
description: Découvrez les différentes méthodes d’authentification disponibles lors de l’utilisation d’Azure Security Center pour le service IoT.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 10b38f20-b755-48cc-8a88-69828c17a108
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: ec87c2b65728d8ac29daa90de36271e24cd85c0e
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758393"
---
# <a name="security-agent-authentication-methods"></a>Méthodes d’authentification de l’agent de sécurité 

> [!IMPORTANT]
> Azure Security Center pour IoT est actuellement en version préliminaire publique.
> Cette version préliminaire est fournie sans contrat de niveau de service et n’est pas recommandée pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge. Pour plus d’informations, consultez [Conditions d’Utilisation Supplémentaires relatives aux Évaluations Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cet article explique les différentes méthodes d’authentification que vous pouvez utiliser avec l’agent AzureIoTSecurity pour s’authentifier auprès du IoT Hub.

Pour chaque périphérique intégré à Azure Security Center (ASC) pour IoT dans IoT Hub, un module de sécurité est nécessaire. Pour authentifier l’appareil, ASC pour IoT permettre utiliser une des deux méthodes. Choisissez la méthode qui convient le mieux à votre solution IoT existante. 

> [!div class="checklist"]
> * Option de Module de sécurité
> * Option de l’appareil

## <a name="authentication-methods"></a>Méthodes d’authentification

Les deux méthodes pour l’agent AzureIoTSecurity effectuer l’authentification :

 - **Module** mode d’authentification<br>
   Le Module est authentifié indépendamment de la représentation d’appareil.
   Utilisez ce type d’authentification si vous souhaitez que l’agent de sécurité à utiliser une méthode d’authentification dédié via le module de sécurité (clé symétrique uniquement).
        
 - **APPAREIL** mode d’authentification<br>
    Dans cette méthode, l’agent de sécurité authentifie tout d’abord avec l’identité d’appareil. Après l’authentification initiale, l’ASC pour l’agent IoT effectue un **REST** appeler à IoT Hub à l’aide de l’API REST avec les données d’authentification de l’appareil. L’ASC pour l’agent IoT demande ensuite la méthode d’authentification de module de sécurité et les données à partir du IoT Hub. Dans la dernière étape, l’ASC pour l’agent IoT effectue une authentification par rapport à l’ASC pour le module IoT.
    
    Utilisez ce type d’authentification si vous souhaitez que l’agent de sécurité pour réutiliser une méthode d’authentification appareil existant (auto-signé certificat ou clé symétrique). 

Consultez [paramètres d’installation de l’agent de sécurité](#security-agent-installation-parameters) pour apprendre à configurer.
                                
## <a name="authentication-methods-known-limitations"></a>Limitations connues des méthodes d’authentification

- **Module** mode d’authentification prend uniquement en charge l’authentification par clé symétrique.
- Certificat d’autorité de certification n’est pas pris en charge par **appareil** mode d’authentification.  

## <a name="security-agent-installation-parameters"></a>Paramètres d’installation de l’agent de sécurité

Lorsque [déploiement d’un agent de sécurité](how-to-deploy-agent.md), détails de l’authentification doivent être fournis en tant qu’arguments.
Ces arguments sont documentées dans le tableau suivant.


|Paramètre|Description|Options|
|---------|---------------|---------------|
|**identity**|mode d'authentification| **Module** ou **appareil**|
|**type**|Type d'authentification|**SymmetricKey** ou **SelfSignedCertificate**|
|**filePath**|Chemin d’accès complet absolu du fichier contenant le certificat ou la clé symétrique| |
|**gatewayHostname**|Nom de domaine complet de l’IoT Hub|Exemple : ContosoIotHub.azure-devices.net|
|**deviceId**|ID de périphérique|Exemple : MyDevice1|
|**certificateLocationKind**|Emplacement de stockage de certificat|**LocalFile** ou **Store**|


Lorsque vous utilisez le script de l’agent de sécurité installation, la configuration suivante est effectuée automatiquement.

Pour modifier manuellement l’authentification de l’agent de sécurité, modifiez le fichier de configuration. 

## <a name="change-authentication-method-after-deployment"></a>Modifier la méthode d’authentification après le déploiement

Lorsque vous déployez un agent de sécurité avec un script d’installation, un fichier de configuration est automatiquement créé.

Pour modifier les méthodes d’authentification après le déploiement, la modification manuelle du fichier de configuration est requise.


### <a name="c-based-security-agent"></a>C#-en fonction de l’agent de sécurité

Modifier _Authentication.config_ avec les paramètres suivants :

```xml
<Authentication>
  <add key="deviceId" value=""/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value=""/>
  <add key="type" value=""/>
  <add key="identity" value=""/>
  <add key="certificateLocationKind" value="" />
</Authentication>
```

### <a name="c-based-security-agent"></a>Agent de sécurité basée sur C

Modifier _LocalConfiguration.json_ avec les paramètres suivants :

```json
"Authentication" : {
    "Identity" : "",
    "AuthenticationMethod" : "",
    "FilePath" : "",
    "DeviceId" : "",
    "HostName" : ""
}
```

## <a name="see-also"></a>Voir aussi
- [Vue d’ensemble des agents de sécurité](security-agent-architecture.md)
- [Déploiement de l’agent de sécurité](how-to-deploy-agent.md)
- [Accéder aux données de sécurité brute](how-to-security-data-access.md)
