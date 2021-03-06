---
title: 'Inleiding tot Azure Cosmos DBL: Table-API | Microsoft Docs'
description: Meer informatie over hoe u Azure Cosmos DB kunt gebruiken om grote hoeveelheden sleutelwaardegegevens met lage latentie op te slaan en op te vragen met behulp van de populaire OSS MongoDB-API's.
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2017
ms.author: arramac
ms.openlocfilehash: 6a399a3a7979f6165d26eb48505242976d51e64f
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Inleiding tot Azure Cosmos DB: tabel-API

[Azure Cosmos DB](introduction.md) biedt de Table-API (preview) voor toepassingen die zijn geschreven voor Azure Table-opslag en die premium-mogelijkheden nodig hebben, zoals:

* [Kant-en-klare wereldwijde distributie](distribute-data-globally.md).
* [Speciale doorvoer](partition-data.md) overal ter wereld.
* Latentie van slechts enkele milliseconden op het 99e percentiel.
* Gegarandeerde hoge beschikbaarheid.
* [Automatische secundaire indexering](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Deze toepassingen kunnen met behulp van de Table-API zonder codeaanpassingen worden gemigreerd naar Azure Cosmos DB en zo gebruikmaken van premium-mogelijkheden. De tabel-API is beschikbaar voor .NET- en Python.

Het is raadzaam om eerst de volgende video te bekijken, waarin Aravind Ramachandran uitlegt hoe u aan de slag gaat met de Table-API voor Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="table-offerings"></a>Aanbiedingen voor Table
Als u momenteel gebruikmaakt van Azure Table-opslag, levert overstappen naar de preview-versie van de Azure Cosmos DB Table-API de volgende voordelen op:

| | Azure-tabelopslag | Azure Cosmos DB Table-API (preview) |
| --- | --- | --- |
| Latentie | Snel, maar geen bovengrens voor latentie. | Latentie van slechts enkele milliseconden voor lees- en schrijfbewerkingen, ondersteund door <10 ms latentie voor leesbewerkingen en <15 ms latentie voor schrijfbewerkingen in het 99e percentiel, op elke schaal, overal ter wereld. |
| Doorvoer | Model voor variabele doorvoersnelheid. Tabellen hebben een schaalbaarheidslimiet van 20.000 bewerkingen/sec. | Zeer schaalbaar met [toegewezen gereserveerde doorvoer per tabel](request-units.md), op basis van serviceovereenkomsten. Accounts hebben geen bovengrens voor doorvoer en bieden ondersteuning voor > 10 miljoen bewerkingen/sec per tabel. |
| Wereldwijde distributie | Eén regio met één optioneel leesbaar secundair leesgebied voor hoge beschikbaarheid. U kunt geen failover starten. | [Kant en klare wereldwijde distributie](distribute-data-globally.md) tussen 1 tot 30+ regio's. Ondersteuning voor [kant en klare wereldwijde distributie](regional-failover.md), op elk moment en overal ter wereld. |
| Indexeren | Alleen primaire index op PartitionKey en RowKey. Geen secundaire indexen. | Automatische en volledige indexering voor alle eigenschappen, geen indexbeheer. |
| Query’s uitvoeren | Voor de queryuitvoering wordt een index gebruikt als primaire sleutel. In andere gevallen wordt er gescand. | Query's kunnen profiteren van de automatische indexering van eigenschappen voor een snelle uitvoertijden van query's. De database-engine van Azure Cosmos DB ondersteunt combinaties, georuimtelijke functies en sorteren. |
| Consistentie | Sterke in primaire regio. Mogelijk in secundaire regio. | [Vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor een wisselwerking tussen beschikbaarheid, latentie, doorvoer en consistentie op basis van uw toepassingsvereisten. |
| Prijzen | Geoptimaliseerd voor opslag. | Geoptimaliseerd voor doorvoer. |
| SLA's | 99,99% beschikbaarheid. | 99,99% beschikbaarheid binnen een enkele regio en de mogelijkheid om meer regio's toe te voegen voor een hogere beschikbaarheid. [Toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) op algemene beschikbaarheid. |

## <a name="get-started"></a>Aan de slag

Maak een Azure Cosmos DB-account in de [Azure-portal](https://portal.azure.com). Ga dan aan de slag met onze [Snelstartgids voor Table-API met behulp van .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanwijzingen om aan de slag te gaan:
* [Een .NET-toepassing ontwikkelen met de Table-API](create-table-dotnet.md)
* [Ontwikkelen met de Table-API in .NET](tutorial-develop-table-dotnet.md)
* [Tabelgegevens opvragen met de Table-API](tutorial-query-table.md)
* [Meer informatie over het instellen van wereldwijde distributie met Azure Cosmos DB met behulp van de Table-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table .NET API](table-sdk-dotnet.md)
* [Inleiding tot Azure Cosmos DB Table SDK voor Python](table-sdk-python.md)

