---
title: Aanbevolen procedures voor automatisch schalen | Microsoft Docs
description: Patronen voor automatisch schalen in Azure voor Web-Apps, Virtual Machine-schaalsets en Cloud-Services
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 9fa2b94b-dfa5-4106-96ff-74fd1fba4657
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: ancav
ms.openlocfilehash: df5059b5509ca4989369cf3bcba8cb89f1c25db4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-autoscale"></a>Aanbevolen procedures voor Automatisch schalen
In dit artikel leert aanbevolen beveiligingsprocedures voor automatisch schalen in Azure. Monitor voor automatisch schalen die Azure is alleen bedoeld voor [virtuele-Machineschaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), en [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/). Andere Azure-services verschillende schalen methoden gebruiken.

## <a name="autoscale-concepts"></a>Concepten voor automatisch schalen
* Een resource kan alleen hebben *één* instelling voor automatisch schalen
* Een instelling voor automatisch schalen kan een of meer profielen en elk profiel zijn een of meer regels voor automatisch schalen.
* Een instelling voor automatisch schalen schaalt exemplaren horizontaal, namelijk *uit* doordat de exemplaren en *in* door te verlagen van het aantal exemplaren.
  Een instelling voor automatisch schalen heeft een maximale, minimale en standaardwaarde van exemplaren.
* Een taak voor automatisch schalen leest altijd de bijbehorende metriek te schalen, controleren of het de geconfigureerde drempelwaarde voor scale-out of de schaal is gepasseerd. U kunt een lijst weergeven van metrische gegevens die automatisch schalen kunt schalen door op [Azure Monitor automatisch schalen algemene metrische gegevens](insights-autoscale-common-metrics.md).
* Alle drempelwaarden zijn berekend op het niveau van een exemplaar. Bijvoorbeeld ' scale door 1 exemplaar wanneer gemiddelde CPU > 80% wanneer het aantal exemplaren is 2 ', scale-out betekent dat als de gemiddelde CPU in alle exemplaren groter dan 80 is %.
* U ontvangt altijd fout meldingen via e-mail. De eigenaar, bijdrager en lezers van de doelbron in het bijzonder ontvangen e-mail. U ontvangt ook altijd een *herstel* Stuur een e-mail wanneer automatisch schalen vanuit een fout herstelt en begint met het normaal functioneert.
* U kunt aanmelden voor het ontvangen van een geslaagde scale actie-melding via e-mail en webhooks.

## <a name="autoscale-best-practices"></a>Aanbevolen procedures voor automatisch schalen
Gebruik de volgende aanbevolen procedures als u automatisch schalen.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Zorg ervoor dat de maximale en minimale waarden zijn verschillend en een voldoende marge tussen deze twee hebben
Als u een instelling van minimaal hebt = 2, maximale = 2 en het huidige aantal exemplaren is 2 en er is geen schaalactie kan optreden. Houd een voldoende marge tussen het aantal maximale en minimale instanties, die inclusief zijn. Automatisch schalen schaalt altijd tussen deze limieten.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Handmatig schalen wordt opnieuw ingesteld door automatisch schalen min en max
Als u handmatig het aantal exemplaren op een waarde boven of onder de maximale bijwerkt, wordt de engine voor het automatisch schalen automatisch terug naar de minimale (indien hieronder) of het maximum (indien hierboven) aangepast. Bijvoorbeeld, instellen u het bereik tussen 3 en 6. Als u een actief exemplaar hebt, is de engine voor het automatisch schalen wordt aangepast aan 3 instanties op de volgende keer wordt uitgevoerd. Ook kan het zou schaal in back-exemplaren van 8 tot en met 6 op de volgende keer wordt uitgevoerd.  Handmatig schalen is zeer tijdelijk tenzij u ook de regels voor automatisch schalen opnieuw.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Gebruik altijd een combinatie van scale-out en schaal in regel waarmee een verhogen en de afname
Als u slechts één deel van de combinatie gebruikt, wordt automatisch schalen scale-dat in enkele uit of in, tot de maximum of minimum, bereikt.

