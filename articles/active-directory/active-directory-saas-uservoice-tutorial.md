<properties 
    pageTitle="Kurz: Azure Active Directory integrace s UserVoice | Microsoft Azure" 
    description="Naučte se používat UserVoice s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Kurz: Azure Active Directory integrace s UserVoice
  
Cílem tohoto kurzu je zobrazit integrace Azure a UserVoice.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   UserVoice klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti UserVoice (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili UserVoice.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro UserVoice
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scénář")

##<a name="enabling-the-application-integration-for-uservoice"></a>Povolení integrace aplikací pro UserVoice
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace UserVoice.

###<a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace UserVoice, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-uservoice-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-uservoice-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-uservoice-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-uservoice-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **UserVoice**.

    ![Galerie aplikace] (./media/active-directory-saas-uservoice-tutorial/IC777513.png "Galerie aplikace")

7.  V podokně výsledků vyberte **UserVoice**a potom klikněte na **Dokončit** přidat aplikaci.

    ![UserVoice] (./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit UserVoice s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro UserVoice vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **UserVoice** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-uservoice-tutorial/IC777515.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k UserVoice** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-uservoice-tutorial/IC777516.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **UserVoice znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. UserVoice.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-uservoice-tutorial/IC777517.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v UserVoice** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\UserVoice.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-uservoice-tutorial/IC777518.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti UserVoice jako správce.

6.  Na panelu nástrojů na horní klikněte na nastavení a potom v nabídce vyberte webového portálu.

    ![Nastavení] (./media/active-directory-saas-uservoice-tutorial/IC777519.png "Nastavení")

7.  Na kartě **Web portálu** , v části **ověřování uživatele** klikněte na **Upravit** a otevře se stránka dialogového okna **Upravit ověřování uživatelů**

    ![Webového portálu] (./media/active-directory-saas-uservoice-tutorial/IC777520.png "Webového portálu")

8.  Na stránce dialogové okno **Upravit ověřování uživatelů** proveďte následující kroky:

    ![Úprava ověřování uživatelů] (./media/active-directory-saas-uservoice-tutorial/IC777521.png "Úprava ověřování uživatelů")

    1.  Klikněte na **Jednotné přihlašování (SSO)**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v UserVoice** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do **Jednotné přihlašování vzdálené přihlášení** do textového pole.
    3.  Na portálu na **konfigurovat jednotné přihlašování v UserVoice** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do **textového pole vzdálené Sign-Out jednotné přihlašování**.
    4.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **aktuální otisku SHA1 certifikát** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    5.  Klikněte na **Uložit nastavení ověřování**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-uservoice-tutorial/IC777522.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit UserVoice, musí být zřízení do UserVoice.  
V případě UserVoice zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **UserVoice** .

2.  Přejděte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-uservoice-tutorial/IC777811.png "Nastavení")

3.  Klikněte na **Obecné**.

4.  Klikněte na **agentů a oprávnění**.

    ![Agentů a oprávnění] (./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agentů a oprávnění")

5.  Klikněte na **Přidat správce**.

    ![Přidat správce] (./media/active-directory-saas-uservoice-tutorial/IC777813.png "Přidat správce")

6.  V dialogu **Pozvat správci** proveďte následující kroky:

    ![Pozvat správci] (./media/active-directory-saas-uservoice-tutorial/IC777814.png "Pozvat správci")

    1.  V e-mailů texbox zadejte e-mailovou adresu účtu trvat a potom klikněte na **Přidat**.
    2.  Klikněte na **pozvánku**.

>[AZURE.NOTE] Můžete použít jakékoli další UserVoice uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou UserVoice k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>Přiřazení uživatelů k UserVoice, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **UserVoice **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-uservoice-tutorial/IC777523.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-uservoice-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).