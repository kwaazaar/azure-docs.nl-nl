---
title: Json-voorbeeld Azure beleid - Audit server niveau detectie van dreigingen instellen | Microsoft Docs
description: Dit beleid json-voorbeeld audits waarschuwing beleidsregels voor de beveiliging van SQL database als deze beleidsregels zijn niet ingesteld op de opgegeven status.
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 0149e7b83de07b4a54ce9049a5ae52eb2152ab2c
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="audit-server-level-threat-detection-setting"></a>Detectie-serverinstelling niveau threat controleren

Dit beleid audits waarschuwing beleidsregels voor de beveiliging van SQL database als deze beleidsregels zijn niet ingesteld op de opgegeven status. U een waarde die aangeeft of de detectie van dreigingen is ingeschakeld of uitgeschakeld.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Voorbeeldsjabloon

[!code-json[main](../../../policy-templates/samples/SQL/audit-sql-server-threat-detection/azurepolicy.json "Audit Server level threat detection setting")]


U kunt implementeren met behulp van deze sjabloon de [Azure-portal](#deploy-with-the-portal), met [PowerShell](#deploy-with-powershell) of met de [Azure CLI](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Implementeren met portal

[![Implementeren in Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade)

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]



````powershell
$definition = New-AzureRmPolicyDefinition -Name "audit-sql-server-threat-detection" -DisplayName "Audit Server level threat detection setting" -description "Audit threat detection setting for SQL Server" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-server-threat-detection/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-server-threat-detection/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
````

### <a name="clean-up-powershell-deployment"></a>Opschonen van de PowerShell-implementatie

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```


## <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]



````cli

az policy definition create --name 'audit-sql-server-threat-detection' --display-name 'Audit Server level threat detection setting' --description 'Audit threat detection setting for SQL Server' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-server-threat-detection/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-server-threat-detection/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-sql-server-threat-detection"

````

### <a name="clean-up-azure-cli-deployment"></a>Opschonen van de implementatie van Azure CLI

Voer de volgende opdracht om de resourcegroep, VM en alle gerelateerde resources te verwijderen.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

- Voorbeelden van de sjabloon extra Azure-beleid, lopen [sjablonen voor Azure beleid](../json-samples.md).
