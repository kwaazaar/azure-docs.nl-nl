---
title: Apparaat instellen voor de Azure IoT Hub apparaat inrichtingsservice | Microsoft Docs
description: Apparaat in te richten via de IoT Hub apparaat inrichten van Service tijdens het proces productie apparaat instellen
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7031409aa63f5d64d5bb7a1b9dcac50a97718630
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Een apparaat om in te richten via de Azure IoT Hub apparaat inrichtingsservice instellen

In de vorige zelfstudie hebt u geleerd hoe u de Azure IoT Hub apparaat inrichtingsservice instellen voor het automatisch inrichten van uw apparaten naar uw IoT-hub. Deze zelfstudie bevat richtlijnen voor het instellen van uw apparaat tijdens de productie, zodat u de Service voor het inrichten van apparaten voor uw apparaat op basis configureren kunt van de [Hardware Security Module (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security), en het apparaat kunt verbinding maken met uw mobiele apparaten inrichten service wanneer deze voor de eerste keer wordt opgestart. Deze zelfstudie worden de processen:

> [!div class="checklist"]
> * Selecteer een Hardware Security Module
> * Apparaat inrichten Client SDK bouwen voor de geselecteerde HSM
> * Pak de artefacten beveiliging
> * De configuratie van de inrichtingsservice apparaat op het apparaat instellen

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, maakt u uw apparaat inrichtingsservice-exemplaar en een iothub met behulp van de instructies die worden vermeld in de zelfstudie [cloud voor mobiele apparaten inrichten instellen](./tutorial-set-up-cloud.md).


## <a name="select-a-hardware-security-module"></a>Selecteer een Hardware Security Module

De [apparaat inrichtingsservice client SDK](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client) biedt ondersteuning voor twee soorten Hardware Security Modules (of HSM's): 

- [Trusted Platform Module (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module).
    - TPM is een vastgestelde norm voor de meeste Windows-apparaat-platforms, evenals enkele op basis van Linux/Ubuntu-apparaten. U kunt deze HSM als een fabrikant kiezen als u een van deze besturingssystemen uitvoeren op uw apparaten hebt, en als u zoekt een vastgestelde norm voor HSM's. U kunt alleen op elk apparaat afzonderlijk dat de Service voor het inrichten van apparaten inschrijven met TPM-chips. Voor ontwikkelingsdoeleinden, kunt u de simulator TPM op uw ontwikkelcomputer Windows of Linux.

- [X.509](https://cryptography.io/en/latest/x509/) op basis van hardware security modules. 
    - HSM's op basis van X.509 zijn relatief nieuwere chips, met werken op dit moment geen voortgang binnen Microsoft oproerbeheersing of OPDELEN chips die het X.509-certificaten te implementeren. U kunt uitvoeren met X.509-chips bulkinschrijving in de portal. Het ondersteunt ook bepaalde niet - Windows-besturingssystemen zoals embedOS. De client-SDK van de inrichtingsservice apparaat ondersteunt ontwikkeling hiertoe een X.509-apparaatsimulator. 

Als een fabrikant van apparaten die u wilt selecteren van hardware security modules/chips die zijn gebaseerd op een van de voorgaande typen. Andere typen van HSM's zijn momenteel niet ondersteund in de inrichtingsservice apparaat-client-SDK.   


## <a name="build-device-provisioning-client-sdk-for-the-selected-hsm"></a>Apparaat inrichten Client SDK bouwen voor de geselecteerde HSM

De Client SDK apparaat inrichten-Service helpt bij het implementeren van het geselecteerde beveiligingsmechanisme in software. De volgende stappen laten zien hoe de SDK wilt gebruiken voor de geselecteerde HSM-chip:

1. Als u hebt gevolgd de [Quick Start voor het gesimuleerde apparaat maken](./quick-create-simulated-device.md), hebt u de installatie gereed voor het bouwen van de SDK. Als dit niet het geval is, volgt u de eerste vier stappen in het gedeelte [voorbereiden van de ontwikkelomgeving](./quick-create-simulated-device.md#setupdevbox). Deze stappen kloon van de GitHub-opslagplaats voor de Client SDK apparaat inrichten-Service, evenals installeren de `cmake` hulpprogramma bouwen. 

1. De SDK bouwen voor het type HSM die u hebt geselecteerd voor uw apparaat met een van de volgende opdrachten op de opdrachtprompt:
    - Voor TPM-apparaten:
        ```cmd/sh
        cmake -Ddps_auth_type=tpm ..
        ```

    - Voor de TPM-simulator:
        ```cmd/sh
        cmake -Ddps_auth_type=tpm_simulator ..
        ```

    - Voor x.509-apparaten en simulator:
        ```cmd/sh
        cmake -Ddps_auth_type=x509 ..
        ```

1. De SDK biedt standaardondersteuning voor apparaten met Windows of Ubuntu implementaties voor TPM en x.509-HSM's. Voor deze HSM's ondersteund, gaat u verder naar het gedeelte [uitpakken van de beveiliging artefacten](#extractsecurity) hieronder. 
 
## <a name="support-custom-tpm-and-x509-devices"></a>Ondersteuning voor aangepaste TPM en x.509-apparaten

De Client SDK apparaat inrichten-systeem biedt geen standaardondersteuning voor TPM en X.509-apparaten waarop Windows of Ubuntu niet wordt uitgevoerd. Voor dergelijke apparaten moet u de aangepaste code schrijven voor uw specifieke HSM-chip, zoals wordt weergegeven in de volgende stappen uit:

### <a name="develop-your-custom-repository"></a>Uw aangepaste opslagplaats ontwikkelen

1. Ontwikkel een GitHub-opslagplaats voor toegang tot uw HSM. Dit project moet voor het produceren van een statische bibliotheek voor de apparaten inrichten SDK gebruiken.
1. De functies die zijn gedefinieerd in de volgende headerbestand moet worden geïmplementeerd in de bibliotheek: een. Voor aangepaste TPM implementeren in gedefinieerde functies `\azure-iot-sdk-c\dps_client\adapters\custom_hsm_tpm_impl.h`.
    b. Aangepaste X.509 implementeren in gedefinieerde functies `\azure-iot-sdk-c\dps_client\adapters\custom_hsm_x509_impl.h`. 
1. Uw HSM-opslagplaats moet ook bevatten een `CMakeLists.txt` bestand in de hoofdmap voor de opslagplaats die moet worden samengesteld.

### <a name="integrate-with-the-device-provisioning-service-client"></a>Integreren met het apparaat voor het inrichten van Client-Service

Zodra uw bibliotheek met succes is gebaseerd op een eigen, kunt u deze kunt verplaatsen naar de IoThub-C-SDK en ophalen in de opslagplaats:

1. De aangepaste HSM GitHub-opslagplaats, het bibliotheekpad en de naam ervan in de volgende cmake opdracht opgeven:
    ```cmd/sh
    cmake -Ddps_auth_type=<custom_hsm> -Ddps_hsm_custom_repo=<github_repo_name> -Ddps_hsm_custom_lib=<path_and_name_of library> <PATH_TO_AZURE_IOT_SDK>
    ```
   Vervang de `<custom_hsm>` in deze opdracht met een `tpm` of `x509`. Deze opdracht maakt u een markering voor de opslagplaats van uw aangepaste HSM binnen de `cmake` directory. Houd er rekening mee dat de aangepaste HSM nog moet worden gebaseerd op TPM of X.509 beveiligingsmechanismen.

1. De SDK openen in visual studio en bouw het. 

    - Het buildproces kloont de opslagplaats van aangepaste en de bibliotheek is gebaseerd.
    - De SDK probeert te koppelen op basis van de aangepaste HSM die is gedefinieerd in de opdracht cmake.

1. Voer de `\azure-iot-sdk-c\dps_client\samples\dps_client_sample\dps_client_sample.c` voorbeeld om te controleren of als uw HSM correct is geïmplementeerd.

<a id="extractsecurity"></a>
## <a name="extract-the-security-artifacts"></a>Pak de artefacten beveiliging

De volgende stap is om op te halen van de artefacten beveiliging voor de HSM op uw apparaat.

1. Voor een TPM-apparaat, moet u weten de **goedkeuringssleutel** gekoppeld aan van de TPM-chipfabrikant. U kunt een unieke afleiden **registratie-ID** voor uw apparaat TPM door de goedkeuringssleutel hashing. 
2. Voor een X.509-apparaat moet u de uitgegeven aan uw apparaten - eindentiteitscertificaten voor afzonderlijke apparaatinschrijvingen tijdens basiscertificaten voor groep inschrijvingen van apparaten certificaten verkrijgen.

Deze beveiliging artefacten zijn vereist voor het inschrijven van uw apparaten met de Service voor het inrichten van apparaat. De inrichting-service wordt er gewacht totdat een van deze apparaten starten en verbinding maken met het op een later tijdstip in de tijd. Zie [apparaatinschrijvingen beheren](how-to-manage-enrollments.md) voor informatie over het gebruik van deze artefacten beveiliging voor het maken van inschrijvingen. 

Wanneer uw apparaat voor het eerst wordt opgestart, wordt de SDK-client communiceert met uw chip uitpakken van de beveiliging artefacten van het apparaat en controleert of de registratie bij uw service apparaten inrichten. 


## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>De configuratie van de inrichtingsservice apparaat op het apparaat instellen

De laatste stap op het apparaat van de productie-proces is het schrijven van een toepassing die gebruikmaakt van de inrichtingsservice apparaat-client-SDK kunt u het apparaat te registreren met de service. Deze SDK biedt de volgende API's voor uw toepassingen te gebruiken:

```C
typedef void(*DPS_REGISTER_DEVICE_CALLBACK)(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* user_context); // Callback to notify user of device registration results.
DPS_CLIENT_LL_HANDLE DPS_Client_LL_Create (const char* dps_uri, const char* scope_id, DPS_TRANSPORT_PROVIDER_FUNCTION protocol, DPS_CLIENT_ON_ERROR_CALLBACK on_error_callback, void* user_ctx); // Creates the IOTHUB_DPS_LL_HANDLE to be used in subsequent calls.
void DPS_Client_LL_Destroy(DPS_CLIENT_LL_HANDLE handle); // Frees any resources created by the IoTHub Device Provisioning Service module.
DPS_RESULT DPS_LL_Register_Device(DPS_LL_HANDLE handle, DPS_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, DPS_CLIENT_REGISTER_STATUS_CALLBACK status_cb, void* status_ctx); // Registers a device that has been previously registered with Device Provisioning Service
void DPS_Client_LL_DoWork(DPS_LL_HANDLE handle); // Processes the communications with the Device Provisioning Service and calls any user callbacks that are required.
```

Houd er rekening mee te initialiseren van de variabelen `dps_uri` en `dps_scope_id` zoals vermeld in de [simuleren eerste opstartvolgorde voor de apparaat-sectie van dit snel starten](./quick-create-simulated-device.md#firstbootsequence), voordat u ze gebruikt. De registratie van mobiele apparaten inrichten client API `DPS_Client_LL_Create` maakt verbinding met de globale apparaat inrichtingsservice. De *bereik-ID* wordt gegenereerd door de service en wordt gegarandeerd dat uniekheid. Het is onveranderbaar en die wordt gebruikt om de registratie-id's uniek te identificeren. De `iothub_uri` kunt u de registratie van de IoT Hub client API `IoTHubClient_LL_CreateFromDeviceAuth` om te verbinden met de juiste IoT-hub. 


Deze API's kunnen uw apparaat verbinding maakt en registreren bij de Service voor het inrichten van apparaten wanneer deze wordt opgestart, de informatie over uw IoT-hub en maak verbinding met het. Het bestand `dps_client/samples/dps_client_sample/dps_client_sample.c` laat zien hoe deze API's gebruiken. In het algemeen moet u het volgende framework voor de clientregistratie van de te maken:

```C
static const char* dps_uri = "global.azure-devices-provisioning.net";
static const char* dps_scope_id = "[ID scope for your provisioning service]";
...
static void register_callback(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* context)
{
    USER_DEFINED_INFO* user_info = (USER_DEFINED_INFO *)user_context;
    ...
    user_info. reg_complete = 1;
}
static void registation_status(DPS_REGISTRATION_STATUS reg_status, void* user_context)
{
}
int main()
{
    ...    
    security_device_init(); // initialize your HSM 

    DPS_CLIENT_LL_HANDLE handle = DPS_Client_LL_Create(dps_uri, dps_scope_id, dps_transport, on_dps_error_callback, &user_info); // Create your DPS client

    if (DPS_Client_LL_Register_Device(handle, register_callback, &user_info, register_status, &user_info) == IOTHUB_DPS_OK) {
        do {
            // The dps_register_callback is called when registration is complete or fails
            DPS_Client_LL_DoWork(handle);
        } while (user_info.reg_complete == 0);
    }
    DPS_Client_LL_Destroy(handle); // Clean up the DPS client
    ...
    iothub_client = IoTHubClient_LL_CreateFromDeviceAuth(user_info.iothub_uri, user_info.device_id, transport); // Create your IoT hub client and connect to your hub
    ...
}
```

U kunt de inrichtingsservice apparaat registratie-clienttoepassing met behulp van een gesimuleerd apparaat aanvankelijk met behulp van een test-service-instellingen verfijnen. Als uw toepassing in de testomgeving werkt, kunt u samenstellen voor uw specifieke apparaat en het uitvoerbare bestand kopiëren naar de installatiekopie van uw apparaat. Het apparaat niet worden gestart, maar u moet [registreren van het apparaat met de Service voor het inrichten van apparaten](./tutorial-provision-device-to-hub.md#enrolldevice) vóór het starten van het apparaat. Zie de volgende stappen hieronder voor meer informatie over dit proces. 

## <a name="clean-up-resources"></a>Resources opschonen

Op dit punt wordt u mogelijk hebt ingesteld de services voor mobiele apparaten inrichten en IoT-Hub in de portal. Als u wilt afbreken van het apparaat setup inrichting en/of met behulp van een van deze services uit te stellen, wordt u aangeraden afgesloten om te vermijden onnodige kosten.

1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer uw Device Provisioning Service. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Selecteer een Hardware Security Module
> * Apparaat inrichten Client SDK bouwen voor de geselecteerde HSM
> * Pak de artefacten beveiliging
> * De configuratie van de inrichtingsservice apparaat op het apparaat instellen

Ga naar de volgende zelfstudie voor informatie over het inrichten van het apparaat naar uw IoT-hub door naar de Azure IoT Hub apparaat inrichtingsservice voor het inrichten van automatische inschrijving.

> [!div class="nextstepaction"]
> [Inrichten van het apparaat naar uw IoT-hub](tutorial-provision-device-to-hub.md)

