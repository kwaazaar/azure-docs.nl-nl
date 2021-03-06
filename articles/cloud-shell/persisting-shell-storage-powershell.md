---
title: Behouden van de bestanden in de Azure-Cloud-Shell (Preview) | Microsoft Docs
description: Overzicht van hoe Azure Cloud Shell zich blijft bestanden voordoen.
services: azure
documentationcenter: 
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: damaerte
ms.openlocfilehash: 933af3860bfe087a0b4db7eff53d4b978a1475da
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
[!include [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-cloud-shell-works"></a>De werking van Cloud-Shell
Cloud-Shell aanhoudt bestanden met de volgende methode: 
* Koppelen van de opgegeven bestandsshare als `clouddrive` in uw `$Home` map voor directe bestandsshare interactie.

## <a name="list-cloud-drive-file-shares"></a>Lijst met Cloud station bestandsshares
De `Get-CloudDrive` opdracht haalt de share-bestandsgegevens op dit moment zijn gekoppeld met de Cloud-station in de Cloud-Shell. <br>
![Get-CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-cloud-drive"></a>Cloud-station ontkoppelen
U kunt een bestandsshare die gekoppeld aan Cloud-Shell op elk gewenst moment ontkoppelen. Als de bestandsshare is verwijderd, wordt u gevraagd om te maken en koppelen van een nieuwe bestandsshare op de volgende sessie.

De `Dismount-CloudDrive` opdracht een bestandsshare ontkoppelt van het huidige opslagaccount. Ontkoppelen van het Cloud-station, wordt de huidige sessie beëindigd. De gebruiker wordt gevraagd om te maken en een nieuwe bestandsshare koppelen tijdens de volgende.
![Ontkoppeling CloudDrive uitgevoerd](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!include [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Volgende stappen
[Snelstartgids voor PowerShell](quickstart-powershell.md) <br>
[Meer informatie over Azure File storage](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Meer informatie over opslag labels](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>