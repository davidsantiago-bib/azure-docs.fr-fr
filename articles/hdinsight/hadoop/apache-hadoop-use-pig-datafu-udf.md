---
title: Utiliser Apache DataFu avec Apache Pig sur HDInsight - Azure
description: Apache DataFu Pig est un ensemble de bibliothèques utilisables avec Apache Pig sur Apache Hadoop. Découvrez comment vous pouvez utiliser DataFu avec Pig sur votre cluster HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/16/2018
ms.author: hrasheed
ms.openlocfilehash: d67c3e452da05c626721d4c3144e612e6f9e0af4
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56338442"
---
# <a name="use-apache-datafu-pig-with-apache-pig-on-hdinsight"></a>Utiliser Apache DataFu Pig avec Apache Pig sur HDInsight

Découvrez comment utiliser Apache DataFu Pig avec HDInsight.

Apache DataFu Pig est un ensemble de bibliothèques open source utilisables avec Apache Pig sur Apache Hadoop.
Pour plus d’informations sur DataFu Pig, consultez [https://datafu.apache.org/](https://datafu.apache.org/).

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure.

* Un cluster Azure HDInsight (Linux ou Windows)

  > [!IMPORTANT]  
  > Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour plus d’informations, consultez [Suppression de HDInsight sous Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Des connaissances élémentaire de l’[utilisation d’Apache Pig sur HDInsight](hdinsight-use-pig.md)

## <a name="install-datafu-on-linux-based-hdinsight"></a>Installation de DataFu dans HDInsight basé sur Linux

> [!IMPORTANT]  
> DataFu est installé sur les clusters basés sur Linux 3.3 et versions ultérieures, ainsi que sur les clusters Windows. Il n’est pas installé sur les clusters Linux avant la version 3.3.
>
> Si vous utilisez un cluster basé sur Windows ou un cluster basé sur une version Linux ultérieure à la version 3.3, ignorez cette section.

DataFu peut être téléchargé et installé à partir du référentiel Maven. Utilisez les étapes suivantes pour rechercher la version dont vous avez besoin et pour l’ajouter à votre cluster HDInsight :

> [!WARNING]  
> Les versions de DataFu peuvent avoir des exigences qui ne sont pas satisfaites par HDInsight. Par exemple, si vous utilisez une version antérieure de DataFu, celle-ci peut nécessiter une version différente de Pig que celle qui est incluse dans HDInsight.

### <a name="find-a-version"></a>Rechercher une version

1. Dans votre navigateur web, accédez à https://mvnrepository.com/artifact/org.apache.datafu/datafu-pig et recherchez la version dont vous avez besoin.

2. Sélectionnez le numéro de version correspondant.

3. Sélectionnez __View all__ (Tout afficher) pour voir tous les fichiers.

4. Dans la liste des fichiers, recherchez le fichier .jar. Ce fichier est généralement le plus grand des fichiers listés, car il inclut toutes les dépendances. Cliquez avec le bouton droit sur le lien et copiez l’adresse du lien.

### <a name="download-datafu-to-hdinsight"></a>Télécharger DataFu dans HDInsight

1. Connectez-vous à votre cluster HDInsight Linux en utilisant SSH. Pour en savoir plus, voir [Utilisation de SSH avec Hadoop Linux sur HDInsight depuis Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Utilisez la commande suivante pour télécharger le fichier .jar de DataFu avec l’utilitaire wget :

    > [!IMPORTANT]  
    > Remplacez le lien dans la commande par l’URL que vous avez copiée précédemment.

    ```
    wget https://central.maven.org/maven2/org/apache/datafu/datafu-pig/1.4.0/datafu-pig-1.4.0.jar
    ```

3. Ensuite, chargez le fichier du stockage par défaut pour votre cluster HDInsight. Placer le fichier dans un stockage par défaut le rend accessible à tous les nœuds du cluster.

    > [!IMPORTANT]  
    > Remplacez le numéro de version dans le nom de fichier par la version que vous avez téléchargée.

    ```
    hdfs dfs -put datafu-pig-1.4.0.jar /example/jars
    ```

    > [!NOTE]  
    > La commande ci-dessus stocke le fichier jar dans `/example/jars`, car ce répertoire existe déjà sur le stockage en cluster. Vous pouvez utiliser n'importe quel emplacement dans le stockage en cluster HDInsight.

## <a name="use-datafu-with-pig"></a>Utilisation de DataFu avec Pig

Les étapes de cette section supposent que vous êtes familiarisé avec l’utilisation de Pig sur HDInsight. Pour plus d'informations sur l'utilisation de Pig avec HDInsight, consultez la rubrique [Utilisation de Pig avec HDInsight](hdinsight-use-pig.md).

> [!IMPORTANT]  
> Si vous avez installé DataFu manuellement en suivant les étapes de la section précédente, vous devez l’enregistrer avant de l’utiliser.
>
> * Si votre cluster utilise le stockage Azure, utilisez un chemin d’accès `wasb://`. Par exemple : `register wasb:///example/jars/datafu-pig-1.4.0.jar`.
>
> * Si votre cluster utilise Azure Data Lake Store Gen2, utilisez un chemin d'accès `abfs://`. Par exemple : `register abfs://home/example/jars/datafu-pig-1.4.0.jar`.
>
> * Si votre cluster utilise Azure Data Lake Store Gen1, utilisez un chemin d'accès `adl://`. Par exemple : `register adl://home/example/jars/datafu-pig-1.4.0.jar`.

Vous définissez souvent un alias pour les fonctions DataFu. L’exemple suivant définit un alias de `SHA` :

```piglatin
DEFINE SHA datafu.pig.hash.SHA();
```

Vous pouvez ensuite utiliser cet alias dans un script Pig Latin pour générer un hachage pour les données d’entrée. Par exemple, le code suivant remplace l’emplacement dans les données d’entrée avec une valeur de hachage :

```piglatin
raw = LOAD '/HdiSamples/HdiSamples/SensorSampleData/building/building.csv' USING
    org.apache.pig.piggybank.storage.CSVExcelStorage(',', 'NO_MULTILINE', 'UNIX', 'SKIP_INPUT_HEADER') AS
    (int1:int,
     id1:chararray,
     int2:int,
     id2:chararray,
     location:chararray);
mask = FOREACH raw GENERATE int1, id1, int2, id2, SHA(location);
DUMP mask;
```

Voici la sortie qu’il génère :

    (1,M1,25,AC1000,aa5ab35a9174c2062b7f7697b33fafe5ce404cf5fecf6bfbbf0dc96ba0d90046)
    (2,M2,27,FN39TG,7a1ca4ef7515f7276bae7230545829c27810c9d9e98ab2c06066bee6270d5153)
    (3,M3,28,JDNS77,07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)
    (4,M4,17,GG1919,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (5,M5,3,ACMAX22,1ed8c7e56c947bebc0cfcf88c4ea0f02c944568f71df893a99970e4f0c78cddc)
    (6,M6,9,AC1000,c96dd81db196cca5f57bd4270bbb9d9e9d1b242d67f9364005ee1dfdc2632523)
    (7,M7,13,FN39TG,3e049d78d958038ac6bd5dcf038075cc73362b4956aaf7308c3a69c8eca76297)
    (8,M8,25,JDNS77,c1ef40ce0484c698eb4bd27fe56c1e7b68d74f9780ed674210d0e5013dae45e9)
    (9,M9,11,GG1919,a7d355b26bda6bf1196ccffead0b2cf2b81f0a9de5b4876b44407f1dc07e51e6)
    (10,M10,23,ACMAX22,10436829032f361a3de50048de41755140e581467bc1895e6c1a17f423e42d10)
    (11,M11,14,AC1000,348841ef53dbd5a64008a86be432973db790774fb28b59b0d99702a3188b3705)
    (12,M12,26,FN39TG,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (13,M13,25,JDNS77,ed9ad13611d7164839dd3feaf9a7f6a75a4138f389e0d45f8e07fa38da1116a2)
    (14,M14,17,GG1919,80db4ccdca106d37b920206331fcfe3e9e50a9e763d89b54ce3ad5ac8cf30f03)
    (15,M15,19,ACMAX22,3552ac0c032467fbf592cb4d10c5c9267800c01e343ad8ca557256d882ae9327)
    (16,M16,23,AC1000,07c67d76ef92ac9853588e098983fefba4ba5965f8fec95f42ab0d04c27865ba)
    (17,M17,11,FN39TG,557c1599d9a04895d3817d293e0806a4419a14de31958386182798d0d2ed3a56)
    (18,M18,25,JDNS77,dbc74a36d8e7439c45c64d856388506cc9b1218619cef979c3d605115a7a4546)
    (19,M19,14,GG1919,be55ef3f4c4e6c2d9c2afe2a33ac90ad0f50d4de7f9163999877e2a9ca5a54f8)
    (20,M20,19,ACMAX22,ea0b937ea317101ee2c26b03a4843a19ceced8a2b9673c3cf409a726ca2b0fd8)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations sur DataFu ou Pig, consultez les documents suivants :

* [Bien démarrer avec Apache DataFu Pig](https://datafu.apache.org/docs/datafu/getting-started.html).
* [Utilisation d’Apache Pig avec HDInsight](hdinsight-use-pig.md)
