<properties 
    pageTitle="Kurz: Azure Active Directory integrace s NetDocuments | Microsoft Azure" 
    description="Naučte se používat NetDocuments s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-netdocuments"></a>Kurz: Azure Active Directory integrace s NetDocuments
  
Cílem tohoto kurzu je zobrazit integrace Azure a NetDocuments.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   NetDocuments klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti NetDocuments (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili NetDocuments.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro NetDocuments
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-netdocuments-tutorial/IC795040.png "Scénář")
##<a name="enabling-the-application-integration-for-netdocuments"></a>Povolení integrace aplikací pro NetDocuments
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace NetDocuments.

###<a name="to-enable-the-application-integration-for-netdocuments-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace NetDocuments, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-netdocuments-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-netdocuments-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-netdocuments-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-netdocuments-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **NetDocuments**.

    ![Galerie aplikace] (./media/active-directory-saas-netdocuments-tutorial/IC795041.png "Galerie aplikace")

7.  V podokně výsledků vyberte **NetDocuments**a potom klikněte na **Dokončit** přidat aplikaci.

    ![NetDocuments] (./media/active-directory-saas-netdocuments-tutorial/IC795042.png "NetDocuments")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit NetDocuments s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro NetDocuments vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **NetDocuments** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-netdocuments-tutorial/IC795043.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k NetDocuments** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-netdocuments-tutorial/IC795044.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-netdocuments-tutorial/IC795045.png "Konfigurace aplikace URL")

    1.  **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci NetDocuments (například: "*https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=CA-JI1BG3H1*").
    2.  **Adresa URL NetDocuments odpověď** do textového pole, zadejte stejné hodnoty jste zadali do textové pole **Symbol na adresu URL** .  

        >[AZURE.NOTE]Můžete najít správnou hodnotu na konci dialogu **Federované Identity** (viz snímek obrazovky kroku 9).

    3.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v NetDocuments** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-netdocuments-tutorial/IC795046.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti NetDocuments jako správce.

6.  Přejděte na **Správce**.

7.  Klikněte na **Přidat a odstranit uživatele nebo skupiny**.

    ![Úložiště] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Úložiště")

8.  Klikněte na **Konfigurovat Upřesnit nastavení ověřování**.

    ![Konfigurace rozšířené možnosti ověřování] (./media/active-directory-saas-netdocuments-tutorial/IC795048.png "Konfigurace rozšířené možnosti ověřování")

9.  V dialogovém okně **Identity federované** proveďte následující kroky:

    ![Federované Identitty] (./media/active-directory-saas-netdocuments-tutorial/IC795049.png "Federované Identitty")

    1.  Jako **Typ serveru identity federovaný**vyberte **Active Directory Federation Services**.
    2.  Klikněte na **Zvolit soubor**k nahrání souboru stažený metadat.
    3.  Klikněte na **OK**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-netdocuments-tutorial/IC795050.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit NetDocuments, musí být zřízení do NetDocuments.  
V případě NetDocuments zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Zpívají k webu společnosti **NetDocuments** jako správce.

2.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-netdocuments-tutorial/IC795051.png "Správce")

3.  Klikněte na **Přidat a odstranit uživatele nebo skupiny**.

    ![Úložiště] (./media/active-directory-saas-netdocuments-tutorial/IC795047.png "Úložiště")

4.  Do textového pole **E-mailová adresa** zadejte e-mailovou adresu platný účet Azure Active Directory trvat a klikněte na **Přidat uživatele**.

    ![E-mailovou adresu] (./media/active-directory-saas-netdocuments-tutorial/IC795053.png "E-mailovou adresu")

    >[AZURE.NOTE]Majitele účtu služby Azure Active Directory pošle e-mail obsahující odkaz před se aktivuje, potvrďte účet.

>[AZURE.NOTE]Můžete použít jakékoli další NetDocuments uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou NetDocuments k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-netdocuments-perform-the-following-steps"></a>Přiřazení uživatelů k NetDocuments, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **NetDocuments **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-netdocuments-tutorial/IC795054.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-netdocuments-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).