---
title: Exploration des journaux .NET dans Application Insights
description: Effectuez une recherche dans les journaux générés avec Trace, NLog ou Log4Net.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: mbullwin
ms.openlocfilehash: 8c722eb0db3022620ba03e02dd2ae00f97a78f28
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57552144"
---
# <a name="explore-netnet-core-trace-logs-in-application-insights"></a>Exploration des journaux .NET/.NET Core dans Application Insights

Si vous utilisez ILogger, NLog, log4Net ou System.Diagnostics.Trace pour le suivi de diagnostic dans votre application ASP.NET/ASP.NET Core, les journaux peuvent être envoyés à [Azure Application Insights][start], où vous pouvez les explorer et les rechercher. Les journaux sont fusionnés avec la télémétrie provenant de votre application, afin que vous puissiez identifier les traces associées au traitement des demandes de l’utilisateur et les mettre en corrélation avec d’autres événements et des rapports d’exception.

> [!NOTE]
> Avez-vous besoin du module de collecte de journaux ? Il s’agit d’un adaptateur très utile pour les enregistreurs d’événements tiers. Cependant, si vous n’utilisez pas déjà NLog, log4Net ou System.Diagnostics.Trace, vous pouvez appeler [Application Insights TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) directement.
>
>

## <a name="install-logging-on-your-app"></a>Installation de la journalisation sur votre application
Installez votre infrastructure de journalisation choisie dans votre projet. Ceci devrait générer une entrée dans le fichier app.config ou web.config.

```XML
    <configuration>
      <system.diagnostics>
    <trace autoflush="true" indentsize="0">
      <listeners>
        <add name="myAppInsightsListener" type="Microsoft.ApplicationInsights.TraceListener.ApplicationInsightsTraceListener, Microsoft.ApplicationInsights.TraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
   </configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Configuration d’Application Insights pour la collecte des journaux
Si vous ne l’avez pas encore fait, **[ajoutez Application Insights à votre projet](../../azure-monitor/app/asp-net.md)**. Une option permettant d’inclure le collecteur de journaux doit s’afficher.

Ou **configurez Application Insights** en cliquant avec le bouton droit dans l’Explorateur de solutions. Sélectionnez l’option **Configurer la collecte des traces**.

*Menu Application Insights non disponible ou aucune option pour le collecteur de journaux ?* Consultez la [Résolution des problèmes](#troubleshooting).

## <a name="manual-installation"></a>Installation manuelle
Utilisez cette méthode si votre type de projet n’est pas pris en charge par le programme d’installation Application Insights (par exemple, un projet de bureau Windows).

1. Si vous prévoyez d'utiliser log4Net ou NLog, installez-le dans votre projet.
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.
3. Recherchez « Application Insights »
4. Sélectionnez l’un des packages suivants :

   - Pour ILogger : [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.Extensions.Logging.ApplicationInsights.svg)](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
   - Pour NLog : [Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.NLogTarget.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
   - Pour Log4Net : [Microsoft.ApplicationInsights.Log4NetAppender](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.Log4NetAppender.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
   - Pour System.Diagnostics : [Microsoft.ApplicationInsights.TraceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.TraceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
   - [Microsoft.ApplicationInsights.DiagnosticSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.DiagnosticSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
   - [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EtwCollector.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
   - [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EventSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)

Le package NuGet installe les assemblys nécessaires, et lorsque nécessaire modifie le fichier web.config ou app.config.

## <a name="ilogger"></a>ILogger

Pour des exemples d’utilisation de l’implémentation Application Insights ILogger avec les applications Console et ASP.NET Core, consultez cet [article](ilogger.md).

## <a name="insert-diagnostic-log-calls"></a>Insertion d'appels de journaux de diagnostic
Si vous utilisez System.Diagnostics.Trace, un appel standard serait :

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Si vous préférez log4net ou NLog :

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>Utilisation des événements EventSource
Vous pouvez configurer les événements [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) à envoyer à Application Insights en tant que traces. D’abord, installez le package NuGet `Microsoft.ApplicationInsights.EventSourceListener`. Ensuite, modifiez la section `TelemetryModules` du fichier [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Pour chaque source, vous pouvez définir les paramètres suivants :
 * `Name` spécifie le nom de l’événement EventSource à collecter.
 * `Level` spécifie le niveau de journalisation à collecter. Peut être `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (Facultatif) spécifie la valeur entière de combinaisons de mots clés à utiliser.

