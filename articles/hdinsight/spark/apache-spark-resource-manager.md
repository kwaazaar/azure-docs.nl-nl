---
title: Resources beheren voor Apache Spark-cluster in Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik beheren van resources voor Spark-clusters in Azure HDInsight voor betere prestaties.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 24f6ce9c22aff727ef597cd7bed0c15b260b8aa0
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Resources beheren voor Apache Spark-cluster in Azure HDInsight 

In dit artikel leert u hoe voor toegang tot de interfaces, zoals Ambari UI, YARN-gebruikersinterface en de Server van de geschiedenis van Spark die zijn gekoppeld aan uw Spark-cluster. U leert ook over het afstemmen van de clusterconfiguratie voor optimale prestaties.

**Vereisten:**

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-the-ambari-web-ui"></a>Hoe ik de Ambari-Webgebruikersinterface starten?
1. Klik vanaf het Startboard in [Azure Portal](https://portal.azure.com/) op de tegel voor uw Spark-cluster (als u deze aan het Startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.
2. Klik op voor uw Spark-cluster **Dashboard**. Wanneer u wordt gevraagd, voert u de beheerdersreferenties voor het Spark-cluster.

    ![Ambari starten](./media/apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "Start Resource Manager")
3. Hierdoor moet de Ambari-Webgebruikersinterface gestart, zoals weergegeven in de schermafbeelding.

    ![Ambari-webgebruikersinterface](./media/apache-spark-resource-manager/ambari-web-ui.png "Ambari-webgebruikersinterface")   

## <a name="how-do-i-launch-the-spark-history-server"></a>Hoe ik de geschiedenis van Spark Server starten?
1. Klik vanaf het Startboard in [Azure Portal](https://portal.azure.com/) op de tegel voor uw Spark-cluster (als u deze aan het Startboard hebt vastgemaakt).
2. De cluster-blade onder **snelkoppelingen**, klikt u op **Cluster-Dashboard**. In de **Cluster-Dashboard** blade, klikt u op **Spark geschiedenis Server**.

    ![Spark geschiedenis Server](./media/apache-spark-resource-manager/launch-history-server.png "Spark geschiedenis van Server")

    Wanneer u wordt gevraagd, voert u de beheerdersreferenties voor het Spark-cluster.

## <a name="how-do-i-launch-the-yarn-ui"></a>Hoe ik de gebruikersinterface van Yarn starten?
U kunt de gebruikersinterface van YARN gebruiken voor het bewaken van toepassingen die momenteel worden uitgevoerd op het Spark-cluster.

1. Klik in de cluster-blade op **Cluster-Dashboard**, en klik vervolgens op **YARN**.

    ![Gebruikersinterface van YARN starten](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > U kunt ook kunt u ook de gebruikersinterface van YARN van de Ambari UI starten. Start de UI Ambari van de cluster-blade, klikt u op **Cluster-Dashboard**, en klik vervolgens op **HDInsight-Cluster-Dashboard**. Klik op de UI Ambari **YARN**, klikt u op **snelkoppelingen**, klikt u op de actieve Resource Manager en klik vervolgens op **Resource Manager UI**.
   >
   >

## <a name="what-is-the-optimum-cluster-configuration-to-run-spark-applications"></a>Wat is de optimale clusterconfiguratie Spark-toepassingen uitvoeren?
De drie belangrijkste parameters die kunnen worden gebruikt voor de configuratie van Spark, afhankelijk van de toepassingsvereisten zijn `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een Executor is een proces gestart voor een Spark-toepassing. Deze wordt uitgevoerd op het werkrolknooppunt en is verantwoordelijk voor het uitvoeren van de taken voor de toepassing. Het standaardaantal Executor en de grootte van de executor voor elk cluster wordt berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze informatie wordt opgeslagen in `spark-defaults.conf` over de hoofdknooppunten van het cluster.

De drie configuratieparameters kunnen worden geconfigureerd op het clusterniveau van het (voor alle toepassingen die worden uitgevoerd op het cluster) of voor elke afzonderlijke toepassing ook worden opgegeven.

### <a name="change-the-parameters-using-ambari-ui"></a>Wijzig de parameters met Ambari UI
1. Via de Ambari UI Klik hier **Spark**, klikt u op **Contigs**, en vouw vervolgens **aangepaste spark-standaardwaarden**.

    ![Parameters instellen met Ambari](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. De standaardwaarden zijn goede 4 Spark-toepassingen gelijktijdig worden uitgevoerd op het cluster hebben. U kunt deze waarden wijzigen via de gebruikersinterface, zoals hieronder wordt weergegeven.

    ![Parameters instellen met Ambari](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Klik op **opslaan** opslaan van wijzigingen in de configuratie. U wordt gevraagd aan de bovenkant van de pagina alle betrokken services opnieuw starten. Klik op **opnieuw**.

    ![Services opnieuw starten](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Wijzig de parameters voor een toepassing die wordt uitgevoerd in de Jupyter-notebook
Voor toepassingen die worden uitgevoerd in de Jupyter-notebook, kunt u de `%%configure` magic de configuratiewijzigingen aanbrengen. In het ideale geval moet u deze wijzigingen aanbrengen aan het begin van de toepassing, voordat u uw eerste codecel uitvoert. Dit zorgt ervoor dat de configuratie is toegepast op de sessie Livy, wanneer deze wordt gemaakt. Als u wijzigen van de configuratie op een later stadium in de toepassing wilt, moet u de `-f` parameter. Echter, door dit doet, alle uitgevoerd in de toepassing niet verloren.

Het onderstaande codefragment laat zien hoe de configuratie voor een toepassing die wordt uitgevoerd in Jupyter wijzigt.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Parameters voor de configuratie moeten worden doorgegeven als een JSON-tekenreeks en moeten op de volgende regel na de magic, zoals wordt weergegeven in de voorbeeldkolom.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Wijzig de parameters voor een toepassing die wordt verzonden met spark verzenden
Na de opdracht is een voorbeeld van het wijzigen van de configuratieparameters voor een batch-toepassing die wordt verzonden met behulp van `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Wijzig de parameters voor een toepassing die wordt verzonden met cURL
Na de opdracht is een voorbeeld van hoe de configuratieparameters voor een batch-toepassing die wordt verzonden met behulp van cURL wijzigen.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Hoe kan ik deze parameters op een Spark Thrift-Server wijzigen?
Spark Thrift-Server JDBC/ODBC toegang biedt tot een Spark-cluster en wordt gebruikt voor service Spark SQL-query's. Hulpprogramma's zoals Power BI, Tableau enzovoort. ODBC-protocol gebruiken om te communiceren met Spark Thrift-Server Spark SQL-query's uitvoeren als een Spark-toepassing. Wanneer een Spark-cluster is gemaakt, worden er twee exemplaren van de Spark Thrift-Server gestart, één op elke hoofdknooppunt. Elke Spark Thrift-Server wordt weergegeven als een Spark-toepassing in de gebruikersinterface van YARN.

Spark dynamische executor toewijzing maakt gebruik van Spark Thrift-Server en wordt daarmee de `spark.executor.instances` wordt niet gebruikt. In plaats daarvan Spark Thrift-Server gebruikt `spark.dynamicAllocation.minExecutors` en `spark.dynamicAllocation.maxExecutors` om op te geven van het aantal executor. De configuratieparameters `spark.executor.cores` en `spark.executor.memory` wordt gebruikt om de grootte van de executor te wijzigen. U kunt deze parameters kunt wijzigen, zoals wordt weergegeven in de volgende stappen uit.

* Vouw de **geavanceerde spark-thrift-sparkconf** categorie om bij te werken van de parameters `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, en `spark.executor.memory`.

    ![Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Vouw de **aangepaste spark thrift sparkconf** categorie om bij te werken met de parameter `spark.executor.cores`.

    ![Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-the-driver-memory-of-the-spark-thrift-server"></a>Hoe wijzig ik de stuurprogramma-geheugen van de Spark Thrift-Server?
Geheugen Spark Thrift-Server is geconfigureerd 25% van het hoofdknooppunt RAM-geheugen, de totale grootte van het RAM-geheugen van het hoofdknooppunt is groter dan 14GB opgegeven. Zoals hieronder wordt weergegeven, kunt u de Ambari UI wijzigen van de configuratie van het stuurprogramma-geheugen.

* Via de Ambari UI Klik hier **Spark**, klikt u op **Configs**, vouw **geavanceerde spark env**, en geef vervolgens de waarde voor **spark_thrift_cmd_opts**.

    ![RAM-geheugen voor Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-the-resources-back"></a>Ik gebruik geen BI met Spark-cluster. Hoe ik de resources weer uitvoeren?
Aangezien we de dynamische toewijzing Spark gebruiken, zijn de enige resources die worden verbruikt door thrift-server de resources voor de toepassing twee modellen. Als u wilt vrijmaken van deze resources moet u de Thrift-Server-services uitgevoerd op het cluster stoppen.

1. Klik in de UI Ambari in het linkerdeelvenster op **Spark**.
2. Klik in de volgende pagina op **Spark Thrift Servers**.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. U ziet de twee headnodes waarop de Spark Thrift-Server wordt uitgevoerd. Klik op een van de headnodes.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. De volgende pagina geeft een lijst van alle services die worden uitgevoerd op die headnode. Klik op de knop omlaag naast de Spark Thrift-Server in de lijst en klik vervolgens op **stoppen**.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Herhaal deze stappen uit op de andere headnode ook.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-the-service"></a>Mijn Jupyter-notebooks niet worden uitgevoerd zoals verwacht. Hoe kan ik de service opnieuw starten?
Start de Ambari-Webgebruikersinterface zoals hierboven. Klik in het navigatiedeelvenster links op **Jupyter**, klikt u op **serviceacties**, en klik vervolgens op **start opnieuw alle**. Hiermee start u de Jupyter-service op de headnodes.

    ![Restart Jupyter](./media/apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Hoe weet ik of mijn onvoldoende resources werkt systeem?
Start de gebruikersinterface van Yarn zoals hierboven. In de tabel van de Cluster metrische gegevens boven op het scherm, controleert u de waarden van **geheugen gebruikt** en **Totaal geheugen** kolommen. Als de waarden 2 bijna, er mogelijk niet voldoende resources om de volgende toepassing te starten. Hetzelfde geldt voor de **VCores gebruikt** en **VCores totaal** kolommen. Ook in de hoofdweergave als er een toepassing gebleven **GEACCEPTEERDE** status en niet in een overgang **met** noch **mislukt** staat, dit kan ook wel een indicatie dat het is niet ophalen van onvoldoende bronnen om te starten.

    ![Resource Limit](./media/apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-to-free-up-resource"></a>Hoe ik een actieve toepassing vrijmaken resource kill?
1. Klik in de gebruikersinterface van Yarn, in het linkerpaneel **met**. In de lijst van actieve toepassingen, bepalen de toepassing worden verwijderd en klik op de **ID**.

    ![Kill App1](./media/apache-spark-resource-manager/kill-app1.png "App1 afsluiten")

2. Klik op **toepassing Kill** op de rechterbovenhoek, klik vervolgens op **OK**.

    ![Kill App2](./media/apache-spark-resource-manager/kill-app2.png "App2 afsluiten")

## <a name="see-also"></a>Zie ook
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Voor gegevensanalisten

* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analyse van Application Insights-telemetriegegevens met behulp van Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Gebruik Caffe op Azure HDInsight Spark voor gedistribueerde grondige learning](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Voor Spark-ontwikkelaars

* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](apache-spark-eventhub-streaming.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)