### <a name="do-not-switch-between-the-azure-portal-and-the-azure-classic-portal-when-managing-autoscale"></a>Niet overschakelen tussen de Azure-portal en de klassieke Azure portal bij het beheren van automatisch schalen
Gebruik de Azure portal (portal.azure.com) maken en beheren van instellingen voor automatisch schalen voor Cloudservices en App-Services (Web-Apps). Gebruik PowerShell, CLI of REST-API maken en beheren van de instelling voor automatisch schalen voor virtuele-Machineschaalsets. Kan niet schakelen tussen de klassieke Azure portal (manage.windowsazure.com) en de Azure portal (portal.azure.com) bij het beheren van configuraties voor automatisch schalen. De klassieke Azure portal en de onderliggende back-end heeft beperkingen. Verplaatsen naar de Azure-portal voor het beheren van automatisch schalen met een grafische gebruikersinterface. De opties zijn om te gebruiken voor het automatisch schalen PowerShell, CLI of REST-API (via Azure Resource Explorer).

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Kies de juiste statistieken voor de metriek diagnostische gegevens
Voor de metrische gegevens voor diagnostische gegevens, kunt u kiezen uit *gemiddelde*, *Minimum*, *maximale* en *totale* als een waarde te schalen. De meest voorkomende statistieken *gemiddelde*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>De drempelwaarden voor alle metrische typen zorgvuldig kiezen
Het is raadzaam om verschillende drempelwaarden voor scale-out en schaal in op basis van praktische situaties zorgvuldig te kiezen.

We *wordt niet aanbevolen* automatisch schalen instellingen zoals de volgende voorbeelden zijn met de dezelfde of een vergelijkbaar drempelwaarden voor out en in situaties:

* Exemplaren verhoogd met 1 tellen wanneer aantal threads < = 600
* Exemplaren verlagen door 1 tellen wanneer aantal threads > = 600

Bekijk een voorbeeld van wat kan leiden tot een gedrag ontstaan. Houd rekening met de volgende reeks.

1. Stel dat u er allereerst zijn exemplaren van 2 en vervolgens het gemiddelde aantal threads per exemplaar uitbreiden tot 625.
2. Automatisch schalen uitgeschaald een 3e exemplaar toe te voegen.
3. Vervolgens wordt ervan uitgegaan dat het gemiddelde aantal threads in het exemplaar naar 575 valt.
4. Voordat het verkleinen, probeert automatisch schalen om in te schatten welke de definitieve status worden als deze aangepast. Voorbeeld: 575 x 3 (huidige aantal exemplaren) = 1,725 / 2 (definitieve aantal exemplaren als verkleind) = 862.5 threads. Dit betekent dat automatisch schalen zou moeten direct scale-out opnieuw zelfs nadat deze geschaald, als het gemiddelde aantal threads hetzelfde is gebleven of zelfs slechts een kleine hoeveelheid valt. Echter als deze opnieuw, voor het hele proces opgeschaald wilt herhalen, wat leidt tot een oneindige lus.
5. Om te voorkomen dat deze situatie ('flapping' genoemd), automatisch schalen niet kan worden uitgebreid naar beneden op alle. In plaats daarvan wordt overgeslagen en de voorwaarde reevaluates opnieuw zodra die de taak van de service wordt uitgevoerd. Dit kan veel mensen verwarrend omdat automatisch schalen lijkt wouldn't te werken, wanneer het gemiddelde aantal threads 575 is.

Schatting tijdens een scale-in is bedoeld om te voorkomen dat 'op en neer' situaties waarbij schaal in en scale-out acties voortdurend heen en weer gaat. Houd dit gedrag in rekening wanneer u ervoor de dezelfde drempelwaarden voor scale-out kiest en in.

U wordt aangeraden een voldoende marge tussen de scale-out en in de drempelwaarden te kiezen. Een voorbeeld kunt u de volgende betere regel combinatie.

* Exemplaren verhoogd met 1 tellen wanneer CPU-percentage > = 80
* Exemplaren verlagen door 1 tellen wanneer CPU-percentage < = 60

In dit geval  

