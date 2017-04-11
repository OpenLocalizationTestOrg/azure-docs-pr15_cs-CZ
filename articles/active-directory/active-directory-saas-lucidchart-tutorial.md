<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Lucidchart | Microsoft Azure" 
    description="Naučte se používat Lucidchart s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Kurz: Azure Active Directory integrace s Lucidchart
  
Cílem tohoto kurzu je zobrazit integrace Azure a Lucidchart.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Lucidchart jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, které jste přiřadili Lucidchart uživatelům Azure AD bude moct jednotného přihlášení do aplikace na webu společnosti Lucidchart (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Lucidchart
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Scénář")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Povolení integrace aplikací pro Lucidchart
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Lucidchart.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Lucidchart, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Lucidchart**.

    ![Galerie aplikace] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Lucidchart**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Lucidchart s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Lucidchart** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Lucidchart** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Lucidchart na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Lucidchart (například: "*https://chart2.office.lucidchart.com/saml/sso/azure*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Lucidchart** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte jej data místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Lucidchart jako správce.

6.  V nabídce nahoře klikněte na **týmu**.

    ![Tým] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Tým")

7.  Klikněte na **aplikace \> Správa SAML**.

    ![Správa SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "Správa SAML")

8.  Na stránce dialogové okno **Nastavení ověřování SAML** proveďte následující kroky:

    1.  Zaškrtněte políčko **Povolit ověřování SAML**a potom klikněte na **nepovinný**.
        ![Nastavení ověřování SAML] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "Nastavení ověřování SAML")
    2.  **Doménu** do textového pole zadejte svoji doménu a klikněte na **Změnit certifikát**.
        ![Změna certifikátu] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Změna certifikátu")
    3.  Metadata stažený soubor otevřít, zkopírujte obsah a potom je vložte do textového pole **Odeslat Metadata** .
        ![Nahrání metadat] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Nahrání metadat")
    4.  Vyberte **automaticky přidat nového uživatele do týmu**a potom klikněte na **Uložit změny**.
        ![Uložení změn] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Uložení změn")

9.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Lucidchart.  
Při přiřazený uživatel se pokusí přihlásit Lucidchart pomocí panelu aplikace access, Lucidchart kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Lucidchart.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Přiřazení uživatelů k Lucidchart, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Lucidchart **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).