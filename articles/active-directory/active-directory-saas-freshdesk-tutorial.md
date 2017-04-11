<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Freshdesk | Microsoft Azure" 
    description="Naučte se používat Freshdesk s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-freshdesk"></a>Kurz: Azure Active Directory integrace s Freshdesk
  
Cílem tohoto kurzu je zobrazit integrace Azure a Freshdesk.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Freshdesk klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Freshdesk (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Freshdesk.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Freshdesk
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-freshdesk-tutorial/IC776761.png "Scénář")
##<a name="enabling-the-application-integration-for-freshdesk"></a>Povolení integrace aplikací pro Freshdesk
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Freshdesk.

###<a name="to-enable-the-application-integration-for-freshdesk-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Freshdesk, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-freshdesk-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-freshdesk-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-freshdesk-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-freshdesk-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Freshdesk**.

    ![Galerie aplikace] (./media/active-directory-saas-freshdesk-tutorial/IC776762.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Freshdesk**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Freshdesk] (./media/active-directory-saas-freshdesk-tutorial/IC776763.png "Freshdesk")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Freshdesk s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Freshdesk vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Freshdesk** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshdesk-tutorial/IC776764.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Freshdesk** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshdesk-tutorial/IC776765.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Freshdesk znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Freshdesk.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-freshdesk-tutorial/IC776766.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Freshdesk** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Freshdesk.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshdesk-tutorial/IC776767.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti Freshdesk jako správce.

6.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Správce")

7.  Na kartě **Obecné nastavení** klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Zabezpečení")

8.  V části **zabezpečení** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Jednotné přihlašování")

    1.  ** **Jeden znak na (SSO)**vyberte.**
    2.  Vyberte **SAML přihlašování**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v Freshdesk** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **SAML přihlašovací adresa URL** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v Freshdesk** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    5.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikátu zabezpečení** .  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    6.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-freshdesk-tutorial/IC776771.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Freshdesk, musí být zřízení do Freshdesk.  
V případě Freshdesk zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Freshdesk** .

2.  V nabídce na horní klikněte na **Správce**.

    ![Správce] (./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Správce")

3.  Na kartě **Obecné nastavení** klikněte na **agentů**.

    ![Agentů] (./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agentů")

4.  Klikněte na **Nový Agent**.

    ![Nový Agent] (./media/active-directory-saas-freshdesk-tutorial/IC776774.png "Nový Agent")

5.  V dialogu informace o Agent proveďte následující kroky:

    ![Informace agenta] (./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Informace agenta")

    1.  **Úplný název** do textového pole zadejte název Azure AD účet, který chcete trvat.
    2.  **E-mailu** do textového pole zadejte e-mailovou adresu Azure AD Azure AD účtu, ke kterému chcete poskytování.
    3.  Do textového pole **název** zadejte název, který chcete poskytování účet Azure AD.
    4.  Vyberte **roli agentů**a klepněte na tlačítko **přiřadit**.
    5.  Klikněte na **Uložit**.
    
        >[AZURE.NOTE] Majitele účtu Azure AD pošle e-mail obsahující odkaz potvrďte účet předtím, než je aktivován.

>[AZURE.NOTE] Můžete použít jakékoli další Freshdesk uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Freshdesk k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-freshdesk-perform-the-following-steps"></a>Přiřazení uživatelů k Freshdesk, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Freshdesk **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-freshdesk-tutorial/IC776776.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-freshdesk-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).