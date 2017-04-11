<properties 
    pageTitle="Kurz: Azure Active Directory integrace centrální plochou | Microsoft Azure" 
    description="Naučte se používat centrální plochy s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Kurz: Azure Active Directory integrace s centrální plochy

Cílem tohoto kurzu je zobrazit integrace Azure a centrální plochy. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Centrální plochy jednotného přihlášení povolené předplatného / centrální ploše klienta

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro centrální plochy
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scénář")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Povolení integrace aplikací pro centrální plochy

Cílem tento oddíl je přehledu jak povolit integraci aplikace centrální plochy.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace centrální plochy, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Centrální plochy**.

    ![Galerie aplikace] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Centrální plochu**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Centrální plochy] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Centrální plochy")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit centrální plochu s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi centrální plochy.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **Centrální plochy** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce, **jakým způsobem uživatelé přihlásit k centrální plochy** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky a potom klikněte na **Další**: 

    -   **Plocha Sign v adresa URL centra** do textového pole, zadejte adresu URL vašeho klienta centrální plochy (například: *http://contoso.centraldesktop.com*).
    -   Adresa URL centra odpověď pro Desktop do textového pole, zadejte adresu URL svého centrální AssertionConsumerService plochy (například: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Získat hodnotu z centrální plochy metadata (například: *http://contoso.centraldesktop.com*).

    ![Konfigurace aplikace URL] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v centrální plochy** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Konfigurace jednotného přihlašování")

5.  Přihlaste se ke tenantovi **Centrální plochy** .

6.  Přejděte na **Nastavení**, klikněte na položku **Upřesnit**a potom klikněte na **Jednotné přihlašování**.

    ![Instalace – Upřesnit] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Instalace – Upřesnit")

7.  Na stránce **Sign v nastavení jednotného** proveďte následující kroky:

    ![Jednotné přihlašování nastavení] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Jednotné přihlašování nastavení")

    1.  Vyberte **Povolit SAML v2 jednotné přihlašování**.
    2.  Na portálu na stránce **konfigurovat jednotné přihlašování v centrální plochy** Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Adresa URL jednotné přihlašování** .
    3.  Na portálu na stránce **konfigurovat jednotné přihlašování v centrální plochy** Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL jednotné přihlašování** .
    4.  Na portálu na stránce **konfigurovat jednotné přihlašování v centrální plochy** Azure klasické zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **URL odhlášení jednotné přihlašování** .

8.  V části **Metodu ověření zpráva podpis** proveďte následující kroky:

    ![Metodu ověření podpisu zprávy] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Zpráva metodu ověření podpisu")

    1.  Vyberte **certifikát**.
    2.  V seznamu **Jednotné přihlašování certifikátů** vyberte **RSH SHA256**.
    3.  Vytvoření textového souboru z stažený certifikát, zkopírujte obsah textový soubor a potom je vložte do pole **Jednotné přihlašování certifikátu** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Vyberte **zobrazení odkazu na stránku přihlášení SAMLv2**.

9.  Klikněte na **Aktualizovat**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pro AAD aby uživatelé mohli přihlásit musí být zřízení aplikace centrální plochy. Tato část popisuje, jak vytvořit AAD uživatelských účtů v centrální plochy.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Chcete-li vytvořit uživatelské účty centrální plochu:

1.  Přihlaste se ke tenantovi centrální plochy.

2.  Přejděte na **lidé \> interní členy**.

3.  Klikněte na **Přidat interní členy**.

    ![Lidé] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Lidé")

4.  **E-mailovou adresu nových členů** do textového pole zadejte účet AAD trvat a klepněte na tlačítko **Další**.

    ![E-mailové adresy nových členů] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-mailové adresy nových členů")

5.  Klikněte na **Přidat interní členy**.

    ![Přidat interní člena] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Přidat interní člena")

    >[AZURE.NOTE] Uživatelů, které jste přidali, dostanou e-mail obsahující odkaz potvrzení, stačí kliknout na tlačítko k aktivaci účtu.

>[AZURE.NOTE] Můžete použít jakékoli další centrální plochy uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou centrální plochy k poskytování AAD uživatelských účtů

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Přiřazení uživatelů k centrální plochy, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **Centrální plochy** klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