## <a name="using-diagnosticsource-events"></a>Utilisation des événements DiagnosticSource
Vous pouvez configurer les événements [System.Diagnostics.Tracing.EventSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) de façon à les envoyer à Application Insights en tant que traces. Commencez par installer le package NuGet [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener). Ensuite, modifiez la section `TelemetryModules` du fichier [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Pour chaque DiagnosticSource à tracer, ajoutez une entrée avec l’attribut `Name` défini sur le nom de votre DiagnosticSource.

## <a name="using-etw-events"></a>Utilisation des événements ETW
Vous pouvez configurer les événements ETW à envoyer à Application Insights en tant que traces. D’abord, installez le package NuGet `Microsoft.ApplicationInsights.EtwCollector`. Ensuite, modifiez la section `TelemetryModules` du fichier [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md).

> [!NOTE] 
> Les événements ETW peuvent uniquement être collectés si le processus hébergeant le Kit de développement logiciel (SDK) s’exécute sous une identité qui membre des « Utilisateurs du journal de performances » ou des administrateurs.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Pour chaque source, vous pouvez définir les paramètres suivants :
 * `ProviderName` est le nom du fournisseur ETW à collecter.
 * `ProviderGuid` spécifie le GUID du fournisseur ETW à collecter, peut être utilisé à la place de `ProviderName`.
 * `Level` définit le niveau de journalisation à collecter. Peut être `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords` (Facultatif) définit la valeur entière de combinaisons de mots clés à utiliser.

## <a name="using-the-trace-api-directly"></a>Utilisation de l’API de suivi directement
Vous pouvez appeler directement l’API de suivi d’Application Insights. Les adaptateurs de journalisation utilisent cette API.

Par exemple : 

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

l’un des avantages de TrackTrace est que vous pouvez insérer des données relativement longues dans le message. Par exemple, vous pourriez y encoder des données POST.

Par ailleurs, vous pouvez ajouter un niveau de gravité à votre message. Comme pour les autres données de télémétrie, vous pouvez également ajouter des valeurs de propriété que vous pouvez utiliser pour filtrer ou rechercher différents jeux de traces. Par exemple : 

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Cela vous permettrait, dans [Recherche][diagnostic], de filtrer facilement tous les messages d’un niveau de gravité particulier portant sur une certaine base de données.

## <a name="explore-your-logs"></a>Exploration de vos journaux
Exécutez votre application en mode débogage ou déployez-la en direct.

Dans le panneau Vue d’ensemble de votre application du [portail Application Insights][portal], choisissez [Rechercher][diagnostic].

Vous pouvez par exemple :

* Filtrer selon les traces de journal ou les éléments avec des propriétés spécifiques
* Inspecter un élément spécifique en détail
* Rechercher d’autres données de télémétrie relatives à la même demande utilisateur (autrement dit, avec la même OperationId)
* Enregistrer la configuration de cette page en tant que favori

> [!NOTE]
> **Échantillonnage.**  Si votre application envoie des données en grand nombre et si vous utilisez le Kit de développement logiciel Application Insights pour ASP.NET version 2.0.0-beta3 ou ultérieure, la fonctionnalité d’échantillonnage adaptatif peut fonctionner et transmettre uniquement un pourcentage de vos données de télémétrie. [En savoir plus sur l'échantillonnage.](../../azure-monitor/app/sampling.md)
>
>

## <a name="next-steps"></a>Étapes suivantes
[Diagnostiquer les défaillances et les exceptions dans ASP.NET][exceptions]

[En savoir plus sur Recherche][diagnostic].

## <a name="troubleshooting"></a>Résolution de problèmes
### <a name="how-do-i-do-this-for-java"></a>Comment faire pour Java ?
Utilisez les [adaptateurs de journaux Java](../../azure-monitor/app/java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Le menu contextuel du projet ne contient aucune option Application Insights
* Vérifiez que les outils Application Insights sont installés sur cette machine de développement. Dans le menu Outils, Extensions et mises à jour de Visual Studio, recherchez les outils Application Insights. S’ils ne sont pas dans l’onglet Installé, ouvrez l’onglet En ligne et installez-les.
* Il peut s’agir d’un type de projet non pris en charge par les outils Application Insights. Utilisez [l’installation manuelle](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Aucune option d’adaptateur dans l’outil de configuration
* Vous devez d’abord installer l’infrastructure de journalisation.
* Si vous utilisez System.Diagnostics.Trace, assurez-vous que vous l’avez [configuré dans `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Disposez-vous de la dernière version d’Application Insights ? Dans le menu **Outils** de Visual Studio, choisissez **Extensions et mises à jour**, puis ouvrez l’onglet **Mises à jour**. Si les outils Developer Analytics sont visibles, cliquez dessus pour les mettre à jour.

### <a name="emptykey"></a>J’obtiens une erreur « La clé d’instrumentation ne peut pas être vide ».
Vous avez peut-être installé le package Nuget de l'enregistreur de données sans installer Application Insights.

Dans l’Explorateur de solutions, cliquez avec le bouton droit sur `ApplicationInsights.config` , puis sélectionnez **Mettre à jour Application Insights**. Une boîte de dialogue vous invite à vous connecter à Azure et à créer une ressource Application Insights, ou à réutiliser une ressource existante. Ceci devrait résoudre le problème.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Je peux voir des suivis dans la recherche de diagnostic, mais pas les autres événements
Le passage des événements et des demandes dans le pipeline peut prendre un certain temps.

### <a name="limits"></a>Quelle est la quantité de données conservée ?
Plusieurs facteurs affectent la quantité de données conservées. Consultez la section [Limites](../../azure-monitor/app/api-custom-events-metrics.md#limits) de la page sur les métriques d’événement client pour plus d’informations. 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Certaines entrées du journal ne sont pas affichées
 Si votre application envoie des données en grand nombre et si vous utilisez le Kit de développement logiciel Application Insights pour ASP.NET version 2.0.0-beta3 ou ultérieure, la fonctionnalité d’échantillonnage adaptatif peut fonctionner et transmettre uniquement un pourcentage de vos données de télémétrie. [En savoir plus sur l'échantillonnage.](../../azure-monitor/app/sampling.md)

## <a name="add"></a>Étapes suivantes
* [Configuration des tests de disponibilité et de réactivité][availability]
* [Résolution des problèmes][qna]

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md
