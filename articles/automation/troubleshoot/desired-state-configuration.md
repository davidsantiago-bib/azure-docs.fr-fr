---
title: Résoudre les problèmes avec la Configuration de l’état souhaité Azure Automation (DSC)
description: Cet article fournit des informations sur la résolution des problèmes de la Configuration de l’état souhaité
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: c8afa671a323e37a99be8b5a43d0a4823fe1877a
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58800874"
---
# <a name="troubleshoot-desired-state-configuration-dsc"></a>Dépanner la Configuration de l’état souhaité

Cet article fournit des informations sur la résolution des problèmes de la Configuration de l’état souhaité.

## <a name="common-errors-when-working-with-desired-state-configuration-dsc"></a>Erreurs courantes avec la Configuration d’état souhaité (DSC)

### <a name="unsupported-characters"></a>Scénario : Une configuration avec des caractères spéciaux ne peut pas être supprimée à partir du portail

#### <a name="issue"></a>Problème

Lorsque vous tentez de supprimer une configuration DSC à partir du portail, vous consultez l’erreur suivante :

```error
An error occured while deleteing the DSC configuration '<name>'.  Error-details: The arguement configurationName with the value <name> is not valid.  Valid configuration names can contain only letters,  numbers, and underscores.  The name must start with a letter.  The length of the name must be between 1 and 64 characters.
```

#### <a name="cause"></a>Cause :

Cette erreur est un problème temporaire qui est prévu pour être résolu.

#### <a name="resolution"></a>Résolution :

* Utilisez l’applet de commande Az « Remove-AzAutomationDscConfiguration » pour supprimer la configuration.
* La documentation relative à cette applet de commande n’a pas été encore mis à jour.  En attendant, reportez-vous à la documentation pour le module AzureRM.
  * [Remove-AzureRmAutomationDSCConfiguration](/powershell/module/azurerm.automation/Remove-AzureRmAutomationDscConfiguration)

### <a name="failed-not-found"></a>Scénario : Le nœud est en état d’échec avec une erreur « Introuvable »

#### <a name="issue"></a>Problème

Un rapport pour le nœud indique un état **Échec** et contient l’erreur :

```error
The attempt to get the action from server https://<url>//accounts/<account-id>/Nodes(AgentId=<agent-id>)/GetDscAction failed because a valid configuration <guid> cannot be found.
```

#### <a name="cause"></a>Cause :

Cette erreur se produit généralement quand le nœud est affecté à un nom de configuration (par exemple ABC) au lieu d’un nom de configuration de nœud (par exemple ABC.WebServer).

#### <a name="resolution"></a>Résolution :

* Assurez-vous que vous affectez le nœud avec « nom de configuration de nœud » et pas le « nom de configuration ».
* Vous pouvez affecter une configuration de nœud à un nœud à l'aide du portail Azure ou d’une applet de commande PowerShell.

  * Pour affecter une configuration de nœud à un nœud à l’aide du portail Azure, ouvrez le **les nœuds DSC** page, puis sélectionnez un nœud et cliquez sur **attribuer une configuration de nœud** bouton.  
  * Pour affecter une configuration de nœud à un nœud à l’aide d’applet de commande PowerShell, utilisez **Set-AzureRmAutomationDscNode** applet de commande

### <a name="no-mof-files"></a>Scénario : Aucune configuration de nœud (fichiers MOF) n’a été produite au cours d’une compilation de configuration

#### <a name="issue"></a>Problème

Votre tâche de compilation DSC s’interrompt avec l’erreur :

```error
Compilation completed successfully, but no node configuration.mofs were generated.
```

#### <a name="cause"></a>Cause :

Quand l’expression qui suit le mot-clé **Node** dans la configuration DSC s’évalue à `$null`, aucune configuration de nœud n’est générée.

#### <a name="resolution"></a>Résolution :

Une des solutions suivantes corrige ce problème :

* Assurez-vous que l’expression suivant pour le **nœud** mot clé dans la définition de configuration n’est pas l’évaluation sur $null.
* Si vous effectuez une transmission de ConfigurationData pendant la compilation de la configuration, vérifiez que vous transmettez les valeurs attendues nécessaires à la configuration depuis [ConfigurationData](../automation-dsc-compile.md#configurationdata).

### <a name="dsc-in-progress"></a>Scénario : Le rapport du nœud DSC se bloque à l’état « en cours »

#### <a name="issue"></a>Problème

La sortie de l’agent DSC est la suivante :

```error
No instance found with given property values
```

#### <a name="cause"></a>Cause :

Vous avez mis à niveau votre version de WMF et endommagé WMI.

#### <a name="resolution"></a>Résolution :

Pour résoudre ce problème, suivez les instructions de la [DSC problèmes connus et limitations](https://msdn.microsoft.com/powershell/wmf/5.0/limitation_dsc) article.

### <a name="issue-using-credential"></a>Scénario : Il est impossible d’utiliser des informations d’identification dans une configuration DSC

#### <a name="issue"></a>Problème

Votre tâche de compilation DSC a été interrompue avec l’erreur :

```error
System.InvalidOperationException error processing property 'Credential' of type <some resource name>: Converting and storing an encrypted password as plaintext is allowed only if PSDscAllowPlainTextPassword is set to true.
```

#### <a name="cause"></a>Cause :

Vous avez utilisé les informations d’identification dans une configuration mais que vous n’avez pas fourni la bonne **ConfigurationData** pour définir **PSDscAllowPlainTextPassword** sur true pour chaque configuration de nœud.

#### <a name="resolution"></a>Résolution :

* Veillez à passer dans la bonne **ConfigurationData** pour définir **PSDscAllowPlainTextPassword** sur true pour chaque configuration de nœud qui est mentionnée dans la configuration. Pour plus d’informations, consultez les [ressources d’Azure Automation DSC](../automation-dsc-compile.md#assets).

## <a name="next-steps"></a>Étapes suivantes

Si votre problème ne figure pas dans cet article ou si vous ne parvenez pas à le résoudre, utilisez un des canaux suivants pour obtenir de l’aide :

* Obtenez des réponses de la part d’experts Azure via les [Forums Windows](https://azure.microsoft.com/support/forums/)
* Connectez-vous avec [@AzureSupport](https://twitter.com/azuresupport), qui est le compte Microsoft Azure officiel pour améliorer l’expérience client en connectant la communauté Azure aux ressources appropriées : réponses, support technique et experts.
* Si vous avez besoin de plus d’aide, vous pouvez signaler un incident au support Azure. Accédez au [site du support Azure](https://azure.microsoft.com/support/options/) , puis cliquez sur **Obtenir un support**.
