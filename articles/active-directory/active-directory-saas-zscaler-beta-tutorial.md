<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Zscaler Beta | Microsoft Azure" 
    description="Naučte se používat Zscaler Beta se službou Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a>Kurz: Azure Active Directory integrace s Zscaler Beta
  
Cílem tohoto kurzu je zobrazit integrace Azure a ZScaler Beta.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   ZScaler Beta jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti ZScaler Beta (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené ZScaler Beta.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro ZScaler Beta
2.  Konfigurace jednotného přihlašování
3.  Konfigurace nastavení proxy serveru.
4.  Konfigurace zřizování uživatelů
5.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-zscaler-beta-tutorial/IC800223.png "Scénář")

##<a name="enabling-the-application-integration-for-zscaler-beta"></a>Povolení integrace aplikací pro ZScaler Beta
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace ZScaler Beta.

###<a name="to-enable-the-application-integration-for-zscaler-beta-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ZScaler Beta, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-zscaler-beta-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-zscaler-beta-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-zscaler-beta-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-zscaler-beta-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ZScaler Beta**.

    ![Galerie aplikace] (./media/active-directory-saas-zscaler-beta-tutorial/IC800224.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ZScaler Beta**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ZScaler jeden] (./media/active-directory-saas-zscaler-beta-tutorial/IC800216.png "ZScaler jeden")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ZScaler Beta se svým účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi ZScaler Beta.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **ZScaler Beta** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800225.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ZScaler Beta** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800226.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **ZScaler Beta znaménko na adresu URL** do textového pole zadejte adresu URL používané vaší uživatelům přihlašování do aplikace ZScaler Beta a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-zscaler-beta-tutorial/IC800227.png "Konfigurace aplikace URL")

    >[AZURE.NOTE] Skutečná hodnota ve vašem prostředí získáte od týmu podpory ZScaler Beta pokud ho potřebujete.

4.  Na stránce **konfigurovat jednotné přihlašování v ZScaler Beta** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800228.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti ZScaler Beta jako správce.

6.  V nabídce nahoře klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-zscaler-beta-tutorial/IC800206.png "Správa")

7.  V části **Správa správců a role**klikněte na **Správa uživatelů a ověření**.

    ![Správa uživatelů a ověření] (./media/active-directory-saas-zscaler-beta-tutorial/IC800207.png "Správa uživatelů a ověření")

8.  V části **Zvolte možnosti ověřování pro vaši organizaci** proveďte následující kroky:

    ![Ověřování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800208.png "Ověřování")

    1.  Vyberte **ověřit pomocí SAML jednotného přihlašování**.
    2.  Klikněte na **Konfigurovat SAML jednoho přihlašování parametry**.

9.  Na stránce dialogové okno **Konfigurovat SAML jednoho přihlašování parametry** proveďte následující kroky a potom klikněte na tlačítko **Hotovo**:

    ![Jednotné přihlašování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800209.png "Jednotné přihlašování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v ZScaler Beta** dialogové okno stránce Azure klasické zkopírujte hodnotu **Adresa URL požadovat ověření** a potom je vložte do textového pole **adresu URL portálu SAML, do kterého uživatelé odesílají pro ověřování** .
    2.  **Atribut obsahující přihlašovací jméno** do textového pole zadejte **NameID**.
    3.  K nahrání stažený certifikát, klikněte na **Zscaler pem**.
    4.  Zaškrtněte políčko **Povolit automatické vytváření SAML**.

10. Na stránce dialogové okno **Konfigurace ověřování uživatelů** proveďte následující kroky:

    ![Správa] (./media/active-directory-saas-zscaler-beta-tutorial/IC800210.png "Správa")

    1.  Klikněte na **Uložit**.
    2.  Klikněte na **Aktivovat nyní**.

11. Na portálu na **konfigurovat jednotné přihlašování v ZScaler Beta** dialogové okno stránce Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zscaler-beta-tutorial/IC800229.png "Konfigurace jednotného přihlašování")

##<a name="configuring-proxy-settings"></a>Konfigurace nastavení proxy serveru.

###<a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Konfigurace nastavení proxy serveru v Internet Exploreru

1.  Spusťte **Internet Explorer**.

2.  Příkaz **Možnosti Internetu** v nabídce **Nástroje** pro otevření dialogového okna **Možnosti Internetu** .

    ![Možnosti Internetu] (./media/active-directory-saas-zscaler-beta-tutorial/IC769492.png "Možnosti Internetu")

3.  Klikněte na kartu **připojení** .

    ![Připojení] (./media/active-directory-saas-zscaler-beta-tutorial/IC769493.png "Připojení")

4.  Klikněte na **Nastavení místní sítě** a otevřete dialogové okno **Nastavení místní sítě** .

5.  V části Proxy server takto:

    ![Proxy serveru] (./media/active-directory-saas-zscaler-beta-tutorial/IC769494.png "Proxy serveru")

    1.  Vyberte použít proxy server pro síť LAN.
    2.  Do textového pole Adresa zadejte **gateway.zscalerBeta.net**.
    3.  Do textového pole Port zadejte **80**.
    4.  Zaškrtněte políčko **vynechat proxy server pro místní adresy**.
    5.  Klikněte na **OK** zavřete dialogové okno **Nastavení místní sítě LAN** .

6.  Klikněte na **OK** zavřete dialogové okno **Možnosti Internetu** .

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit ZScaler Beta, musí být zřízení na ZScaler Beta.  
V případě ZScaler Beta zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Zscaler** .

2.  Klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-zscaler-beta-tutorial/IC781035.png "Správa")

3.  Klikněte na **Správa uživatelů**.

    ![Přidání] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Přidání")

4.  Na kartě **Uživatelé** klikněte na **Přidat**.

    ![Přidání] (./media/active-directory-saas-zscaler-beta-tutorial/IC781037.png "Přidání")

5.  V části přidat uživatele proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-zscaler-beta-tutorial/IC781038.png "Přidání uživatele")

    1.  Zadejte **uživatelské ID**, **Zobrazované jméno**, **heslo**, **Potvrďte heslo**a klikněte na **skupiny** a **oddělení** platného AAD účtu, který chcete poskytování.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další ZScaler Beta uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ZScaler Beta k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-zscaler-beta-perform-the-following-steps"></a>Přiřazení uživatelů k ZScaler Beta, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **ZScaler Beta** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-zscaler-beta-tutorial/IC800230.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-zscaler-beta-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).