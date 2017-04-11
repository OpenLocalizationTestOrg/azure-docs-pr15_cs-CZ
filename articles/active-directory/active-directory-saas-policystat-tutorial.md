<properties 
    pageTitle="Kurz: Azure Active Directory integrace s PolicyStat | Microsoft Azure" 
    description="Naučte se používat PolicyStat s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Kurz: Azure Active Directory integrace s PolicyStat
  
Cílem tohoto kurzu je zobrazit integrace Azure a PolicyStat.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   PolicyStat klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti PolicyStat (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili PolicyStat.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro PolicyStat
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Scénář")
##<a name="enabling-the-application-integration-for-policystat"></a>Povolení integrace aplikací pro PolicyStat
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace PolicyStat.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace PolicyStat, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **PolicyStat**.

    ![Galerie aplikace] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Galerie aplikace")

7.  V podokně výsledků vyberte **PolicyStat**a potom klikněte na **Dokončit** přidat aplikaci.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit PolicyStat s účtem v Azure AD pomocí federace podle protokolu SAML.  
Aplikace PolicyStat očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![Atributy] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **PolicyStat** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k PolicyStat** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** **Odhlásit na adresu URL** do textového pole, zadejte adresu URL používané uživatelům přihlašování do aplikace PolicyStat adresu URL (například: *"https://demo-azure.policystat.com"*) a potom na tlačítko **Další**.

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Konfigurace nastavení aplikace")

4.  Na stránce **konfigurovat jednotné přihlašování v PolicyStat** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Konfigurace jednotného přihlašování")

5.  V okně prohlížeče různých webu Přihlaste se k webu společnosti PolicyStat jako správce.

6.  Klikněte na kartu **Správce** a potom klikněte na **Jednotné přihlašování** v levém navigačním podokně.

    ![Nabídka správce] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Nabídka správce")

7.  V části **Nastavení** vyberte **Povolit jednoho integrace přihlašování**.

    ![Jednotné přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Jednotné přihlašování")

8.  Klikněte na **Konfigurovat atributy**a potom v části **Atributy konfigurace** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Jednotné přihlašování")

    1.  Do textového pole **Uživatelské jméno atribut** zadejte **uid**.
    2.  Do textového pole **Atribut křestní jméno** zadejte **jméno**.
    3.  **Poslední atribut název** do textového pole zadejte **Příjmení**.
    4.  **Atribut e-mailu** do textového pole zadejte **emailaddress**.
    5.  Klikněte na **Uložit změny**.

9.  Klepněte na **Svůj Metadata IDP**a potom v části **Your IDP metadat** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Jednotné přihlašování")

    1.  Metadata stažený soubor otevřít, zkopírujte obsah a potom je vložte do textového pole **Your metadat poskytovatele Identity**
    2.  Klikněte na **Uložit změny**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Konfigurace jednotného přihlašování")

11. 12. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Atributy")

13. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![Atributy] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Atributy")

    1.  Klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte **uid**.
    3.  **Hodnota atributu** do textového pole vyberte **ExtractMailPrefix()**.
    4.  V seznamu **Pošta** vyberte **User.mail**.
    5.  Klikněte na **dokončení**.
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit PolicyStat, musí být zřízení do PolicyStat.  
PolicyStat podporuje jenom čas zřizování uživatelů. To znamená nepotřebujete ruční přidání uživatelů do PolicyStat.  
Uživatelé získat přidá automaticky při jejich prvním přihlášení pomocí jednotného přihlášení na.

>[AZURE.NOTE]Můžete použít jakékoli další PolicyStat uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou PolicyStat k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Přiřazení uživatelů k PolicyStat, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **PolicyStat **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).