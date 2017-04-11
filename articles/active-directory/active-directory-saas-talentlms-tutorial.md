<properties 
    pageTitle="Kurz: Azure Active Directory integrace s TalentLMS | Microsoft Azure" 
    description="Naučte se používat TalentLMS s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Kurz: Azure Active Directory integrace s TalentLMS
  
Cílem tohoto kurzu je zobrazit integrace Azure a TalentLMS.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   TalentLMS klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti TalentLMS (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili TalentLMS.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro TalentLMS
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Scénář")

##<a name="enabling-the-application-integration-for-talentlms"></a>Povolení integrace aplikací pro TalentLMS
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace TalentLMS.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace TalentLMS, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **TalentLMS**.

    ![Galerie aplikace] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Galerie aplikace")

7.  V podokně výsledků vyberte **TalentLMS**a potom klikněte na **Dokončit** přidat aplikaci.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit TalentLMS s svůj účet v Azure AD pomocí federace na základě SAML protokolu. .  
Konfigurace jednotného přihlašování pro TalentLMS vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **TalentLMS** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k TalentLMS** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **TalentLMS Sign v adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. TalentLMSapp.com*"a potom na tlačítko **Další**.

    ![Přihlaste se na adresy URL] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Přihlaste se na adresy URL")

4.  Na stránce **konfigurovat jednotné přihlašování v TalentLMS** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\TalentLMS.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti TalentLMS jako správce.

6.  V části **Nastavení účtu a** klikněte na kartu **uživatelů** .

    ![Účtu a nastavení] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Účtu a nastavení")

7.  Klikněte na **Jednotné přihlašování (SSO)**

8.  V části jednotného přihlašování proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Jednotné přihlašování")

    1.  V seznamu **Typ integrace jednotného přihlašování** vyberte **SAML 2.0**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v TalentLMS** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **zprostředkovatele identit (IdP)** .
    3.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikát** .

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    4.  Na portálu na **konfigurovat jednotné přihlašování v TalentLMS** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL vzdálené** .
    5.  Na portálu na **konfigurovat jednotné přihlašování v TalentLMS** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL vzdálené odhlašovací** .
    6.  Do textového pole **TargetedID** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Do textového pole **jméno** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  **Příjmení** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Do textového pole **e-mailu** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit TalentLMS, musí být zřízení do TalentLMS.  
V případě TalentLMS zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **TalentLMS** .

2.  Klikněte na **uživatele**a potom klikněte na **Přidat uživatele**.

3.  Na stránce dialogové okno **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Přidání uživatele")

    1.  Zadejte hodnoty související atribut Azure AD uživatelského účtu do následujících textová pole: **křestního jména**, **Příjmení**, **e-mailovou adresu**.
    2.  Klikněte na **Přidat uživatele**.

>[AZURE.NOTE] Můžete použít jakékoli další TalentLMS uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou TalentLMS k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Přiřazení uživatelů k TalentLMS, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **TalentLMS **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).