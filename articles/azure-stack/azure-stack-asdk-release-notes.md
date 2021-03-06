---
title: Opmerkingen bij de release van Microsoft Azure-Stack Development Kit | Microsoft Docs
description: Verbeteringen, correcties en bekende problemen voor Azure Stack Development Kit.
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: twooley
ms.openlocfilehash: 95f63bc65491e56832b2c473d539cc702c38e584
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-stack-development-kit-release-notes"></a>Azure-Stack Development Kit release-opmerkingen

*Van toepassing op: Azure stapelen Development Kit*

Deze releaseopmerkingen bevatten informatie over verbeteringen, correcties en bekende problemen in Azure Stack Development Kit. Als u niet zeker weet welke versie u uitvoert, kunt u [de portal gebruiken om te controleren](azure-stack-updates.md#determine-the-current-version).

## <a name="build-201710201"></a>Build 20171020.1

### <a name="improvements-and-fixes"></a>Verbeteringen en oplossingen

Zie de lijst met verbeteringen en verbeteringen in de build 20171020.1 de [verbeteringen en correcties](azure-stack-update-1710.md#improvements-and-fixes) sectie van de 1710 release-opmerkingen voor Azure-Stack geïntegreerd systemen. Sommige van de items die worden vermeld in de sectie 'aanvullende kwaliteit verbeteringen en correcties' zijn alleen relevant voor geïntegreerde systemen.

Ook zijn de volgende correcties aangebracht:
- Een probleem opgelost waarbij de resourceprovider voor Compute een onbekende status weergegeven.
- Een probleem opgelost waarbij quota's mogelijk niet weergegeven in de beheerdersportal nadat u deze maken en later probeert om details van abonnement weer te geven.

### <a name="known-issues"></a>Bekende problemen

#### <a name="powershell"></a>PowerShell
- De versie van de PowerShell-module AzureRM 1.2.11 wordt geleverd met een lijst met wijzigingen op te splitsen. Voor informatie over het upgraden van de 1.2.10 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).
 
#### <a name="deployment"></a>Implementatie
- U moet een tijd-server op IP-adres opgeven tijdens de implementatie.

#### <a name="infrastructure-management"></a>Beheer van infrastructuur
- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.
- De baseboard management controller (BMC) IP-adres en het model worden niet weergegeven in de essentiële informatie van een scale unit-knooppunt. Dit is verwacht gedrag in Azure Stack Development Kit.

#### <a name="portal"></a>Portal
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
-  Voor elke werkstroom waarin u een abonnement, resourcegroep of locatie in een vervolgkeuzelijst selecteren, kan u een of meer van de volgende problemen optreden:

   - Mogelijk ziet u een lege rij boven aan de lijst. U moet nog steeds mogelijk een item selecteren zoals verwacht.
   - Als de lijst met items in de vervolgkeuzelijst kort is, kan het niet mogelijk om een van de namen van items weer te geven.
   - Als u meerdere gebruikersabonnementen hebt, kan de lijst met resources groep vervolgkeuzelijst leeg zijn. 

   Om de problemen met de laatste twee omzeilen, kunt u de naam van het abonnement of resourcegroep (als u deze kent) typen of u kunt in plaats daarvan PowerShell gebruiken.

- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.
- In de **activering vereist** waarschuwing Waarschuwingsdetails, niet op de koppeling naar de **AzureBridge** onderdeel. Als u dit doet, de **overzicht** blade zonder succes probeert te laden, en won't time-out.
- In de beheerdersportal ziet u een **fout bij het ophalen van tenants** fout in de **meldingen** gebied. U kunt deze fout negeren.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement weergeven met behulp van de Azure-Stack-portals. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Wanneer u probeert items toevoegen aan de Stack van Azure marketplace met behulp van de **toevoegen uit Azure** optie, niet alle items mogelijk niet zichtbaar voor downloaden.
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken. 
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.
- Instellingen voor virtuele-machineschaalsets schalen zijn niet beschikbaar in de portal. Als tijdelijke oplossing kunt u [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Vanwege verschillen tussen versies van PowerShell, moet u de `-Name` parameter in plaats van `-VMScaleSetName`.

#### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (gewoonlijk aangeduid als een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.

#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
 
#### <a name="usage-and-billing"></a>Gebruik en facturering
- Openbare IP-adres meter gebruiksgegevens toont dezelfde *datum/tijd gebeurtenis* waarde voor elke record in plaats van de *TimeDate* tijdstempel die wordt aangegeven wanneer de record is gemaakt. U niet op dit moment kunnen deze gegevens gebruiken om uit te voeren nauwkeurige registratie van het gebruik van openbare IP-adres.

## <a name="build-201709283"></a>Build 20170928.3

### <a name="known-issues"></a>Bekende problemen

#### <a name="powershell"></a>PowerShell
- De versie van de PowerShell-module AzureRM 1.2.11 wordt geleverd met een lijst met wijzigingen op te splitsen. Voor informatie over het upgraden van de 1.2.10 versie, Zie de [Migratiehandleiding](https://aka.ms/azspowershellmigration).

#### <a name="deployment"></a>Implementatie
- U moet een tijd-server op IP-adres opgeven tijdens de implementatie.

 #### <a name="infrastructure-management"></a>Beheer van infrastructuur
- Schakel geen infrastructuur back-up op de **infrastructuur back-up** blade.
- De resourceprovider voor Compute bevat een onbekende status.
- De baseboard management controller (BMC) IP-adres en het model worden niet weergegeven in de essentiële informatie van een scale unit-knooppunt. Dit is verwacht gedrag in Azure Stack Development Kit. 
   
#### <a name="portal"></a>Portal
- Mogelijk ziet u een lege dashboard in de portal. Als u wilt herstellen het dashboard, selecteert u het pictogram tandwielpictogram in de rechterbovenhoek van de portal en selecteer vervolgens **standaardinstellingen herstellen**.
- Wanneer u de eigenschappen van een resourcegroep weergeven de **verplaatsen** knop is uitgeschakeld. Dit is verwacht gedrag. Brongroepen verplaatsen tussen abonnementen wordt momenteel niet ondersteund.
- U ziet een **activering vereist** waarschuwingsmelding dat aangeeft dat het registreren van uw Azure-Stack Development Kit noodzakelijk. Dit is verwacht gedrag.
- In de **activering vereist** waarschuwing Waarschuwingsdetails, niet op de koppeling naar de **AzureBridge** onderdeel. Als u dit doet, de **overzicht** blade zonder succes probeert te laden, en won't time-out.
- Quota's mogelijk niet weergegeven in de beheerdersportal nadat u deze maken en probeer het later weergave de details van abonnement. Als tijdelijke oplossing in **Services en quota's**, klikt u op **toevoegen**, en een nieuwe vermelding toe te voegen.
- Gebruiker abonnementen resulteert in een zwevende resources te verwijderen. Als tijdelijke oplossing Gebruikersbronnen of de hele resourcegroep eerst te verwijderen en verwijder vervolgens de gebruikersabonnementen.
- U bent geen kunnen machtigingen aan uw abonnement met behulp van de Azure-Stack-portals weergeven. Als tijdelijke oplossing kunt u machtigingen controleren met behulp van Powershell.
  
#### <a name="marketplace"></a>Marketplace
- Gebruikers de volledige marketplace zonder een abonnement kunnen bladeren en beheeritems zoals plannen en aanbiedingen kunnen zien. Deze items zijn niet-functioneel voor gebruikers.
 
#### <a name="compute"></a>Compute
- Gebruikers krijgen de optie voor het maken van een virtuele machine met geografisch redundante opslag. Deze configuratie zorgt ervoor dat de virtuele machine wordt gemaakt mislukken.
- U kunt een virtuele machine beschikbaarheidsset alleen met een domein met fouten van een en een updatedomein van een configureren.
- Er is geen marketplace-ervaring voor het maken van virtuele-machineschaalsets. U kunt een schaal ingesteld met behulp van een sjabloon maken.

#### <a name="networking"></a>Netwerken
- U kunt een load balancer met een openbaar IP-adres maken met behulp van de portal. Als tijdelijke oplossing kunt u PowerShell gebruiken voor het maken van de load balancer.
- Wanneer u een network load balancer maken, moet u een regel network address translation (NAT) maken. Als u dat niet doet, ontvangt u een fout wanneer u probeert een NAT-regel toevoegen nadat de load balancer is gemaakt.
- Onder **Networking**, als u op **verbinding** voor het instellen van een VPN-verbinding, **VNet-naar-VNet** wordt vermeld als een mogelijke verbindingstype. Selecteer deze optie niet. Op dit moment wordt alleen de **Site-naar-site (IPsec)** optie wordt ondersteund.
- U kunt een openbare IP-adres van een virtuele machine (VM) kan niet loskoppelen nadat de virtuele machine is gemaakt en gekoppeld aan dat IP-adres. Disassociation wordt weergegeven om te werken, maar de eerder toegewezen openbaar IP-adres blijft gekoppeld aan de oorspronkelijke virtuele machine. Dit gebeurt zelfs als u een nieuwe virtuele machine het IP-adres toewijzen (ook wel een *VIP's wisselen*). Alle toekomstige verbinding probeert te maken via dit resultaat van de IP-adres in van een verbinding met de oorspronkelijk gekoppelde virtuele machine en niet naar het nieuwe bericht. U moet nieuwe openbare IP-adressen op dit moment alleen gebruiken voor het maken van nieuwe virtuele machine.


#### <a name="sqlmysql"></a>SQL/MySQL
- Dit kan een uur duren voordat tenants databases in een nieuwe SQL- of MySQL SKU kunnen maken. 
- Maken van artikelen die rechtstreeks op de SQL- en MySQL die als host fungeert voor servers die niet worden uitgevoerd door de resourceprovider wordt niet ondersteund en kan leiden tot een niet-overeenkomende staat.

#### <a name="app-service"></a>App Service
- Een gebruiker moet de storage resourceprovider registreren voordat ze hun eerste Azure-functie in het abonnement maken.
