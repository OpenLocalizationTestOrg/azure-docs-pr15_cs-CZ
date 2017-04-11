<properties 
    pageTitle="Kurz: Azure Active Directory integrace s adaptivní sadu | Microsoft Azure"
    description="Naučte se používat adaptivní sadu s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Kurz: Azure Active Directory integrace s adaptivní Suite

Cílem tohoto kurzu je zobrazit integrace Azure a adaptivní sadu.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Adaptivní sadu klienta

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti adaptivní sady (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, ke kterému je přiřazena adaptivní sadě.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací adaptivní sadu
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scénář")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Povolení integrace aplikací adaptivní sadu

Cílem tento oddíl je přehledu jak povolit integraci aplikace adaptivní sadu.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace adaptivní sadu, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Adaptivní sadu**.

    ![Galerie aplikace] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Adaptivní sady**a pak klikněte na **Dokončit** přidat aplikaci.

    ![Adaptivní Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptivní Suite")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit adaptivní sadě s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování adaptivní sadu vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Adaptivní sadu** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k adaptivní sadu** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** v textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce "*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*" a potom na tlačítko **Další**.

    >[AZURE.NOTE] Tato hodnota dostali ze stránky **Nastavení jednotného přihlašování SAML** adaptivní sadu.

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Konfigurace nastavení aplikace")

4.  Na stránce **konfigurovat jednotné přihlašování v adaptivní sadu** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti adaptivní sadu jako správce.

6.  Přejděte na **Správce**.

    ![Správce] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Správce")

7.  V části **Uživatelé a role** klikněte na **Spravovat nastavení jednotného přihlašování SAML**.

    ![Správa nastavení SAML jednotné přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Správa nastavení SAML jednotné přihlašování")

8.  Na stránce **Nastavení jednotného přihlašování SAML** proveďte následující kroky:

    ![Nastavení SAML jednotné přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "Nastavení SAML jednotné přihlašování")

    1.  Do textového pole **název poskytovatele Identity** zadejte název pro konfiguraci.
    2.  Na portálu na **konfigurovat jednotné přihlašování v adaptivní sadu** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **zprostředkovatele identit Entity ID** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v adaptivní sadu** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **zprostředkovatele identit URL jednotné přihlašování** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v adaptivní sadu** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **vlastní odhlášení adresa URL** .
    5.  K nahrání stažený certifikát, klikněte na **Zvolit soubor**.
    6.  Jako **SAML id uživatele**vyberte **adaptivní přehledy uživatelské jméno**.
    7.  Je **SAML umístění id uživatele**vyberte **id uživatele v NameID předmětu**.
    8.  Jako **SAML NameID formát**vyberte **e-mailovou adresu**.
    9.  Jako **SAML povolit**vyberte **Povolit SSO SAML a přímé přihlášení adaptivní přehledy**.
    10. Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit adaptivní sadu, musí být zřízení do adaptivní sadu.  
V případě adaptivní sadu zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Adaptivní sadu** jako správce.

2.  Přejděte na **Správce**.

    ![Správce] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Správce")

3.  V části **Uživatelé a rolí** klikněte na **Přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Přidání uživatele")

4.  V části **Nový uživatel** proveďte následující kroky:

    ![Odeslání] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Odeslání")

    1.  Zadejte **název**, **přihlášení**, **e-mailu**, **heslo** platné Azure Active Directory uživatele, kterého chcete poskytování do související textová pole.
    2.  Vyberte **roli**.
    3.  Klikněte na **Odeslat**.

>[AZURE.NOTE] Můžete použít jakékoli další adaptivní sady uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou adaptivní sadu k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Přiřazení uživatelů adaptivní sadě, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Adaptivní sadu **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
