1. In een nieuw browservenster, moet u zich aanmelden bij de [Azure-portal](https://portal.azure.com/).
2. Klik op **nieuwe** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. In de **nieuwe account** pagina, voert u de instellingen voor de nieuwe Azure DB die Cosmos-account. 
 
    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Voer een unieke naam*|Voer een unieke naam voor dit account Azure Cosmos DB. Omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken, gebruikt u een unieke maar identificeerbare id.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|SQL (DocumentDB)|De API bepaalt het type account te maken. Azure Cosmos DB biedt vier API's voor tegemoetkomt aan de behoeften van uw toepassing: Gremlin (grafiek), MongoDB, SQL (DocumentDB) en tabel (sleutel-waarde), elke die momenteel vereisen een afzonderlijk account. <br><br>Selecteer **SQL (DocumentDB)** omdat deze snelstartgids u maakt in een documentdatabase die is waarop SQL-syntaxis gebruiken.<br><br>[Meer informatie over de DocumentDB-API](../articles/cosmos-db/documentdb-introduction.md)|
    Abonnement|*Uw abonnement*|Selecteer de Azure-abonnement dat u wilt gebruiken voor dit account Azure Cosmos DB. 
    Resourcegroep|*Voer de dezelfde unieke naam zoals hierboven omschreven in-ID*|Voer een nieuwe naam resourcegroep voor uw account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*Selecteer de regio die het dichtst bij uw gebruikers*|Selecteer de geografische locatie op waar voor het hosten van uw Azure DB die Cosmos-account. De locatie die zich het dichtst bij hen de snelste toegang geven tot de gegevens van uw gebruikers gebruiken.
    Geografische redundantie inschakelen| Leeg laten | Hiermee maakt u een gerepliceerde versie van uw database in een tweede (gekoppelde) regio. Laat dit leeg.  
    Vastmaken aan dashboard | Selecteer | Selecteer dit selectievakje in zodat uw nieuwe databaseaccount wordt toegevoegd aan uw portaldashboard voor eenvoudige toegang.

    Klik vervolgens op **Maken**.

    ![De blade Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

4. Het maken van een account duurt enkele minuten duren. Account tijdens het maken van de portal geeft de **implementeren Azure Cosmos DB** tegel.

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount/deploying-cosmos-db.png)

    Zodra het account is gemaakt, de **Gefeliciteerd! Uw Azure DB die Cosmos-account is gemaakt** pagina wordt weergegeven. 

