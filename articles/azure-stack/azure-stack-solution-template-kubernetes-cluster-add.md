---
title: Ajouter Kubernetes sur la Place de marché Azure Stack | Microsoft Docs
description: Découvrez comment ajouter Kubernetes sur la Place de marché Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: ca58059716ebebfaf663412b37014ae4f534d0e3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58081506"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Ajouter Kubernetes sur la Place de marché Azure Stack

*S’applique à : systèmes intégrés Azure Stack et Kit de développement Azure Stack*

> [!note]  
> Kubernetes sur Azure Stack est en préversion. Le scénario Azure Stack déconnecté n’est actuellement pas pris en charge par la préversion.

Vous pouvez créer une offre Kubernetes et la mettre à disposition des utilisateurs en tant qu’élément de la Place de marché. Vos utilisateurs peuvent alors déployer Kubernetes en une seule opération coordonnée.

L’article suivant décrit comment utiliser un modèle Azure Resource Manager pour déployer et provisionner les ressources d’un cluster Kubernetes autonome. Avant de commencer, vérifiez les paramètres Azure Stack et les paramètres du locataire Azure global. Collectez les informations sur Azure Stack requises. Ajoutez les ressources nécessaires à votre locataire et à la Place de marché Azure Stack. Le cluster dépend d’un serveur Ubuntu, du script personnalisé et un élément Place de marché du cluster Kubernetes à ajouter à la Place de marché.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Créer un plan, une offre et un abonnement

Créez un plan, une offre et un abonnement pour l’élément Kubernetes de la Place de marché. Vous pouvez également utiliser un plan et une offre existants.

