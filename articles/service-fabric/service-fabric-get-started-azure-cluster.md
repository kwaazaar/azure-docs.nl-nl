---
title: Een Azure Service Fabric-cluster instellen | Microsoft Docs
description: Quick Start - Maak een Service Fabric-cluster voor Windows of Linux in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: ryanwi
ms.openlocfilehash: facb9643e0bb848f0ea9aadf447f05af218fdd0f
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2017
---
# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Uw eerste Service Fabric-cluster maken in Azure
Een [Service Fabric-cluster](service-fabric-deploy-anywhere.md) is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Met deze Quick Start kunt u met behulp van [Azure PowerShell](https://msdn.microsoft.com/library/dn135248) of [Azure Portal](http://portal.azure.com) in slechts enkele minuten een cluster met vijf knooppunten maken voor Windows of Linux.  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Meld u bij Azure Portal aan via [http://portal.azure.com](http://portal.azure.com).

### <a name="create-the-cluster"></a>Het cluster maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Zoek naar **Service Fabric** en selecteer **Service Fabric-cluster** in **Service Fabric-cluster** in de geretourneerde resultaten.  Klik op **Create**.
3. Vul het Service Fabric-formulier **Basisinformatie** in. Bij **Besturingssysteem** selecteert u de versie van Windows of Linux waarop u de clusterknooppunten wilt uitvoeren. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Selecteer een **resourcegroep** of maak een nieuwe. Een resourcegroep is een logische container waarin Azure-resources worden gemaakt en waarin ze collectief worden beheerd. Na het voltooien klikt u op **OK**.

    ![Cluster-installatieuitvoer][cluster-setup-basics]

4. Vul het formulier **Clusterconfiguratie** in.  Voer voor **Aantal knooppunttype** de waarde '1' in.

5. Selecteer **Knooppunttype 1 (primair)** en vul het formulier **Configuratie van knooppunttypen** in.  Voer een naam voor het type knooppunt in en geef voor de laag [Duurzaamheid](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) de waarde 'Brons' op.  Selecteer een VM-grootte.

    Op basis van knooppunttypen worden de VM-grootte, het aantal virtuele machines, aangepaste eindpunten en andere instellingen voor de virtuele machines van elk type gedefinieerd. Elk gedefinieerd knooppunttype wordt ingesteld als een afzonderlijke VM-schaalset die wordt gebruikt voor het als set implementeren en beheren van virtuele machines. Elk knooppunttype kan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en verschillende capaciteitsstatistieken hebben.  Het eerste (primaire) knooppunttype is waar Service Fabric-systeemservices worden gehost. Dit type moet vijf of meer virtuele machines bevatten.

    Bij elke implementatie voor productie is [capaciteitsplanning](service-fabric-cluster-capacity.md) van groot belang.  Voor deze Quick Start voert u echter geen toepassingen uit, dus selecteert u de VM-grootte *DS1_v2 Standard*.  Selecteer Zilver als [betrouwbaarheidslaag](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) en selecteer een schaalset van 5 voor de virtuele machine.  

    Met aangepaste eindpunten opent u poorten in Azure Load Balancer, zodat u verbinding kunt maken met toepassingen die op het cluster worden uitgevoerd.  Voer '80, 8172' in om de poorten 80 en 8172 te openen.

    Schakel het selectievakje bij **Geavanceerde instellingen configureren** in. Deze instellingen hebben betrekking op het aanpassen van TCP/HTTP-beheereindpunten, poortbereiken voor toepassingen, [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) en [capaciteitseigenschappen](service-fabric-cluster-resource-manager-metrics.md).    

    Selecteer **OK**.

6. In het formulier **Clusterconfiguratie** stelt u **Diagnostische gegevens** in op **Aan**.  Voor deze Quick Start hoeft u geen eigenschappen op te geven voor de [infrastructuurinstelling](service-fabric-cluster-fabric-settings.md).  In **Infrastructuurversie** selecteert u de modus voor **automatisch** upgraden, zodat Microsoft de versie van de infrastructuurcode van het cluster automatisch bijwerkt.  Stel de modus in op **Handmatig** als u een [ondersteunde versie](service-fabric-cluster-upgrade.md) wilt kiezen om naar te upgraden. 

    ![Configuratie van knooppunttypen][node-type-config]

    Selecteer **OK**.

7. Vul het formulier **Beveiliging** in.  Voor deze Quick Start selecteert u **Onbeveiligd**.  Het wordt echter aanbevolen om een beveiligd cluster te maken voor productieworkloads, omdat iedereen anoniem verbinding kan maken met een onbeveiligd cluster en beheerbewerkingen kan uitvoeren.  

    Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie [Service Fabric-clusterbeveiligingsscenario's](service-fabric-cluster-security.md) voor meer informatie over hoe certificaten worden gebruikt in Service Fabric.  Als u gebruikersverificatie wilt inschakelen met Azure Active Directory of als u certificaten wilt instellen voor toepassingsbeveiliging, [maakt u een cluster op basis van een Resource Manager-sjabloon](service-fabric-cluster-creation-via-arm.md).

    Selecteer **OK**.

8. Bekijk de samenvatting.  Als u een Resource Manager-sjabloon wilt downloaden dat is gebaseerd op de instellingen die u hebt ingevoerd, selecteert u **Sjabloon en parameters downloaden**.  Selecteer **Maken** om het cluster te maken.

    U kunt de voortgang van het maken in de meldingen bekijken. (Klik op het belpictogram bij de statusbalk rechtsboven in uw scherm.) Als u tijdens het maken van het cluster op **Vastmaken aan Startboard** hebt geklikt, ziet u dat **Service Fabric-cluster implementeren** is vastgemaakt aan het **Startboard**.

### <a name="connect-to-the-cluster-using-powershell"></a>Verbinding maken met het cluster met behulp van PowerShell
Controleer met PowerShell of het cluster actief is.  De Service Fabric PowerShell-module wordt met de [Service Fabric-SDK](service-fabric-get-started.md) geïnstalleerd.  De cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) brengt een verbinding met het cluster tot stand.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint quickstartcluster.westus2.cloudapp.azure.com:19000
```
Zie [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) (Verbinding maken met een beveiligd cluster) voor meer voorbeelden van clusterverbindingen. Nadat u verbinding hebt gemaakt met het cluster, gebruikt u de cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) om een lijst weer te geven van de knooppunten in het cluster, evenals statusinformatie voor elk knooppunt. **HealthState** moet *OK* zijn voor elk knooppunt.

```powershell
PS C:\Users\sfuser> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion  ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  -----------  ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.7.198.9494 1                     Up 03:00:38   00:00:00              Ok
```

### <a name="remove-the-cluster"></a>Het cluster verwijderen
Een Service Fabric-cluster bevat de clusterresource zelf én andere Azure-resources. Als u een Service Fabric-cluster volledig wilt verwijderen, moet u dus ook alle resources waar het cluster uit bestaat, verwijderen. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen. Zie [Een cluster verwijderen](service-fabric-cluster-delete.md) voor andere manieren om een cluster of enkele (maar niet alle) resources in een resourcegroep te verwijderen

Een resourcegroep verwijderen in Azure Portal:
1. Ga naar het Service Fabric-cluster dat u wilt verwijderen.
2. Klik op de naam van de **resourcegroep** op de pagina met clusterbenodigdheden.
3. Klik op de pagina met **resourcegroepbenodigdheden** op **Resourcegroep verwijderen** en volg de instructies op die pagina om het verwijderen van de resourcegroep te voltooien.
    ![De resourcegroep verwijderen][cluster-delete]


## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
1. Download de [Azure Powershell-moduleversie 4.0 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) op de computer.

2. Voer de cmdlet [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/new-azurermservicefabriccluster) uit om een Service Fabric-cluster te maken met vijf knooppunten dat is beveiligd met een X.509-certificaat. Met de opdracht wordt een zelfondertekend certificaat gemaakt en geüpload naar een nieuwe sleutelkluis. Het certificaat wordt ook gekopieerd naar een lokale map. Stel de *-OS* parameter in om de versie van Windows of Linux die op de clusterknooppunten wordt uitgevoerd te kiezen. Pas de parameters zo nodig aan. 

    ```powershell
    #Provide the subscription Id
    $subscriptionId = 'yourSubscriptionId'

    # Certificate variables.
    $certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
    $certfolder="c:\mycertificates\"

    # Variables for VM admin.
    $adminuser="vmadmin"
    $adminpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force 

    # Variables for common values
    $clusterloc="SouthCentralUS"
    $clustername = "mysfcluster"
    $groupname="mysfclustergroup"       
    $vmsku = "Standard_D2_v2"
    $vaultname = "mykeyvault"
    $subname="$clustername.$clusterloc.cloudapp.azure.com"

    # Set the number of cluster nodes. Possible values: 1, 3-99
    $clustersize=5 

    # Set the context to the subscription ID where the cluster will be created
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Create the Service Fabric cluster.
    New-AzureRmServiceFabricCluster -Name $clustername -ResourceGroupName $groupname -Location $clusterloc `
    -ClusterSize $clustersize -VmUserName $adminuser -VmPassword $adminpwd -CertificateSubjectName $subname `
    -CertificatePassword $certpwd -CertificateOutputFolder $certfolder `
    -OS WindowsServer2016DatacenterwithContainers -VmSku $vmsku -KeyVaultName $vaultname
    ```

    Het duurt tussen de 10 en 30 minuten om de opdracht te voltooien. Als het goed is, wordt hierna ongeveer de volgende uitvoer weergegeven. De uitvoer bevat informatie over het certificaat, de KeyVault waarnaar dit is geüpload en de lokale map waarin het certificaat is gekopieerd.     

3. Kopieer de hele uitvoer en sla deze op in een tekstbestand, aangezien we deze later willen raadplegen. Noteer de volgende informatie uit de uitvoer. 

    - CertificateSavedLocalPath
    - CertificateThumbprint
    - ManagementEndpoint
    - ClientConnectionEndpointPort

### <a name="install-the-certificate-on-your-local-machine"></a>Het certificaat installeren op de lokale computer
  
Als u verbinding wilt maken met het cluster, moet u het certificaat installeren in de persoonlijke opslag (Mijn opslag) van de huidige gebruiker. 

Voer het volgende uit:

```powershell
$certpwd="Password#1234" | ConvertTo-SecureString -AsPlainText -Force
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\mycertificates\<certificatename>.pfx `
        -Password $certpwd
```

U bent nu klaar om verbinding te maken met het beveiligde cluster.

### <a name="connect-to-a-secure-cluster"></a>Verbinding maken met een beveiligd cluster 

Voer de cmdlet [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) uit om verbinding te maken met een beveiligd cluster. De details van het certificaat moeten overeenkomen met een certificaat dat is gebruikt om het cluster in te stellen. 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <ManagementEndpoint>:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <CertificateThumbprint> `
          -FindType FindByThumbprint -FindValue <CertificateThumbprint> `
          -StoreLocation CurrentUser -StoreName My
```

In het volgende voorbeeld worden voorbeeldparameters weergegeven: 

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint mycluster.southcentralus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -FindType FindByThumbprint -FindValue C4C1E541AD512B8065280292A8BA6079C3F26F10 `
          -StoreLocation CurrentUser -StoreName My
```

Voer de volgende opdracht uit om te controleren of u verbonden bent en of het cluster goed werkt.

```powershell

Get-ServiceFabricClusterHealth

```
### <a name="remove-the-cluster"></a>Het cluster verwijderen
Een cluster bevat de clusterresource zelf én andere Azure-resources. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen. 

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```
## <a name="use-azure-cli"></a>Azure CLI gebruiken

1. Installeer [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) op de computer.
2. Meld u aan bij Azure en selecteer het abonnement waarin u het cluster wilt maken.
   ```azurecli
   az login
   az account set --subscription <GUID>
   ```
3. Voer de opdracht [az sf cluster create](/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) uit om een Service Fabric-cluster te maken met vijf knooppunten dat is beveiligd met een X.509-certificaat. Met de opdracht wordt een zelfondertekend certificaat gemaakt en geüpload naar een nieuwe sleutelkluis. Het certificaat wordt ook gekopieerd naar een lokale map. Stel de *-os* parameter in om de versie van Windows of Linux die op de clusterknooppunten wordt uitgevoerd te kiezen. Pas de parameters zo nodig aan.

    ```azurecli
    #!/bin/bash

    # Variables
    ResourceGroupName="aztestclustergroup" 
    ClusterName="aztestcluster" 
    Location="southcentralus" 
    Password="q6D7nN%6ck@6" 
    Subject="aztestcluster.southcentralus.cloudapp.azure.com" 
    VaultName="aztestkeyvault" 
    VaultGroupName="testvaultgroup"
    VmPassword="Mypa$$word!321"
    VmUserName="sfadminuser"

    # Create resource groups
    az group create --name $ResourceGroupName --location $Location 
    az group create --name $VaultGroupName --location $Location

    # Create secure five node Linux cluster. Creates a key vault in a resource group
    # and creates a certficate in the key vault. The certificate's subject name must match 
    # the domain that you use to access the Service Fabric cluster.  The certificate is downloaded locally.
    az sf cluster create --resource-group $ResourceGroupName --location $Location --certificate-output-folder . \
        --certificate-password $Password --certificate-subject-name $Subject --cluster-name $ClusterName \
        --cluster-size 5 --os UbuntuServer1604 --vault-name $VaultName --vault-resource-group $VaultGroupName \
        --vm-password $VmPassword --vm-user-name $VmUserName
    ```
    
### <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster
Voer de volgende CLI-opdracht uit om verbinding met het cluster te maken met behulp van het certificaat.  Als u ter verificatie een clientcertificaat gebruikt, moeten de certificaatgegevens overeenkomen met een certificaat dat is geïmplementeerd in de clusterknooppunten.  Gebruik de optie `--no-verify` voor een zelfondertekend certificaat.

```azurecli
az sf cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 --pem ./linuxcluster201709161647.pem --no-verify
```

Voer de volgende opdracht uit om te controleren of u verbonden bent en of het cluster goed werkt.

```azurecli
az sf cluster health
```

### <a name="connect-to-the-nodes-directly"></a>Rechtstreeks verbinding maken met de knooppunten 

Als u verbinding wilt maken met de knooppunten in een Linux-cluster, kunt u SSH gebruiken en een poortnummer opgeven vanaf 3389. Voor bijvoorbeeld het cluster met vijf knooppunten dat eerder is gemaakt, zijn de opdrachten als volgt:
```bash
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3389
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3390
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3391
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3392
ssh sfadminuser@aztestcluster.southcentralus.cloudapp.azure.com -p 3393
```

### <a name="remove-the-cluster"></a>Het cluster verwijderen
Een cluster bevat de clusterresource zelf én andere Azure-resources. De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen. 

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen
Nu u een ontwikkelingscluster hebt ingesteld, kunt u het volgende proberen:
* [Een cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Een cluster verwijderen](service-fabric-cluster-delete.md) 
* [Apps implementeren met behulp van PowerShell](service-fabric-deploy-remove-applications.md)
* [Apps implementeren met behulp van CLI](service-fabric-application-lifecycle-sfctl.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