1. Wordt ervan uitgegaan dat er 2 exemplaren worden gestart met zijn.
2. Als de gemiddelde CPU-percentage over exemplaren tot 80 gaat, schaalt automatisch schalen uit het toevoegen van een derde exemplaar.
3. Nu wordt ervan uitgegaan dat gedurende een bepaalde periode de CPU-percentage en 60 valt.
4. Automatisch schalen van schaal in regel maakt een schatting van de definitieve status te schalen in zijn. Voorbeeld: 60 x 3 (huidige aantal exemplaren) = 180 / 2 (definitieve aantal exemplaren als verkleind) = 90. Dus voor automatisch schalen biedt geen scale-in omdat het scale-out onmiddellijk opnieuw zou moeten. In plaats daarvan overgeslagen bij het verkleinen.
5. De volgende keer voor automatisch schalen controleert, blijft de CPU en 50 liggen. Schat opnieuw - exemplaar 50 x 3 = 150 / 2 exemplaren = 75, dat zich onder de drempelwaarde voor scale-out van 80, zodat het schalen is naar 2 exemplaren.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Overwegingen voor het schalen van drempelwaarden voor speciale metrische gegevens
 Voor speciale metrische gegevens zoals opslag- of Service Bus-wachtrij lengte metriek is de drempel voor het gemiddelde aantal berichten per huidige aantal exemplaren van beschikbaar. Kies zorgvuldig de drempelwaarde voor deze metrische gegevens.

Toegelicht de met een voorbeeld om te controleren of dat u het gedrag beter begrijpt.

* Exemplaren verhogen door 1 aantal wanneer Opslagwachtrij bericht count > = 50
* Exemplaren met 1 aantal verlagen als Opslagwachtrij mailbericht aantal < = 10

Houd rekening met de volgende volgorde:

1. Er zijn exemplaren van de wachtrij 2 opslag.
2. Berichten houden afkomstig is en wanneer u de opslagwachtrij bekijkt, het totale aantal 50 leest. U kunt wordt ervan uitgegaan dat automatisch schalen een scale-out-actie moet worden gestart. Echter, houd er rekening mee dat het nog steeds 50/2 = 25-berichten per exemplaar. Scale-out wordt dus niet uitgevoerd. Voor de eerste scale-out gebeurt, moet het totale aantal berichten in de opslagwachtrij 100.
3. Vervolgens wordt ervan uitgegaan dat het totale aantal berichten gelijk is aan 100.
4. Een 3e opslag wachtrij-exemplaar wordt toegevoegd als gevolg van een scale-out-actie.  De volgende actie van de scale-out gebeurt niet totdat het totale aantal berichten in de wachtrij 150 bereikt omdat 150/3 = 50.
5. Nu kleiner het aantal berichten in de wachtrij. Met 3 exemplaren de eerste actie schaal in gebeurt er wanneer het totale aantal berichten in alle wachtrijen maximaal 30 niet toevoegen omdat 30/3 = 10 berichten per instantie, die de drempelwaarde voor schaal.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Overwegingen voor het schalen van wanneer er meerdere profielen worden geconfigureerd in een instelling voor automatisch schalen
U kunt een standaard-profiel altijd zonder een afhankelijkheid op schema of tijd toegepast wordt, in een instelling voor automatisch schalen, of kunt u een terugkerende profiel of een profiel voor een bepaalde periode met een bereik van de datum en tijd.

Als de service automatisch schalen verwerkt, controleert deze altijd in de volgende volgorde:

1. Vaste datum profiel
2. Terugkerende profiel
3. Standaard-profiel ("Always")

Als een profiel voorwaarde wordt voldaan, wordt automatisch schalen niet de volgende voorwaarde profiel eronder gecontroleerd. Automatisch schalen verwerkt alleen één profiel tegelijk. Dit betekent dat als u ook een verwerkingsvoorwaarde van het profiel van een lagere lagen bevatten wilt, moet u opnemen deze regels ook in het huidige profiel.

Laten we Controleer dit met een voorbeeld:

De onderstaande afbeelding ziet u een instelling voor automatisch schalen met een standaardprofiel met minimale exemplaren = 2- en maximumaantal exemplaren = 10. In dit voorbeeld regels zijn geconfigureerd voor scale-out wanneer het aantal berichten in de wachtrij groter dan 10 is en schaal in wanneer het aantal berichten in de wachtrij minder dan 3 is. De resource kan nu worden geschaald tussen 2 en 10 exemplaren.

