---
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 242f601ced4838a1b4e559774c25d05de04ddb77
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2019
ms.locfileid: "57016329"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Ajouter un groupe de consommateurs à votre instance IoT Hub

Les groupes de consommateurs sont utilisés par les applications pour extraire des données de l’instance Azure IoT Hub. Dans ce didacticiel, vous créez un groupe de consommateurs qu’un prochain service Azure utilisera pour lire les données à partir de votre instance IoT Hub.

Pour ajouter un groupe de consommateurs à votre instance IoT Hub, procédez comme suit :

1. Dans le [portail Azure](https://portal.azure.com/), ouvrez votre instance IoT Hub.

2. Dans le volet gauche, cliquez sur **Point de terminaison prédéfinis**, sélectionnez **Événements** dans le volet supérieur, puis entrez un nom sous **Groupes de consommateurs** dans le volet droit. Cliquez sur **Enregistrer** après avoir changé la valeur de **Durée de vie par défaut** et rétabli sa valeur d’origine.

   ![Créer un groupe de consommateurs dans votre IoT Hub](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
