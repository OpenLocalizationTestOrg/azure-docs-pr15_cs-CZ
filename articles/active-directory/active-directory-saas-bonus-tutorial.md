<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Bonus.ly | Microsoft Azure" 
    description="Naučte se používat Bonus.ly s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Kurz: Azure Active Directory integrace s Bonus.ly

Cílem tohoto kurzu je zobrazit integrace Azure a Bonus.ly. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Test klienta v Bonus.ly

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Bonus.ly
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-bonus-tutorial/IC773679.png "Scénář")
##<a name="enabling-the-application-integration-for-bonusly"></a>Povolení integrace aplikací pro Bonus.ly

Cílem tento oddíl je přehledu jak povolit integraci aplikace Bonus.ly.

###<a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Bonus.ly, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-bonus-tutorial/IC773680.png "Povolení jednotné přihlašování")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-bonus-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-bonus-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-bonus-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Bonus.ly**.

    ![Galerie aplikace] (./media/active-directory-saas-bonus-tutorial/IC773681.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Bonus.ly**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Bonus.ly s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Bonus.ly vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Bonus.ly** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bonus-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Bonus.ly** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bonus-tutorial/IC773683.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Bonus.ly klienta URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Bonus.ly*"a potom klikněte na **Další**: 

    ![Konfigurace aplikace URL] (./media/active-directory-saas-bonus-tutorial/IC773684.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Bonus.ly** klikněte na **Stáhnout certifikát**a potom soubor uložil v certifikát místně **c:\\Bonusly.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bonus-tutorial/IC773685.png "Konfigurace jednotného přihlašování")

5.  V jiném okně prohlížeče Přihlaste se k vašemu tenantovi **Bonus.ly** .

6.  Na panelu nástrojů na horní klikněte na **Nastavení**a vyberte **integrace a aplikace**.

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")

7.  V části **Jednotné přihlašování**vyberte **SAML**.

8.  Na stránce dialogové okno **SAML** proveďte následující kroky:

    ![Bonusly] (./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Bonus.ly** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **IdP SSO cílovou adresu URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v Bonus.ly** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID Vystavitel** a potom je vložte do textového pole **IdP vydavatel** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v Bonus.ly** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **IdP přihlašovací adresa URL** .
    4.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikátu** .

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

9.  Klikněte na **Uložit**.

10. Na portálu Microsoft Azure klasické vyberte potvrzení konfigurace a klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bonus-tutorial/IC773689.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Bonus.ly, musí být zřízení do Bonus.ly.  
V případě Bonus.ly zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  V okně webového prohlížeče Přihlaste se k vašemu tenantovi Bonus.ly.

2.  Klikněte na **Nastavení**

    ![Nastavení] (./media/active-directory-saas-bonus-tutorial/IC781041.png "Nastavení")

3.  Klikněte na kartu **uživatelů a prémií** .

    ![Uživatelé a prémií] (./media/active-directory-saas-bonus-tutorial/IC781042.png "Uživatelé a prémií")

4.  Klikněte na **Správa uživatelů**.

    ![Správa uživatelů] (./media/active-directory-saas-bonus-tutorial/IC781043.png "Správa uživatelů")

5.  Klikněte na **Přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-bonus-tutorial/IC781044.png "Přidání uživatele")

6.  V dialogovém okně **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-bonus-tutorial/IC781045.png "Přidání uživatele")

    1.  Zadejte "**e-mailu**, **křestního jména**, **Příjmení**" platného AAD účtu, který chcete poskytování do související textová pole.
    2.  Klikněte na **Uložit**.

    >[AZURE.NOTE] Majitele účtu AAD dostanou e-mail obsahující odkaz před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Bonus.ly uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Bonus.ly k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>Přiřazení uživatelů k Bonus.ly, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace Bonus.ly aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-bonus-tutorial/IC773690.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-bonus-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
