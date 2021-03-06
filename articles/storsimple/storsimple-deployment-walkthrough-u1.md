---
title: Een StorSimple-apparaat implementeren (Update 1) | Microsoft Docs
description: Hier vindt u de stappen en aanbevolen procedures voor de implementatie van het StorSimple Update 1-apparaat en de bijbehorende service.
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: ac631d3c-3c53-4c9e-9e4a-5c61c0cd8167
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 4d568fb2eca418ca939f7a76ac24197a0457fe47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>Een on-premises StorSimple-apparaat implementeren (Update 1)
> [!div class="op_single_selector"]
> * [Update 2](storsimple-deployment-walkthrough-u2.md)
> * [Update 1](storsimple-deployment-walkthrough-u1.md)
> * [GA Release](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Overzicht
Welkom bij de implementatie van Microsoft Azure StorSimple-apparaten. Deze zelfstudies voor implementatie zijn van toepassing op StorSimple 8000 Series Update 1.0. In deze reeks zelfstudies wordt beschreven hoe u een StorSimple-apparaat configureert en de reeks bevat een configuratiecontrolelijst, configuratievereisten en gedetailleerde configuratiestappen.

Bij de informatie in deze zelfstudies wordt ervan uitgegaan dat u de voorzorgsmaatregelen hebt gelezen en uw StorSimple-apparaat hebt uitgepakt, geplaatst en alle kabels hebt aangesloten. Als u dit nog moet doen, lees dan eerst de [voorzorgsmaatregelen](storsimple-safety.md). Afhankelijk van het model kunt u het apparaat vervolgens uitpakken, op het rek monteren en bekabelen door de instructies te volgen in:

* [De 8100 uitpakken, op het rek monteren en bekabelen](storsimple-8100-hardware-installation.md)
* [De 8600 uitpakken, op het rek monteren en bekabelen](storsimple-8600-hardware-installation.md)

U hebt beheerdersbevoegdheden nodig om het installatie- en configuratieproces uit te voeren. U wordt geadviseerd om de configuratiecontrolelijst te raadplegen voordat u begint. Het implementatie- en configuratieproces kan enige tijd duren.

> [!NOTE]
> De StorSimple-implementatiegegevens die zijn gepubliceerd op de website van Microsoft Azure zijn alleen van toepassing op apparaten uit de StorSimple 8000-serie. Voor volledige informatie over apparaten uit de 5000- en 7000-serie gaat u naar: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Raadpleeg de [Introductiehandleiding van het StorSimple-systeem ](http://onlinehelp.storsimple.com/111_Appliance/) voor informatie over de implementatie van de 5000- en 7000-serie.
> 
> 

## <a name="deployment-steps"></a>Implementatiestappen
Voer de volgende verplichte stappen uit om uw StorSimple-apparaat te configureren en te verbinden met uw StorSimple Manager-service. Naast de verplichte stappen zijn er nog optionele stappen en procedures die u tijdens de implementatie misschien nodig hebt. In de stapsgewijze implementatie-instructies wordt aangeven wanneer u deze optionele stappen moet uitvoeren.

| Stap | Beschrijving |
| --- | --- |
| **VEREISTEN** |Deze moeten zijn worden voltooid ter voorbereiding van de implementatie. |
| Configuratiecontrolelijst voor implementatie. |Gebruik deze controlelijst om informatie te verzamelen en te registreren voorafgaand aan en tijdens de implementatie. |
| Vereisten voor implementatie. |Hiermee wordt gecontroleerd of de omgeving gereed is voor implementatie. |
|  | |
| **STAPSGEWIJZE IMPLEMENTATIE** |Deze stappen zijn vereist om uw StorSimple-apparaat in productie te nemen. |
| Stap 1: een nieuwe service maken. |Stel cloudbeheer en -opslag voor uw StorSimple-apparaat in. U kunt deze stap overslaan als u al een service voor andere StorSimple-apparaten hebt. |
| Stap 2: de serviceregistratiesleutel ophalen. |Gebruik deze sleutel om uw StorSimple-apparaat te registreren en te verbinden met de managementservice. |
| Stap 3: het apparaat configureren en registreren via Windows PowerShell voor StorSimple. |Verbind het apparaat met uw netwerk en registreer het bij Azure om de installatie met behulp van de managementservice te voltooien. |
| Stap 4: minimale apparaatconfiguratie voltooien</br>Optioneel: het StorSimple-apparaat bijwerken. |Gebruik de managementservice om de installatie van het apparaat te voltooien en het in te schakelen voor opslag. |
| Stap 5: een volumecontainer maken. |Maak een container voor het inrichten van volumes. Een volumecontainer heeft opslagaccount, bandbreedte en versleutelingsinstellingen voor alle volumes in de container. |
| Stap 6: een volume maken. |Richt opslagvolume(s) op het StorSimple-apparaat in voor uw servers. |
| Stap 7: een volume koppelen, initialiseren en formatteren.</br>Optioneel: MPIO configureren. |Verbind uw servers met de iSCSI-opslag die is geleverd door het apparaat. U kunt MPIO desgewenst configureren om ervoor te zorgen dat uw servers koppelings-, netwerk- en interfacefouten kunnen tolereren. |
| Stap 8: een back-up maken. |Stel een back-upbeleid in om uw gegevens te beschermen. |
|  | |
| **OVERIGE PROCEDURES** |Mogelijk moet u deze procedures raadplegen tijdens de implementatie van uw oplossing. |
| Een nieuw opslagaccount voor de service configureren. | |
| PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat. | |
| Het IQN ophalen van een Windows Server-host. | |
| Een handmatige back-up maken. | |

## <a name="deployment-configuration-checklist"></a>Configuratiecontrolelijst voor implementatie
In de volgende controlelijst voor de implementatie-configuratie wordt beschreven welke informatie u moet verzamelen voordat en terwijl u de software op uw StorSimple-apparaat configureert. De implementatie van het StorSimple-apparaat in uw omgeving verloopt gestroomlijnder wanneer u deze gegevens zoveel mogelijk op voorhand voorbereidt. Gebruik deze controlelijst eveneens om de configuratiegegevens te noteren terwijl u het apparaat implementeert.

| Fase | Parameter | Details | Waarden |
| --- | --- | --- | --- |
| **Uw apparaat bekabelen** |Seriële toegang |Eerste apparaatconfiguratie |Ja/Nee |
|  | | | |
| **Apparaat configureren en registreren** |Netwerkinstellingen Data 0 |IP-adres Data 0:</br>Subnetmasker:</br>Gateway:</br>Primaire DNS-server:</br>Primaire NTP-server:</br>IP/FQDN van webproxyserver (optioneel):</br>Webproxypoort: | |
| &nbsp; |Wachtwoord apparaatbeheerder |Wachtwoord moet tussen 8 en 15 tekens lang zijn en kleine letters, hoofdletters, numerieke en speciale tekens bevatten. | |
| &nbsp; |Wachtwoord StorSimple Snapshot Manager |Wachtwoord moet 14 of 15 tekens lang zijn en kleine letters, hoofdletters, numerieke en speciale tekens bevatten. | |
| &nbsp; |Serviceregistratiesleutel |Deze sleutel wordt gegenereerd via de klassieke Azure Portal. | |
| &nbsp; |Gegevensversleutelingssleutel van service |Deze sleutel wordt gemaakt wanneer het apparaat via de Windows PowerShell voor StorSimple wordt geregistreerd bij de managementservice. Kopieer deze sleutel en bewaar deze op een veilige plaats. | |
|  | | | |
| **Minimale apparaatconfiguratie voltooien** |Beschrijvende naam voor uw apparaat |Dit is een beschrijvende naam voor het apparaat. | |
| &nbsp; |Tijdzone |Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen. | |
| &nbsp; |Secundaire DNS-server |Dit is een vereiste configuratie. | |
| &nbsp; |Netwerkinterface: Vaste IP's Data 0-controller |Deze IP moet routeerbaar zijn naar internet.</br>Vaste IP-adres Controller 0:</br>Vaste IP-adres Controller 1: | |
|  | | | |
| **Aanvullende netwerkinterface-instellingen** |Netwerkinterface: Data 1</br>Als iSCSI is ingeschakeld, hoeft u de gateway niet te configureren. |Doel: Cloud/iSCSI/Niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway: | |
| &nbsp; |Netwerkinterface: Data 2</br>Als iSCSI is ingeschakeld, hoeft u de gateway niet te configureren. |Doel: Cloud/iSCSI/Niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway: | |
| &nbsp; |Netwerkinterface: Data 3</br>Als iSCSI is ingeschakeld, hoeft u de gateway niet te configureren. |Doel: Cloud/iSCSI/Niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway: | |
| &nbsp; |Netwerkinterface: Data 4</br>Als iSCSI is ingeschakeld, hoeft u de gateway niet te configureren. |Doel: Cloud/iSCSI/Niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway: | |
| &nbsp; |Netwerkinterface: Data 5</br>Als iSCSI is ingeschakeld, hoeft u de gateway niet te configureren. |Doel: Cloud/iSCSI/Niet gebruikt</br>IP-adres:</br>Subnetmasker:</br>Gateway: | |
|  | | | |
| **Een volumecontainer maken** |Naam volumecontainer |Naam van de container | |
| &nbsp; |Azure Storage-account: |Opslagnaam en toegangssleutel die aan deze volumecontainer moet worden gekoppeld | |
| &nbsp; |Coderingssleutel voor cloudopslag: |Versleutelingssleutel voor opslag in elke container | |
|  | | | |
| **Een volume maken** |Details voor elk volume |Volumenaam: | |
| &nbsp; |&nbsp; |Grootte: | |
| &nbsp; |&nbsp; |Gebruikstype: | |
| &nbsp; |&nbsp; |ACR-naam: | |
| &nbsp; |&nbsp; |Standaardbeleid voor back-up: | |
|  | | | |
| **Een volume koppelen, initialiseren en formatteren** |Details voor elke hostserver die verbinding maakt met de opslag |Naam Windows Server: | |
| &nbsp; |&nbsp; |IQN Windows Server: | |
| &nbsp; |&nbsp; |Naam Windows Server-volume: | |
| &nbsp; |&nbsp; |NTFS-koppelpunt/-stationsletter: | |

## <a name="deployment-prerequisites"></a>Vereisten voor implementatie
In de volgende secties worden de configuratievereisten voor de StorSimple Manager-service en het StorSimple-apparaat toegelicht.

### <a name="for-the-storsimple-manager-service"></a>Voor de StorSimple Manager-service
Zorg voordat u begint voor het volgende:

* U hebt een Microsoft-account met toegangsreferenties.
* U hebt een Microsoft Azure Storage-account met toegangsreferenties.
* Uw Microsoft Azure-abonnement is ingeschakeld voor de StorSimple Manager-service. Uw abonnement moet zijn aangeschaft via de [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/).
* U hebt toegang tot terminalemulatiesoftware, zoals PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Voor het apparaat in het datacenter
Zorg voordat u het apparaat configureert voor het volgende:

* Het apparaat is volledig uitgepakt, gemonteerd op het rek en volledig bekabeld voor voeding, netwerk- en seriële toegang zoals is beschreven in:
  
  * [Een 8100-apparaat uitpakken, op het rek monteren en bekabelen](storsimple-8100-hardware-installation.md)
  * [Een 8600-apparaat uitpakken, op het rek monteren en bekabelen](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>Voor het netwerk in het datacenter
Zorg voordat u begint voor het volgende:

* De poorten in de firewall van het netwerkcentrum zijn geopend om iSCSI- en cloud-verkeer toe te staan, zoals is beschreven in [Netwerkvereisten voor uw StorSimple-apparaat](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

## <a name="step-by-step-deployment"></a>Stapsgewijze implementatie
Gebruik de volgende stapsgewijze instructies om uw StorSimple-apparaat te implementeren in het datacenter.

## <a name="step-1-create-a-new-service"></a>Stap 1: een nieuwe service maken
Met een StorSimple Manager-service kunt u meerdere StorSimple-apparaten beheren. Voer de volgende stappen uit om een nieuw exemplaar van de StorSimple Manager-service uit te voeren.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Als u de service niet hebt ingeschakeld om automatisch een opslagaccount te maken, moet u minimaal één opslagaccount maken nadat u een service hebt gemaakt. Dit opslagaccount wordt gebruikt bij het maken van een volumecontainer.
> 
> * Als u niet automatisch een opslagaccount hebt gemaakt, gaat u naar [Een nieuw opslagaccount voor de service configureren](#configure-a-new-storage-account-for-the-service) voor gedetailleerde instructies.
> * Als u het automatisch maken van een opslagaccount hebt ingeschakeld, gaat u naar [Stap 2: de serviceregistratiesleutel ophalen](#step-2-get-the-service-registration-key).
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Stap 2: de serviceregistratiesleutel ophalen
Wanneer de StorSimple Manager-service bedrijfsklaar is, moet u de serviceregistratiesleutel ophalen. Deze sleutel wordt gebruikt om het StorSimple-apparaat te registreren en te verbinden met de service.

Voer de volgende stappen uit in de klassieke Azure Portal.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Stap 3: het apparaat configureren en registreren via Windows PowerShell voor StorSimple
Gebruik Windows PowerShell voor StorSimple om de eerste installatie van uw StorSimple-apparaat uit te voeren zoals wordt uitgelegd in de volgende procedure. U moet terminalemulatiesoftware gebruiken om deze stap uit te voeren. Zie [PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat](#use-putty-to-connect-to-the-device-serial-console) voor meer informatie.

[!INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Stap 4: minimale apparaatconfiguratie voltooien
Voor de minimale apparaatconfiguratie van uw StorSimple-apparaat moet u het volgende doen:

* De secundaire DNS-server instellen
* ISCSI inschakelen op ten minste één netwerkinterface
* Vaste IP-adressen toewijzen aan beide domeincontrollers

Voer de volgende stappen in de klassieke Azure Portal uit om de minimale configuratie van het apparaat uit te voeren.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Stap 5: een volumecontainer maken
Een volumecontainer heeft opslagaccount, bandbreedte en versleutelingsinstellingen voor alle volumes in de container. U moet een volumecontainer maken voordat u de volumes op uw StorSimple-apparaat kunt gaan inrichten.

Voer de volgende stappen uit in de klassieke Azure Portal om een volumecontainer te maken.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Stap 6: een volume maken
Wanneer u een volumecontainer hebt gemaakt, kunt u een opslagvolume inrichten op het StorSimple-apparaat voor uw servers. Voer de volgende stappen uit in de klassieke Azure Portal om een volume te maken.

> [!IMPORTANT]
> Met StorSimple Manager kunt u alleen thin ingerichte volumes maken. U kunt geen volledig of gedeeltelijk ingerichte volumes maken.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Stap 7: een volume koppelen, initialiseren en formatteren
De volgende stappen worden uitgevoerd op uw Windows Server-host.

> [!IMPORTANT]
> * Voor hoge beschikbaarheid van uw StorSimple-oplossing wordt u aangeraden om de MPIO op de hostservers te configureren (optioneel) voordat u iSCSI configureert. MPIO-configuratie op hostservers zorgt ervoor dat de servers een koppelings-, netwerk- of interfacefout kunnen tolereren.
> * Voor MPIO- en iSCSI-installatie- en configuratie-instructies op een Windows Server-host gaat u naar [MPIO configureren voor uw StorSimple-apparaat](storsimple-configure-mpio-windows-server.md). Deze omvatten ook de stappen voor het koppelen, initialiseren en formatteren van StorSimple-volumes.
> * Voor MPIO- en iSCSI-installatie- en configuratie-instructies op een Linux-host gaat u naar [MPIO configureren voor uw StorSimple Linux-host](storsimple-configure-mpio-on-linux.md).
> 
> 

Als u besluit geen MPIO te configureren, voer dan de volgende stappen uit om uw StorSimple-volumes te koppelen, te initialiseren en te formatteren op een Windows Server-host.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Stap 8: een back-up maken
Back-ups bieden tijdgebonden bescherming van volumes en verbeteren de herstelmogelijkheden met minimale hersteltijden. U kunt twee soorten back-ups uitvoeren op een StorSimple-apparaat: lokale momentopnamen en cloudmomentopnamen. Beide back-uptypen kunnen **gepland** of **handmatig** zijn.

Voer de volgende stappen uit in de klassieke Azure Portal om een geplande back-up te maken.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

U kunt op elk moment een handmatige back-up maken. Voor procedures gaat u naar [Een handmatige back-up maken](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Een nieuw opslagaccount voor de service configureren
Dit is een optionele stap die u alleen hoeft uit te voeren als u het automatisch maken van een opslagaccount met uw service niet hebt ingeschakeld. U hebt een Microsoft Azure Storage-account nodig om een StorSimple-volumecontainer te maken.

Als u een Azure Storage-account in een andere regio wilt maken, raadpleeg dan [Over Azure Storage-accounts](../storage/common/storage-create-storage-account.md) voor stapsgewijze instructies.

Voer de volgende stappen uit op de pagina **StorSimple Manager-service** van de klassieke Azure Portal.

[!INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>PuTTY gebruiken om verbinding te maken met de seriële console van het apparaat
U moet terminalemulatiesoftware, zoals PuTTY, gebruiken om verbinding te maken met Windows PowerShell voor StorSimple. U kunt PuTTY gebruiken wanneer u het apparaat rechtstreeks benadert via de seriële console of door een Telnet-sessie te openen vanaf een externe computer.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Updates zoeken en toepassen
Het bijwerken van een apparaat kan enkele uren duren. Voer de volgende stappen uit als u wilt scannen op updates en deze wilt toepassen op uw apparaat.
<!--can take 1-4 hours-->

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Uw apparaat bijwerken
1. Klik op de pagina **Snel starten** van het apparaat op **Apparaten**. Selecteer het fysieke apparaat, klik op **Onderhoud** en vervolgens op **Updates zoeken**.  
2. Er wordt een taak gemaakt om te zoeken naar beschikbare updates. Als er updates beschikbaar zijn, verandert **Updates zoeken** in **Updates installeren**. Klik op **Updates installeren**.
3. Er wordt een updatetaak gemaakt. U bewaakt de status van de update door te navigeren naar **Taken**.
   
   > [!NOTE]
   > Wanneer de update start, wordt onmiddellijk de status als 50 procent weergegeven. De status verandert alleen in 100 procent wanneer de update is voltooid. Er is geen realtime-status voor het updateproces.
   > 
   > 
4. Wanneer het apparaat is bijgewerkt, schakelt u netwerkinterfaces Data 2 en Data 3 in als deze zijn uitgeschakeld.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Het IQN ophalen van een Windows Server-host
Voer de volgende stappen uit om de IQN (iSCSI Qualified Name) van een Windows-host te verkrijgen waarop Windows Server® 2012 wordt uitgevoerd.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Een handmatige back-up maken
Voer de volgende stappen uit in de klassieke Azure Portal als u voor één volume op het StorSimple-apparaat een handmatige back-up op aanvraag wilt maken.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>MPIO configureren
Multipath I/O (MPIO) is een optionele functie en wordt niet standaard op Windows Server geïnstalleerd. Het moet als een functie via Serverbeheer worden geïnstalleerd. Voor MPIO-installatie-instructies gaat u naar [MPIO configureren voor uw StorSimple-apparaat](storsimple-configure-mpio-windows-server.md) (Engelstalig).

Voor MPIO-installatie-instructies voor een StorSimple-apparaat die is verbonden met een Linux-host gaat u naar [MPIO configureren voor uw Linux-host](storsimple-configure-mpio-on-linux.md) (Engelstalig).

> [!NOTE]
> MPIO wordt niet ondersteund op een virtueel StorSimple-apparaat.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Configureer een [virtueel apparaat](storsimple-virtual-device-u2.md).
* Gebruik de [StorSimple Manager-service](storsimple-manager-service-administration.md) om uw StorSimple-apparaat te beheren.

