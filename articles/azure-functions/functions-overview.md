---
title: Overzicht van Azure Functions | Microsoft Docs
description: Informatie over het gebruik van Azure Functions voor het optimaliseren van asynchrone workloads.
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/03/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 41f26a4b03a6431aaad21bda6336b8840d2d923f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="an-introduction-to-azure-functions"></a>Een inleiding tot Azure Functions  
Azure Functions is een oplossing voor het eenvoudig uitvoeren van kleine stukjes code, ofwel ‘functies’, in de cloud. U hoeft alleen de code te schrijven die u op dat moment nodig hebt, zonder dat u een complete toepassing of de bijbehorende infrastructuur nodig hebt. Functies kunnen aanbrengen ontwikkelen nog efficiënter en kunt u de gewenste programmeertaal, zoals C#, F #, Node.js, Java of PHP. U betaalt alleen voor de tijd dat uw code wordt uitgevoerd en Azure zorgt het eventuele schalen. Azure Functions, kunt u ontwikkelen [zonder server](https://azure.microsoft.com/overview/serverless-computing/) toepassingen in Microsoft Azure.

Dit onderwerp bevat een globaal overzicht van Azure Functions. Als u meteen en aan de slag met functies wilt, beginnen met een [uw eerste Azure-functie maken](functions-create-first-azure-function.md). Als u behoefte hebt aan meer technische informatie over Functions, raadpleegt u de [naslaginformatie voor ontwikkelaars](functions-reference.md).

## <a name="features"></a>Functies
Hier volgen enkele belangrijke functies van functies:

* **Keuze van taal** : schrijf functies met C#, F #, Node.js, Java, PHP, batch, bash of elk uitvoerbaar bestand.
* **Betalen per gebruik**: betaal alleen voor de tijd die nodig is voor het uitvoeren van uw code. Raadpleeg de optie voor het hostingabonnement Consumption in de sectie over [prijzen](#pricing).  
* **Breng uw eigen afhankelijkheden mee**: Functions ondersteunt NuGet en NPM, zodat u uw favoriete bibliotheken kunt gebruiken.  
* **Geïntegreerde beveiliging**: beveilig HTTP-geactiveerde functies met OAuth-providers zoals Azure Active Directory, Facebook, Google, Twitter en Microsoft-account.  
* **Eenvoudige integratie**: maak eenvoudig gebruik van Azure-services en Software-as-a-Service (SaaS)-producten. Zie de sectie [Integraties](#integrations) hieronder voor enkele voorbeelden.  
* **Flexibel ontwikkelen** - Code van uw functies direct in de portal of stel doorlopende integratie en implementeer uw code via [GitHub](../app-service/scripts/app-service-cli-continuous-deployment-github.md), [Visual Studio Team Services](../app-service/scripts/app-service-cli-continuous-deployment-vsts.md), en andere [ondersteunde ontwikkelprogramma's](../app-service/app-service-deploy-local-git.md).  
* **Open-source**: de runtime van Functions is open-source en [beschikbaar op GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Wat kan ik doen met Functions?
Functies is een uitstekende oplossing voor het verwerken van gegevens, integreren van systemen, werken met de internet-der dingen (IoT) en het bouwen van eenvoudige API's en microservices. Overweeg om Functions te gebruiken voor taken zoals het verwerken van afbeeldingen of bestellingen, onderhouden van bestanden, of voor andere taken die u wilt uitvoeren volgens een planning. 

Functions biedt sjablonen waarmee u meteen aan de slag kunt met essentiële scenario's, zoals:

* **HTTPTrigger**: activeer de uitvoering van de code met een HTTP-aanvraag. Zie voor een voorbeeld [maken van uw eerste functie](functions-create-first-azure-function.md).
* **TimerTrigger**: voer opschoonacties of andere batchtaken uit volgens een vooraf gedefinieerd schema. Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door een timer](functions-create-scheduled-function.md).
* **GitHub webhook**: reageer op gebeurtenissen die plaatsvinden in uw GitHub-opslagplaatsen. Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door een GitHub webhook](functions-create-github-webhook-triggered-function.md).
* **Generic webhook**: verwerk HTTP-webhook-aanvragen van services die webhooks ondersteunen. Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door een algemene webhook](functions-create-generic-webhook-triggered-function.md).
* **CosmosDBTrigger** -proces Azure Cosmos DB documenten wanneer ze worden toegevoegd of in verzamelingen in een NoSQL-database bijgewerkt. Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door Azure Cosmos DB](functions-create-cosmos-db-triggered-function.md).
* **BlobTrigger**: verwerk Azure Storage-blobs wanneer ze worden toegevoegd aan containers. Deze functie kunt u bijvoorbeeld gebruiken voor het wijzigen van de grootte van afbeeldingen. Zie voor meer informatie [Blob-opslag-bindingen](functions-bindings-storage-blob.md).
* **QueueTrigger**: reageer op berichten die binnenkomen in een Azure Storage-wachtrij. Zie voor een voorbeeld [maken van een functie die wordt geactiveerd door Azure Queue storage](functions-create-storage-queue-triggered-function.md).
* **EventHubTrigger**: reageer op gebeurtenissen die worden geleverd aan een Azure Event Hub. Dit is met name handig voor toepassingsinstrumentatie, verwerking van gebruikerservaringen of werkstromen en scenario's met betrekking tot het Internet of Things (IoT). Zie voor meer informatie [Event Hubs bindingen](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger**: koppel uw code aan andere Azure-services of on-premises services door te luisteren naar berichtenwachtrijen. Zie voor meer informatie [Service Bus-bindingen](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger**: koppel uw code aan andere Azure-services of on-premises services met een abonnement op onderwerpen. Zie voor meer informatie [Service Bus-bindingen](functions-bindings-service-bus.md).

