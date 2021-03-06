---
title: 'Zelfstudie: Azure Active Directory-integratie met ADP eTime | Microsoft Docs'
description: Informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ADP eTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 31fed307a32e629d00aab7cc9d5167ee16d83936
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Zelfstudie: Azure Active Directory-integratie met ADP eTime

In deze zelfstudie leert u hoe ADP eTime integreren met Azure Active Directory (Azure AD).

ADP eTime integreren met Azure AD biedt de volgende voordelen:

- U kunt beheren in Azure AD die toegang tot ADP eTime heeft
- U kunt uw gebruikers automatisch ophalen aangemeld bij ADP eTime (Single Sign-On) met hun Azure AD-accounts inschakelen
- U kunt uw accounts op één centrale locatie - en de Azure-portal beheren

Als u weten van meer informatie over de integratie van de SaaS-app met Azure AD wilt, Zie [wat is er toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ADP eTime, moet u de volgende items:

- Een Azure AD-abonnement
- Een ADP eTime eenmalige aanmelding ingeschakeld abonnement

> [!NOTE]
> Test de stappen in deze zelfstudie, raden we niet met behulp van een productieomgeving.

Test de stappen in deze zelfstudie, moet u deze aanbevelingen volgen:

- Gebruik niet uw productieomgeving, tenzij het noodzakelijk is.
- Als u geen een proefabonnement Azure AD-omgeving hebt, kunt u een proefversie van één maand [hier](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeschrijving
In deze zelfstudie test u Azure AD eenmalige aanmelding in een testomgeving. Het scenario in deze zelfstudie bestaat uit twee belangrijkste bouwstenen:

1. ADP eTime uit de galerie toevoegen
2. Configureren en testen van Azure AD eenmalige aanmelding

## <a name="adding-adp-etime-from-the-gallery"></a>ADP eTime uit de galerie toevoegen
Voor het configureren van de integratie van ADP eTime in Azure AD, moet u ADP eTime uit de galerie toevoegen aan de lijst met beheerde SaaS-apps.

**Als u wilt toevoegen ADP eTime uit de galerie, moet u de volgende stappen uitvoeren:**

1. In de  **[Azure-portal](https://portal.azure.com)**, klik in het linkernavigatievenster op **Azure Active Directory** pictogram. 

    ![Active Directory][1]

2. Navigeer naar **bedrijfstoepassingen**. Ga vervolgens naar **alle toepassingen**.

    ![Toepassingen][2]
    
3. Om de nieuwe toepassing toevoegen, klikt u op **nieuwe toepassing** knop boven aan het dialoogvenster.

    ![Toepassingen][3]

4. Typ in het zoekvak **ADP eTime**.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. Selecteer in het deelvenster resultaten **ADP eTime**, en klik vervolgens op **toevoegen** om toe te voegen van de toepassing.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configureren en testen van Azure AD eenmalige aanmelding
In deze sectie kunt u configureren en testen eenmalige aanmelding Azure AD met ADP eTime op basis van een testgebruiker genaamd "Britta Simon."

Voor eenmalige aanmelding werkt, moet Azure AD weten wat de gebruiker equivalent in ADP eTime is voor een gebruiker in Azure AD. Met andere woorden, moet een koppeling relatie tussen een Azure AD-gebruiker en de betreffende gebruiker in ADP eTime tot stand worden gebracht.

Deze relatie koppeling wordt ingesteld door het toewijzen van de waarde van de **gebruikersnaam** in Azure AD als de waarde van de **gebruikersnaam** in ADP eTime.

Om te configureren en testen van Azure AD eenmalige aanmelding met ADP eTime, moet u de volgende bouwstenen voltooien:

1. **[Configureren van Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  : als u wilt dat uw gebruikers kunnen deze functie gebruiken.
2. **[Maken van een Azure AD-testgebruiker](#creating-an-azure-ad-test-user)**  - voor het testen van Azure AD eenmalige aanmelding met Britta Simon.
3. **[Maken van een gebruiker ADP eTime test](#creating-an-adp-etime-test-user)**  - hebben een equivalent van Britta Simon in ADP eTime die is gekoppeld aan de Azure AD-weergave van de gebruiker.
4. **[Toewijzen van de Azure AD-testgebruiker](#assigning-the-azure-ad-test-user)**  - Britta Simon gebruik van Azure AD eenmalige aanmelding inschakelen.
5. **[Testen van eenmalige aanmelding](#testing-single-sign-on)**  : om te controleren of de configuratie werkt.

### <a name="configuring-azure-ad-single-sign-on"></a>Eenmalige aanmelding Azure AD configureren

In dit gedeelte Azure AD eenmalige aanmelding inschakelen in de Azure portal en eenmalige aanmelding configureren in uw ADP eTime-toepassing.

**Voor het configureren van Azure AD eenmalige aanmelding met ADP eTime, moet u de volgende stappen uitvoeren:**

1. In de Azure-portal op de **ADP eTime** toepassing Integratiepagina, klikt u op **eenmalige aanmelding**.

    ![Eenmalige aanmelding configureren][4]

2. Op de **eenmalige aanmelding** dialoogvenster Selecteer **modus** als **op basis van SAML aanmelding** voor eenmalige aanmelding inschakelen.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. Op de **ADP eTime domein en URL's** sectie, voert u de volgende stap:

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    a. In de **id** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.adp.com`

    b. In de **antwoord-URL** textbox, typ een URL met het volgende patroon volgen:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer` 
 
    > [!NOTE] 
    > Deze waarden zijn niet de werkelijke. Deze waarden bijwerken met het werkelijke antwoord-URL en de id. Neem contact op met [ADP eTime ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) ophalen van deze waarden.

4. Op de **SAML-certificaat voor ondertekening van** sectie, klikt u op **Metadata XML** en sla het XML-bestand op uw computer.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. De toepassing van de eTime ADP verwacht de SAML-asserties in een specifieke indeling waarvoor u aangepaste kenmerktoewijzingen toevoegen aan uw configuratie van SAML-token kenmerken. De volgende Schermafbeelding toont een voorbeeld voor deze. De naam van de claim wordt altijd worden **'PersonImmutableID'** en de waarde die we hebben toegewezen aan ExtensionAttribute2 waarin de werknemer-id van de gebruiker. 

    Hier wordt de gebruiker toewijzen van Azure AD aan ADP eTime worden uitgevoerd op de werknemer-id maar u kunt dit toewijzen aan een andere waarde ook op basis van de toepassingsinstellingen van uw. Dus neem werk met [ADP eTime ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) eerst voor het gebruik van de juiste id van een gebruiker en wijs die waarde met de **'PersonImmutableID'** claim.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. In de **gebruikerskenmerken** sectie op de **eenmalige aanmelding** dialoogvenster SAML-token kenmerk configureren zoals wordt weergegeven in de afbeelding en de volgende stappen uitvoeren:
    
    | Naam van kenmerk | De waarde van kenmerk |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.extensionattribute2 |
    
    a. Klik op **toevoegen kenmerk** openen de **kenmerk toevoegen** dialoogvenster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    b. In de **naam** textbox, typ de naam van het kenmerk wordt weergegeven voor die rij.

    c. Van de **waarde** typt u de waarde van het kenmerk wordt weergegeven voor die rij.
    
    d. Klik op **OK**.

    > [!NOTE] 
    > Voordat u de SAML-bevestiging configureren kunt, moet u contact op met uw [ADP eTime ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) en vraagt u de waarde van het kenmerk unieke id voor uw tenant. U moet deze waarde voor het configureren van de aangepaste claim voor uw toepassing. 

7. Op de **ADP eTime configuratie** sectie, klikt u op **ADP configureren eTime** openen **eenmalige aanmelding configureren** venster.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. Klik op **opslaan** knop.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. Eenmalige aanmelding configureren op **ADP eTime** zijde, moet u de gedownloade verzenden **Metadata XML** naar [ADP eTime ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx). 

> [!TIP]
> U kunt nu een beknopte versie van deze instructies binnen lezen de [Azure-portal](https://portal.azure.com), terwijl u de app instelt!  Na het toevoegen van deze app uit de **Active Directory > bedrijfstoepassingen** sectie, klikt u op de **Single Sign-On** tabblad en toegang tot de ingesloten documentatie via de **configuratie** sectie onderaan. U kunt meer lezen over de ingesloten documentatie-functie: [embedded-documentatie voor Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken
Het doel van deze sectie is het een testgebruiker maken in de Azure portal Britta Simon aangeroepen.

![Azure AD-gebruiker maken][100]

**Als u wilt een testgebruiker maken in Azure AD, moet u de volgende stappen uitvoeren:**

1. In de **Azure-portal**, klik op het navigatiedeelvenster links **Azure Active Directory** pictogram.

    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. Als u wilt weergeven in de lijst met gebruikers, gaat u naar **gebruikers en groepen** en klik op **alle gebruikers**.
    
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. Openen van de **gebruiker** dialoogvenster, klikt u op **toevoegen** boven aan het dialoogvenster.
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Op de **gebruiker** dialoogvenster pagina, voert u de volgende stappen uit:
 
    ![Een Azure AD-testgebruiker maken](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    a. In de **naam** textbox type **BrittaSimon**.

    b. In de **gebruikersnaam** textbox type de **e-mailadres** van BrittaSimon.

    c. Selecteer **wachtwoord weergeven** en noteer de waarde van de **wachtwoord**.

    d. Klik op **Create**.
 
### <a name="creating-an-adp-etime-test-user"></a>Maken van een gebruiker ADP eTime testen

Het doel van deze sectie is het maken van een gebruiker Britta Simon in ADP eTime genoemd. Werken met [ADP eTime ondersteuningsteam](https://www.adp.com/contact-us/overview.aspx) de gebruikers in het ADP eTime account toevoegen. 
   
### <a name="assigning-the-azure-ad-test-user"></a>Toewijzen van de testgebruiker Azure AD

In deze sectie schakelt u Britta Simon Azure eenmalige aanmelding gebruiken door het verlenen van toegang tot ADP eTime.

![Gebruiker toewijzen][200] 

**Britta Simon om aan te wijzen ADP eTime, moet u de volgende stappen uitvoeren:**

1. Open de weergave toepassingen in de Azure-portal en gaat u naar de directoryweergave en gaat u naar **bedrijfstoepassingen** klikt u vervolgens op **alle toepassingen**.

    ![Gebruiker toewijzen][201] 

2. Selecteer in de lijst met toepassingen **ADP eTime**.

    ![Eenmalige aanmelding configureren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. Klik in het menu aan de linkerkant op **gebruikers en groepen**.

    ![Gebruiker toewijzen][202] 

4. Klik op **toevoegen** knop. Selecteer vervolgens **gebruikers en groepen** op **toevoegen toewijzing** dialoogvenster.

    ![Gebruiker toewijzen][203]

5. Op **gebruikers en groepen** dialoogvenster Selecteer **Britta Simon** in de lijst gebruikers.

6. Klik op **Selecteer** knop op **gebruikers en groepen** dialoogvenster.

7. Klik op **toewijzen** knop op **toevoegen toewijzing** dialoogvenster.
    
### <a name="testing-single-sign-on"></a>Testen van eenmalige aanmelding

In deze sectie kunt u uw Azure AD eenmalige aanmelding configuratie met behulp van het toegangsvenster testen.

Als u op de tegel ADP eTime in het deelvenster toegang, u moet ophalen automatisch aangemeld bij uw ADP eTime-toepassing.
Zie voor meer informatie over het toegangsvenster [Inleiding tot het toegangsvenster](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Aanvullende bronnen

* [Lijst met zelfstudies over het integreren van SaaS-Apps met Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

