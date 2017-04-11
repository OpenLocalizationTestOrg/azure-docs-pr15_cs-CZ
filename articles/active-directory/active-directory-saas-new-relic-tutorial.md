<properties 
    pageTitle="Kurz: Azure Active Directory integrace s novou Relic | Microsoft Azure" 
    description="Naučte se používat novou Relic s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Kurz: Azure Active Directory integrace s novou Relic
  
Cílem tohoto kurzu je ukazují, jak nastavit jednotné přihlašování mezi Azure Active Directory a nové Relic.
  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Nové Relic jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure Active Directory uživatele, které jste přiřadili nové Relic budou moct jednotné přihlašování pomocí panelu AAD přístup.

1.  Povolení integrace aplikací pro nové Relic
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Scénář")
##<a name="enabling-the-application-integration-for-new-relic"></a>Povolení integrace aplikací pro nové Relic
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro nové Relic.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace pro nové Relic, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Nové Relic**.

    ![Galerie aplikace] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Nové Relic**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Nové Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Nové Relic")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Tato část popisuje, jak povolit uživatelům ověřit nové Relic s účtem v Azure Active Directory federation podle protokolu SAML pomocí.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Nové Relic** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k nové Relic** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Nová Relic znak na adresa URL** zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci nové Relic a klikněte na tlačítko **Další**. 

    Adresa URL aplikace je svoji adresu URL nového Relic klienta (například: *https://rpm.newrelic.com*):

    ![Konfigurace aplikace URL] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v nové Relic** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně do svého počítače.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webových Přihlaste se k vašemu webu společnosti **Nové Relic** jako správce.

6.  V nabídce nahoře klikněte na **Nastavení účtu**.

    ![Nastavení účtu] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Nastavení účtu")

7.  Klikněte na kartu **zabezpečení a ověření** a potom klikněte na kartu **jednotné přihlašování** .

    ![Jednotné přihlašování] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Jednotné přihlašování")

8.  Na stránce dialogové okno SAML proveďte následující kroky:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Klikněte na **Zvolit soubor** k nahrání stažený certifikát Azure Active Directory.
    2.  Na portálu na stránce **konfigurovat jednotné přihlašování v nové Relic** Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **vzdálené přihlašovací adresa URL** .
    3.  Na portálu na stránce **konfigurovat jednotné přihlašování v nové Relic** Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **odhlášení úvodní adresu URL** .
    4.  Klikněte na **Uložit změny**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure Active Directory do nového Relic přihlásit, musí být zřízení do nového Relic.  
V případě nové Relic zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Zřízení uživatelského účtu nové Relic, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Nové Relic** jako správce.

2.  V nabídce nahoře klikněte na **Nastavení účtu**.

    ![Nastavení účtu] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Nastavení účtu")

3.  V podokně **účtu** na levé straně klikněte na **Shrnutí**a pak klikněte na **Přidat uživatele**.

    ![Nastavení účtu] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Nastavení účtu")

4.  V dialogovém okně **aktivní uživatelé** proveďte následující kroky:

    ![Aktivní uživatelé] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktivní uživatelé")

    1.  **E-mailu** do textového pole zadejte e-mailovou adresu platné Azure Active Directory uživatele, kterého chcete poskytování.
    2.  **Roli** vyberte **uživatele**.
    3.  Klikněte na **Přidat uživatele**.

>[AZURE.NOTE]Můžete použít jakékoli další nové Relic uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou nové Relic k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Přiřazení uživatelů k nové Relic, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce **Nové Relic** aplikace integrace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).