Azure Functions ondersteunt *triggers*, waarmee de uitvoering van code kan worden gestart, en *bindingen*, waarmee het coderen voor invoer- en uitvoergegevens kan worden vereenvoudigd. Zie [Naslaginformatie voor ontwikkelaars over triggers en bindingen van Azure Functions](functions-triggers-bindings.md) voor een gedetailleerde beschrijving van de triggers en bindingen van Azure Functions.

## <a name="integrations"></a>Integraties
Azure Functions integreert met diverse services van Azure en derden. Deze services kunnen uw functie activeren en het uitvoeren starten, of fungeren als invoer en uitvoer voor uw code. De volgende service-integraties worden ondersteund door Azure Functions:

* Azure Cosmos DB
* Azure Event Hubs 
* Azure Event Grid
* Azure Mobile Apps (tabellen)
* Azure Notification Hubs
* Azure Service Bus (wachtrijen en onderwerpen)
* Azure Storage (blob, wachtrijen en tabellen) 
* GitHub (webhooks)
* On-premises (met Service Bus)
* Twilio (SMS-berichten)

## <a name="pricing"></a>Wat kost Functions?
Azure Functions heeft twee soorten prijsstelling. U kunt kiezen die het beste past bij uw behoeften: 

* **Consumption-abonnement**: wanneer de functie wordt uitgevoerd, biedt Azure alle benodigde rekenbronnen. U hoeft u geen zorgen te maken over het beheer van bronnen en betaalt alleen voor de tijd die nodig is voor het uitvoeren van de code. 
* **App Service-abonnement**: voer uw functies net zo uit als uw web-apps, mobiele apps en API-apps. Wanneer u App Service al voor andere toepassingen gebruikt, voert u de functies kosteloos binnen hetzelfde abonnement uit. 

Zie voor meer informatie over hostingplannen [Azure Functions die als host fungeert voor de vergelijking plan](functions-scale.md). Volledige prijsinformatie is beschikbaar op de pagina [Prijzen voor Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Volgende stappen
* [Uw eerste Azure-functie maken](functions-create-first-azure-function.md)  
  Ga meteen aan de slag en maak uw eerste functie met de Azure Functions-snelstartgids. 
* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Biedt meer technische informatie over de Azure Functions-runtime en bevat gedetailleerde informatie over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het hostingabonnement Consumption, en helpt u bij het kiezen van het juiste abonnement. 
* [Meer informatie over Azure App Service](../app-service/app-service-web-overview.md)  
  Azure Functions maakt gebruik van Azure App Service voor kernfunctionaliteit zoals implementaties, omgevingsvariabelen en diagnostische gegevens. 

