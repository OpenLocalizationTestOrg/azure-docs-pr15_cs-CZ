<properties 
    pageTitle="Kurz: Azure Active Directory integrace se službou xMatters OnDemand | Microsoft Azure"
    description="Naučte se používat xMatters OnDemand s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Kurz: Azure Active Directory integrace se službou xMatters OnDemand
  
Cílem tohoto kurzu je zobrazit integrace Azure a xMatters OnDemand. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Ke klientovi OnDemand xMatters
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti OnDemand xMatters (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili xMatters OnDemand.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro xMatters OnDemand
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scénář")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Povolení integrace aplikací pro xMatters OnDemand
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace xMatters OnDemand.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace xMatters OnDemand, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **xMatters OnDemand**.

    ![Galerie aplikace] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Galerie aplikace")

7.  V podokně výsledků vyberte **XMatters OnDemand**a potom klikněte na **Dokončit** přidat aplikaci.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit XMatters OnDemand s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **XMatters OnDemand** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k XMatters OnDemand** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Konfigurace aplikace URL")

    na. Do textového pole **XMatters OnDemand znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Klikněte na tlačítko **Další**.


4.  Na stránce **konfigurovat jednotné přihlašování v XMatters OnDemand** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Potřebujete přeposlání certifikát, který chcete xMatters tým podpory. Certifikát se musí nahrát tak, že tým podpory xMatters před dokončení konfigurace přihlašování.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Konfigurace jednoho přihlášení")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti XMatters OnDemand jako správce.

6.  Na panelu nástrojů na horní klikněte na **Správce**a potom klikněte na **Podrobnosti o společnosti** v navigačním panelu na levé straně.

    ![Správce] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Správce")

7.  Na stránce **Konfigurace SAML** proveďte následující kroky:

    ![Konfigurace SAML] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "Konfigurace SAML")

    1.  Zaškrtněte políčko **Povolit SAML**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v XMatters OnDemand** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **ID zprostředkovatele identit** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v XMatters OnDemand** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Jeden znak na adresu URL** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v XMatters OnDemand** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Jedné adresy URL odhlásit** .
    5.  Na stránce Podrobnosti o společnosti nahoře klikněte na **Uložit změny**.
        ![Podrobnosti o společnosti] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Podrobnosti o společnosti")

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Konfigurace jednoho přihlášení")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit XMatters OnDemand, musí být zřízení do XMatters OnDemand.  
V případě XMatters OnDemand zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **XMatters OnDemand** .

2.  Klikněte na kartu **uživatelů** .

3.  Klikněte na **Přidat uživatele**.

    ![Uživatelé] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Uživatelé")

4.  Vyberte **aktivní**.

5.  V části **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Přidání uživatele")

    1.  Zadejte **uživatelské ID**, **Křestní jméno**, **Příjmení**a **webu** platné AAD účet, který chcete trvat.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další XMatters OnDemand uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou XMatters OnDemand k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Přiřazení uživatelů k XMatters OnDemand, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **XMatters OnDemand **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).