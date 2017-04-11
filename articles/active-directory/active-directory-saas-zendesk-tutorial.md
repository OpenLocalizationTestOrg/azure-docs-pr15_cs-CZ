<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Zendesk | Microsoft Azure" 
    description="Naučte se používat Zendesk s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Kurz: Azure Active Directory integrace s Zendesk
  
Cílem tohoto kurzu je zobrazit integrace Azure a Zendesk.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Zendesk klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Zendesk (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Zendesk.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Zendesk
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scénář")

##<a name="enabling-the-application-integration-for-zendesk"></a>Povolení integrace aplikací pro Zendesk
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Zendesk.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Zendesk, proveďte následující kroky:

1.  Na portálu Správa Azure v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Zendesk**.

    ![Galerie aplikace] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Zendesk**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Zendesk s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Zendesk vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu Azure AD na stránce integrace **Zendesk** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Jednotné přihlašování] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Zendesk** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Konfigurace aplikace URL")
  
    na. Do textového pole **Symbol Zendesk v adrese URL** zadejte adresu URL pomocí následujícího vzorce:`https://<tenant-name>.zendesk.com`

    b. Klikněte na tlačítko **Další**.



4.  Na stránce **konfigurovat jednotné přihlašování v Zendesk** klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vaše compiter.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti Zendesk jako správce.

6.  Klikněte na **Správce**.

7.  V levém navigačním podokně klikněte na **Nastavení**a potom klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Zabezpečení")

8.  Na kartě **zabezpečení** klikněte na kartu **správce a agentů** .

9.  Vyberte **jedním přihlašování (SSO) a SAML**a pak vyberte **SAML**.

10. Na portálu Azure AD na stránce **konfigurovat jednotné přihlašování v Zendesk** zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **URL SAML jednotné přihlašování** .

11. Na portálu Azure AD na stránce **konfigurovat jednotné přihlašování v Zendesk** zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL vzdálené odhlásit** .

    ![Jednotné přihlašování] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Jednotné přihlašování")

12. Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikát** .

    >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

13. Klikněte na **Uložit**.

14. Na portálu Azure AD vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit **Zendesk**, musí být zřízení do **Zendesk**.  
V případě **Zendesk**zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Zřízení uživatelského účtu Zendesk, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Zendesk** .

2.  Vyberte kartu **Seznam zákazníků** .

3.  Vyberte kartu **uživatele** a klikněte na **Přidat**.

    ![Přidat uživatele] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Přidat uživatele")

4.  Zadejte e-mailovou adresu existující účet Azure AD trvat a potom klikněte na **Uložit**.

    ![Nového uživatele] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Nového uživatele")

>[AZURE.NOTE] Můžete použít jakékoli další Zendesk uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Zendesk k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Přiřazení uživatelů k Zendesk, proveďte následující kroky:

1.  Na portálu Azure AD vytvořte testovací účet.

2.  Na stránce integrace **Zendesk **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
