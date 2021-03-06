---
title: Verplaatsen van een openbare peering op Azure ExpressRoute voor Microsoft-peering | Microsoft Docs
description: Dit artikel ziet u de stappen voor het verplaatsen van uw openbare peering naar Microsoft in ExpressRoute-peering.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2017
ms.author: cherylmc
ms.openlocfilehash: 311e1de3200cd684bbec1329ebd5217b4fb3a2e2
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Verplaatsen van een openbare peering voor het Microsoft-peering

ExpressRoute ondersteunt nu Azure PaaS-services, zoals Azure storage en Azure SQL Database, met behulp van Microsoft-peering met routefilters. U moet nu slechts één routeringsdomein voor toegang tot Microsoft PaaS en SaaS-services. U kunt profiteren van routefilters selectief adverteren de voorvoegsels PaaS-service voor Azure-regio's die u wilt gebruiken.

In dit artikel helpt u bij de configuratie van een openbare peering verplaatsen naar Microsoft-peering zonder uitvaltijd. Zie voor meer informatie over routering domeinen en peerings [ExpressRoute-circuits en Routeringsdomeinen](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> U moet de invoegtoepassing ExpressRoute premium hebben om te gebruiken Microsoft-peering. Zie voor meer informatie over de premium-invoegtoepassing, de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md#expressroute-premium).

## <a name="before-you-begin"></a>Voordat u begint

* Voor verbinding met Microsoft-peering, moet u instellen en beheren van NAT bevinden. Uw connectiviteitsprovider kan instellen en beheren van de NAT als een beheerde service. Als u van plan bent voor toegang tot de Azure-PaaS en Azure SaaS-services op Microsoft-peering, is het belangrijk dat u het formaat van het NAT IP-adresgroep correct. Zie voor meer informatie over de NAT voor ExpressRoute, de [NAT-vereisten voor Microsoft-peering](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Als u momenteel een netwerk toegangsbeheerlijst (ACL) voor de eindpunten van de service Azure PaaS hebt bij gebruik van openbare Azure-peering u ervoor zorgen dat het NAT IP van toepassingen is geconfigureerd moet met is Microsoft-peering opgenomen in de access control list geconfigureerd voor de service het eindpunt.

Als u wilt verplaatsen naar de Microsoft-peering zonder uitvaltijd, moet u de stappen in dit artikel in de volgorde waarin ze worden weergegeven.

## <a name="1-create-microsoft-peering"></a>1. Microsoft-peering maken

Als Microsoft-peering niet gemaakt is, kunt u een van de volgende artikelen voor het maken van de Microsoft-peering gebruiken. Als uw connectiviteit provider aanbiedingen beheerde laag-3-services, kunt u de connectiviteitsprovider om in te schakelen van Microsoft-peering voor uw circuit vragen.

  * [Microsoft-peering met Azure portal maken](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Microsoft-peering met Azure Powershell maken](expressroute-howto-routing-arm.md#msft)
  * [Microsoft-peering met Azure CLI maken](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a>2. Valideren van Microsoft-peering is ingeschakeld

Controleer of de Microsoft-peering is ingeschakeld en de aangekondigde openbare voorvoegsels zijn in de geconfigureerde status.

  * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a>3. Configureren en een routefilter koppelen aan het circuit

Standaard komen nieuwe Microsoft-peerings niet alle voorvoegsels adverteren totdat een routefilter is gekoppeld aan het circuit. Wanneer u een route filterregel maakt, kunt u de lijst met service-community's voor Azure-regio's die u gebruiken voor Azure PaaS-services, wilt zoals wordt weergegeven in de volgende schermafbeelding:

![Openbare peering samenvoegen](.\media\how-to-move-peering\public.png)

Gebruik de volgende artikelen om routefilters te configureren.

  * [Routefilters voor Microsoft-peering met Azure portal configureren](how-to-routefilter-portal.md)
  * [Routefilters voor Microsoft-peering met Azure PowerShell configureren](how-to-routefilter-powershell.md)
  * [Routefilters voor Microsoft-peering met Azure CLI configureren](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a>4. De openbare peering verwijderen

Nadat u hebt gecontroleerd of de Microsoft-peering is geconfigureerd en de voorvoegsels die u wilt gebruiken op het Microsoft-peering voor het correct worden geadverteerd, kunt u vervolgens de openbare peering verwijderen. Als u wilt verwijderen openbare peering, gebruikt u een van de volgende artikelen:

  * [Openbare Azure-peering met Azure portal verwijderen](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Verwijderen van openbare Azure-peering met Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)
  * [Verwijderen van openbare Azure-peering met CLI](howto-routing-cli.md#deletepublic)

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).