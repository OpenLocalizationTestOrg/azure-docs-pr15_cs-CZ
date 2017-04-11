<properties
    pageTitle="Kurz: Azure Active Directory integrace s plátno LMS | Microsoft Azure" 
    description="Naučte se používat plátno LMS s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Kurz: Azure Active Directory integrace s plátno LMS

Cílem tohoto kurzu je zobrazit integrace Azure a plátno.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Plátno klienta

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti plátno (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené plátno.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro plátno
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Scénář")
##<a name="enabling-the-application-integration-for-canvas"></a>Povolení integrace aplikací pro plátno

Cílem tento oddíl je přehledu jak povolit integraci aplikace plátno.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace plátno, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **plátno**.

    ![Galerie aplikace] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Galerie aplikace")

7.  V podokně výsledků vyberte **plátno**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Plátno] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Plátno")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit plátno s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro plátno vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, přečtěte si, [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **plátno** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k plátno** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Plátno znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce `https://<tenant-name>.instructure.com`a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v plátno** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu společnosti plátno.

6.  Přejděte na **kurzy \> spravovat účty \> Microsoft**.

    ![Plátno] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Plátno")

7.  V navigačním podokně na levé straně vyberte **ověření**a potom klikněte na **Přidat novou SAML konfigurace**.

    ![Ověřování] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Ověřování")

8.  Na stránce aktuální integrace proveďte následující kroky:

    ![Aktuální integrace] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Aktuální integrace")

    1.  Na portálu na **konfigurovat jednotné přihlašování v plátno** dialogového okna stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **IdP Entity ID** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v plátno** dialogového okna stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Protokolu na adresu URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v plátno** dialogového okna stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Se adresa URL protokolu** .
    4.  Na portálu na **konfigurovat jednotné přihlašování v plátno** dialogového okna stránce Azure klasické zkopírujte její hodnotu **Heslo změnit adresu URL** a potom je vložte do textového pole **Odkazem pro změnu hesla** .
    5.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikát** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    6.  V seznamu **Přihlášení atributů** vyberte **NameID**.
    7.  Ze seznamu **Formát identifikátor URI** vyberte **emailAddress**.
    8.  Klikněte na **Uložit nastavení ověřování**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit plátno, musí být zřízení do plátno.  
V případě plátno zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **plátno** .

2.  Přejděte na **kurzy \> spravovat účty \> Microsoft**.

    ![Plátno] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Plátno")

3.  Klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Uživatelé")

4.  Klikněte na **Přidat nového uživatele**.

    ![Uživatelé] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Uživatelé")

5.  Na stránce Přidat nového uživatele dialogové okno proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Přidání uživatele")

    1.  Do textového pole **Jméno** zadejte uživatelské jméno.
    2.  **E-mailu** do textového pole zadejte e-mailovou adresu uživatele.
    3.  **Přihlášení** do textového pole zadejte uživatele Azure AD e-mailovou adresu.
    4.  Vyberte **e-mailu uživatele o vytvoření tohoto účtu**.
    5.  Klikněte na **Přidat uživatele**.

>[AZURE.NOTE] Můžete použít jakékoli další plátno uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou plátno k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Přiřazení uživatelů k plátno, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **plátno **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
