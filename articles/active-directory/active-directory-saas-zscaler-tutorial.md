<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Zscaler | Microsoft Azure" 
    description="Naučte se používat Zscaler s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zscaler"></a>Kurz: Azure Active Directory integrace s Zscaler
  
Cílem tohoto kurzu je integrace Azure a Zscaler zobrazit. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Ke klientovi Zscaler
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Zscaler
2.  Konfigurace jednotného přihlašování
3.  Konfigurace nastavení proxy serveru.
4.  Konfigurace zřizování uživatelů
5.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-zscaler-tutorial/IC769226.png "Scénář")

##<a name="enabling-the-application-integration-for-zscaler"></a>Povolení integrace aplikací pro Zscaler
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Zscaler.

###<a name="to-enable-the-application-integration-for-zscaler-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Zscaler, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-zscaler-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-zscaler-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-zscaler-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-zscaler-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Zscaler**.

    ![Galerie aplikace] (./media/active-directory-saas-zscaler-tutorial/IC769227.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Zscaler**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Zscaler] (./media/active-directory-saas-zscaler-tutorial/IC769228.png "Zscaler")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Zscaler s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu musíte do Zscaler nahrát certifikát.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Zscaler** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-zscaler-tutorial/IC769229.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Zscaler** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-zscaler-tutorial/IC769230.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** **Zscaler znak v adresu URL** do textového pole zadejte do pole Adresa URL jste získali od Zscaler registraci a klikněte na **Další**: 

    >[AZURE.NOTE] Pokud nevíte, co je vaše přihlašovací adresa URL, obraťte se na tým podpory Zscaler.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-zscaler-tutorial/IC769231.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Zscaler** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-tutorial/IC769232.png "Konfigurace jednotného přihlašování")

    1.  Klikněte na **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Zscaler.cer**.
    2.  Zkopírujte **ověřování požadavku na adresu URL** do schránky.

5.  Přihlaste se ke klientovi Zscaler.

6.  V nabídce nahoře klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-zscaler-tutorial/IC769486.png "Správa")

7.  V části **Správa správců a role**klikněte na **spravovat uživatelé a ověření**.

    ![Správa správců a rolí] (./media/active-directory-saas-zscaler-tutorial/IC769487.png "Správa správců a rolí")

8.  V části **Vyberte možnost ověřování pro vaši organizaci** proveďte následující kroky:

    ![Vyberte možnosti ověřování] (./media/active-directory-saas-zscaler-tutorial/IC769488.png "Vyberte možnosti ověřování")

    1.  Vyberte **ověřit pomocí SAML jednotného přihlašování**.
    2.  Klikněte na **Konfigurovat SAML jednoho přihlašování parametry**.

9.  Na stránce dialogové okno **Konfigurovat SAML jednoho přihlašování parametry** proveďte následující kroky a potom klikněte na tlačítko **Hotovo**:

    ![Nahrání certifikátu] (./media/active-directory-saas-zscaler-tutorial/IC769489.png "Nahrání certifikátu")

    1.  Do textového pole **adresu URL portálu SAML, do kterého uživatelé odesílají pro ověřování** vložte hodnotu do pole **Adresa URL žádost o ověřování** z portálu Microsoft Azure klasické.
    2.  **Atribut obsahující přihlašovací jméno** do textového pole zadejte **NameID**.
    3.  V poli **Nahrát veřejné certifikát SSL** nahrajte certifikát, který jste stáhli z portálu Microsoft Azure klasické.
    4.  Zaškrtněte políčko **Povolit automatické vytváření SAML**.

10. Na stránce dialogové okno **Konfigurace ověřování uživatelů** proveďte následující kroky:

    ![Konfigurace ověřování uživatelů] (./media/active-directory-saas-zscaler-tutorial/IC769490.png "Konfigurace ověřování uživatelů")

    1.  Klikněte na **Uložit**.
    2.  Klikněte na **Aktivovat nyní**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-tutorial/IC769491.png "Konfigurace jednotného přihlašování")

##<a name="configuring-proxy-settings"></a>Konfigurace nastavení proxy serveru.

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Konfigurace nastavení proxy serveru v Internet Exploreru

1.  Spusťte **Internet Explorer**.

2.  Příkaz **Možnosti Internetu** v nabídce **Nástroje** pro otevření dialogového okna **Možnosti Internetu** .

    ![Možnosti Internetu] (./media/active-directory-saas-zscaler-tutorial/IC769492.png "Možnosti Internetu")

3.  Klikněte na kartu **připojení** .

    ![Připojení] (./media/active-directory-saas-zscaler-tutorial/IC769493.png "Připojení")

4.  Klikněte na **Nastavení místní sítě** a otevřete dialogové okno **Nastavení místní sítě** .

5.  V části Proxy server takto:

    ![Proxy serveru] (./media/active-directory-saas-zscaler-tutorial/IC769494.png "Proxy serveru")

    1.  Vyberte použít proxy server pro síť LAN.
    2.  Do textového pole Adresa zadejte **gateway.zscalertwo.net**.
    3.  Do textového pole Port zadejte **80**.
    4.  Zaškrtněte políčko **vynechat proxy server pro místní adresy**.
    5.  Klikněte na **OK** zavřete dialogové okno **Nastavení místní sítě LAN** .

6.  Klikněte na **OK** zavřete dialogové okno **Možnosti Internetu** .

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Zscaler, musí být zřízení do Zscaler.  
V případě Zscaler zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Zscaler** .

2.  Klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-zscaler-tutorial/IC781035.png "Správa")

3.  Klikněte na **Správa uživatelů**.

    ![Správa uživatelů] (./media/active-directory-saas-zscaler-tutorial/IC781036.png "Správa uživatelů")

4.  Na kartě **Uživatelé** klikněte na **Přidat**.

    ![Přidání] (./media/active-directory-saas-zscaler-tutorial/IC781037.png "Přidání")

5.  V části přidat uživatele proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-zscaler-tutorial/IC781038.png "Přidání uživatele")

    1.  Zadejte **uživatelské ID**, **Zobrazované jméno**, **heslo**, **Potvrďte heslo**a klikněte na **skupiny** a **oddělení** platného AAD účtu, který chcete poskytování.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli jiné Zscaler uživatelského účtu nástroje pro vytváření nebo rozhraní API poskytovanou Zscaler k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-zscaler-perform-the-following-steps"></a>Přiřazení uživatelů k Zscaler, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Zscaler** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-zscaler-tutorial/IC769495.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-zscaler-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