1. Connectez-vous au [portail d’administration](https://adminportal.local.azurestack.external).

1. Créez un plan de base. Pour obtenir des instructions, consultez [Créer un plan dans Azure Stack](azure-stack-create-plan.md).

1. Créez une offre. Pour obtenir des instructions, consultez [Créer une offre dans Azure Stack](azure-stack-create-offer.md).

1. Sélectionnez **Offres**et recherchez l’offre que vous venez de créer.

1. Sélectionnez **Vue d’ensemble** dans le panneau Offre.

1. Sélectionnez **Changer l’état**. Sélectionnez **Public**.

1. Sélectionnez **+ Créer une ressource** > **Offres et plans** > **Abonnement** pour créer un abonnement.

    a. Entrez un **nom d’affichage**.

    b. Entrez un **utilisateur**. Utilisez le compte Azure AD associé au locataire.

    c. Entrez une **description du fournisseur**

    d. Définissez le **locataire d’annuaire** sur le locataire Azure AD dans votre environnement Azure Stack. 

    e. Sélectionnez **Offre**. Sélectionnez le nom de l’offre que vous avez créée. Prenez note de l’ID d’abonnement.

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>Créer un principal de service et des informations d’identification dans AD FS

Si vous utilisez AD FS (Active Directory Federated Services) pour votre service de gestion des identités, vous devez créer un principal de service pour les utilisateurs qui déploient un cluster Kubernetes.

1. Créez et exportez un certificat autosigné utilisé pour créer le principal de service. 

    - Vous avez besoin des informations suivantes :

       | Valeur | Description |
       | ---   | ---         |
       | Mot de passe | Entrez un nouveau mot de passe pour le certificat. |
       | Chemin du certificat local | Entrez le chemin et le nom de fichier du certificat. Par exemple : `c:\certfilename.pfx` |
       | Nom du certificat | Entrez le nom du certificat. |
       | Emplacement du magasin de certificats |  Par exemple, `Cert:\LocalMachine\My` |

    - Ouvrez PowerShell avec une invite de commandes avec élévation de privilèges. Exécutez le script suivant, en utilisant vos valeurs pour les paramètres :

        ```PowerShell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "CN=<certificate name>"
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2.  Notez le nouvel ID de certificat qui s'affiche dans votre session PowerShell, `1C2ED76081405F14747DC3B5F76BB1D83227D824`. L'ID sera utilisé lors de la création du principal de service.

    ```PowerShell  
    VERBOSE: Generated new certificate 'CN=<certificate name>' (1C2ED76081405F14747DC3B5F76BB1D83227D824).
    ```

3. Créez un principal de service à l’aide du certificat.

    - Vous avez besoin des informations suivantes :

       | Valeur | Description                     |
       | ---   | ---                             |
       | Adresse IP ERCS | Dans le Kit de développement Azure Stack (ASDK), le point de terminaison privilégié est normalement `AzS-ERCS01`. |
       | Nom de l’application | Entrez un nom simple pour le principal de service d’application. |
       | Emplacement du magasin de certificats | Chemin sur votre ordinateur où vous avez stocké le certificat. Il est indiqué par l'emplacement du magasin et l’ID de certificat généré à la première étape. Par exemple : `Cert:\LocalMachine\My\1C2ED76081405F14747DC3B5F76BB1D83227D824` |

       Lorsque vous y êtes invité, utilisez les informations d’identification suivantes pour vous connecter au point de terminaison privilégié. 
        - Nom d’utilisateur : spécifiez le compte CloudAdmin, au format <Azure Stack domain>\cloudadmin. (Pour le Kit ASDK, le nom d’utilisateur est azurestack\cloudadmin.)
        - Mot de passe : entrez le mot de passe du compte d'administrateur de domaine AzureStackAdmin tel qu'il vous a été fourni pendant l'installation.

    - Exécutez le script suivant, en utilisant vos valeurs pour les paramètres :

        ```PowerShell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        $certStoreLocation="<certificate location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - Les détails du principal de service se présentent comme dans l’extrait ci-dessous.

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>Ajouter une image de serveur Ubuntu

Ajoutez l’image de serveur Ubuntu suivante sur la Place de marché :

1. Connectez-vous au [portail d’administration](https://adminportal.local.azurestack.external).

1. Sélectionnez **Tous les services**, puis, dans la catégorie **ADMINISTRATION**, sélectionnez **Gestion de la Place de marché**.

1. Sélectionnez **+ Ajouter à partir d’Azure**.

1. Entrez `Ubuntu Server`.

1. Sélectionnez la version la plus récente du serveur. Consultez la version complète et assurez-vous que vous avez la version la plus récente :
    - **Éditeur** : Canonical
    - **Offre** : UbuntuServer
    - **Version** : 16.04.201806120 (ou la dernière version)
    - **SKU** : 16.04-LTS

1. Sélectionnez **Télécharger**.

## <a name="add-a-custom-script-for-linux"></a>Ajouter un script personnalisé pour Linux

Ajoutez Kubernetes à partir de la Place de marché :

1. Ouvrez le [portail d’administration](https://adminportal.local.azurestack.external).

1. Sélectionnez **Tous les services**, puis, dans la catégorie **ADMINISTRATION**, sélectionnez **Gestion de la Place de marché**.

1. Sélectionnez **+ Ajouter à partir d’Azure**.

1. Entrez `Custom Script for Linux`.

1. Sélectionnez le script avec le profil suivant :
   - **Offre** : Script personnalisé pour Linux 2.0
   - **Version** : 2.0.6 (ou la dernière version)
   - **Éditeur** : Microsoft Corp

     > [!Note]  
     > Plusieurs versions d’un script personnalisé pour Linux peuvent être répertoriées. Vous devez ajouter la dernière version de l’élément.

1. Sélectionnez **Télécharger**.


## <a name="add-kubernetes-to-the-marketplace"></a>Ajouter Kubernetes sur la Place de marché

1. Ouvrez le [portail d’administration](https://adminportal.local.azurestack.external).

1. Sélectionnez **Tous les services** et, sous la catégorie **ADMINISTRATION**, sélectionnez **Gestion de la Place de marché**.

1. Sélectionnez **+ Ajouter à partir d’Azure**.

1. Entrez `Kubernetes`.

1. Sélectionnez `Kubernetes Cluster`.

1. Sélectionnez **Télécharger**.

    > [!note]  
    > L’affichage de l’élément sur la Place de marché peut prendre jusqu’à cinq minutes.

    ![kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Mettre à jour ou supprimer Kubernetes 

Quand vous mettez à jour l’élément Kubernetes, vous supprimez l’élément existant sur la Place de marché. Suivez les instructions fournies dans cet article pour ajouter la mise à jour Kubernetes à la Place de marché.

Pour supprimer l’élément Kubernetes :

1. Connectez-vous en tant qu’opérateur à Azure Stack à l’aide de PowerShell. Pour plus d’informations, consultez [Se connecter en tant qu’opérateur à Azure Stack à l’aide de PowerShell](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Recherchez l’élément Cluster Kubernetes actuel dans la galerie.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Notez le nom de l’élément actuel (par exemple, `Microsoft.AzureStackKubernetesCluster.0.3.0`)

4. Pour supprimer l’élément, utilisez l’applet de commande PowerShell suivante :

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Étapes suivantes

[Déployer Kubernetes sur Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Vue d’ensemble de l’offre de services dans Azure Stack](azure-stack-offer-services-overview.md)