Er is bovendien een terugkerende profiel instellen voor maandag. Deze waarde is ingesteld voor minimale exemplaren = 2- en maximumaantal exemplaren = 12. Dit betekent dat op maandag, de eerste keer voor automatisch schalen voor deze voorwaarde controleert als het aantal exemplaren 2 is, wordt geschaald naar het nieuwe minimum van 3. Zolang automatisch schalen, blijft deze voorwaarde profiel vinden vergeleken (maandag), wordt alleen de regels worden verwerkt op basis van CPU scale-out/in voor dit profiel is geconfigureerd. Op dit moment wordt niet gecontroleerd voor de lengte van de wachtrij. Echter, als u wilt dat de wachtrij lengte voorwaarde moet worden gecontroleerd, moet u opnemen die regels uit het standaardprofiel ook in uw profiel maandag.

Op dezelfde manier als voor automatisch schalen terug naar de standaard-profiel schakelt, controleert eerst deze als de minimale en maximale voorwaarden wordt voldaan. Als het aantal exemplaren op het moment van 12 is, wordt het op 10, het toegestane maximum voor het standaardprofiel schalen.

![instellingen voor automatisch schalen](./media/insights-autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Overwegingen voor het schalen van wanneer meerdere regels zijn geconfigureerd in een profiel
Er zijn ook gevallen waarbij u moet mogelijk meerdere regels in een profiel instellen. De volgende set van regels voor automatisch schalen die worden gebruikt door services gebruikt wanneer meerdere regels worden ingesteld.

Op *uitschalen*, automatisch schalen die wordt uitgevoerd als alle regels is voldaan.
Op *schaal in*, automatisch schalen is vereist voor alle regels moet worden voldaan.

Ter illustratie, wordt ervan uitgegaan dat u de volgende 4 regels voor automatisch schalen hebt:

* Als CPU < 30% schaal in met 1
* Als geheugen < 50% schaal in met 1
* Als CPU > 75%, scale-out van 1
* Als geheugen > 75%, scale-out van 1

Vervolgens het volgende gebeurt:

* Als geheugen is 50% CPU is 76% we scale-out.
* Als is 50% van CPU en geheugen 76% we scale is-out.

Aan de andere kant als CPU 25% en geheugen 51% automatisch schalen biedt **niet** schaal in. Om scale-in, CPU moeten 29% en het geheugen 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Altijd een veilige standaardexemplaren selecteren
De standaardexemplaren is belangrijk voor automatisch schalen die uw service aan die telling wordt geschaald wanneer metrische gegevens niet beschikbaar zijn. Selecteer daarom een standaard-exemplaren die voor uw werkbelastingen veilig is.

### <a name="configure-autoscale-notifications"></a>Meldingen over automatisch schalen configureren
Automatisch schalen verwittigt beheerders en medewerkers van de resource via e-mail als een van de volgende condities optreedt:

* service voor automatisch schalen mislukt om een actie te ondernemen.
* Metrische gegevens zijn niet beschikbaar voor de service automatisch schalen om een scale-beslissing te nemen.
* Metrische gegevens zijn beschikbaar (herstelserver) opnieuw te maken van een beslissing schaal.
  Naast de bovenstaande voorwaarden kunt u e-mailadres of webhook meldingen Blijf op de hoogte voor geslaagde scale acties configureren.
  
U kunt ook een waarschuwing activiteitenlogboek gebruiken voor het bewaken van de status van de engine voor het automatisch schalen. Hier volgen voorbeelden aan [maken van een activiteit logboek-waarschuwing voor het bewaken van alle automatisch schalen engine bewerkingen voor uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) of [maken van een activiteit logboek-waarschuwing voor het bewaken van alle mislukte automatisch schalen schaal / uitschalen bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

## <a name="next-steps"></a>Volgende stappen
- [Maak een activiteit logboek-waarschuwing voor het bewaken van alle automatisch schalen engine bewerkingen voor uw abonnement.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Een activiteit logboek-waarschuwing voor het bewaken van alle mislukte automatisch schalen schaal / uitschalen bewerkingen op uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)
