---
title: De beleidsmodule Azure Stack gebruiken | Microsoft Docs
description: Meer informatie over het beperken van een Azure-abonnement kan worden gebruikt als een Azure-Stack-abonnement
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 937ef34f-14d4-4ea9-960b-362ba986f000
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e505c52a5e1897d5626ee2cacce9fa3eff12fbbd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-policy-using-the-azure-stack-policy-module"></a>Azure met behulp van de beleidsmodule van de Azure-Stack-beleid beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De beleidsmodule van Azure Stack kunt u een Azure-abonnement configureren met de dezelfde versies en de beschikbaarheid van de service als Azure-Stack.  De module wordt gebruikt de **nieuw AzureRMPolicyAssignment** cmdlet voor het maken van een Azure-beleid, waardoor de resourcetypen en services die beschikbaar zijn in een abonnement.  Als u klaar is, kunt u uw Azure-abonnement kunt gebruiken voor het ontwikkelen van apps die zijn gericht voor Azure-Stack.  

## <a name="install-the-module"></a>De module installeren
1. Installeer de vereiste versie van de AzureRM PowerShell-module, zoals beschreven in stap 1 van [Installeer PowerShell voor Azure-Stack](azure-stack-powershell-install.md).   
2. [Download de Azure-Stack-tools van GitHub](azure-stack-powershell-download.md)  
3. [PowerShell configureren voor gebruik met Azure Stack](azure-stack-powershell-configure-user.md)

4. Importeer de module AzureStack.Policy.psm1:

   ```PowerShell
   Import-Module .\Policy\AzureStack.Policy.psm1
   ```

## <a name="apply-policy-to-subscription"></a>Beleid van toepassing op abonnement
De volgende opdracht kan worden gebruikt om toe te passen van een Azure-Stack standaardbeleid op basis van uw Azure-abonnement. Voordat u gaat werken, vervangt *de naam van de Azure-abonnement* met uw Azure-abonnement.

```PowerShell
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzureStackRmPolicy)
$subscriptionID = $s.Subscription.SubscriptionId
$rgName = 'AzureStack'
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID

```

## <a name="apply-policy-to-a-resource-group"></a>Beleid toepassen op een resourcegroep
U kunt beleidsregels in een meer gedetailleerd methode.  Als u bijvoorbeeld wellicht andere resources voor uitvoering in hetzelfde abonnement.  U kunt het bereik van de toepassing van beleid aan een specifieke resourcegroep, waarmee u uw apps testen voor Azure-resources met Azure-Stack. Voordat u gaat werken, vervangt *de naam van de Azure-abonnement* met de naam van uw Azure-abonnement.

```PowerShell
$resourceGroupName = ‘myRG01’
$s = Select-AzureRmSubscription -SubscriptionName "<Azure Subscription Name>"
$policy = New-AzureRmPolicyDefinition -Name AzureStackPolicyDefinition -Policy (Get-AzureStackRmPolicy)
New-AzureRmPolicyAssignment -Name AzureStack -PolicyDefinition $policy -Scope /subscriptions/$subscriptionID/resourceGroups/$rgName

```

## <a name="policy-in-action"></a>Beleid in actie
Zodra u het Azure-beleid hebt geïmplementeerd, u een foutbericht ontvangt bij het implementeren van een resource die niet toegestaan door het beleid.  

![Resultaat van de resource-implementatie is mislukt, omdat de beperking van beleid](./media/azure-stack-policy-module/image1.png)

## <a name="next-steps"></a>Volgende stappen
[Sjablonen implementeren met PowerShell](azure-stack-deploy-template-powershell.md)

[Sjablonen met Azure CLI implementeren](azure-stack-deploy-template-command-line.md)

[Sjablonen met Visual Studio implementeren](azure-stack-deploy-template-visual-studio.md)
