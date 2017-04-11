<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Citrix ShareFile | Microsoft Azure" 
    description="Naučte se používat Citrix ShareFile s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Kurz: Azure Active Directory integrace s Citrix ShareFile

Cílem tohoto kurzu je zobrazit integrace Azure a Citrix ShareFile.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Citrix ShareFile klienta

Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili Citrix ShareFile bude moct jednotného přihlášení do aplikace na webu společnosti Citrix ShareFile (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Citrix ShareFile
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scénář")
##<a name="enabling-the-application-integration-for-citrix-sharefile"></a>Povolení integrace aplikací pro Citrix ShareFile

Cílem tento oddíl je přehledu jak povolit integraci aplikace Citrix ShareFile.

###<a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace Citrix ShareFile, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Citrix ShareFile**.

    ![Galerie aplikace] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Citrix ShareFile**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Citrix ShareFile] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Citrix ShareFile s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Citrix ShareFile** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Citrix ShareFile** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Citrix ShareFile znak na adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce `https://<tenant-name>.shareFile.com`a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Citrix ShareFile** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![ConfigureSingle jednotné přihlášení] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle jednotné přihlášení")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti **Citrix ShareFile** jako správce.

6.  Na panelu nástrojů na horní klikněte na **Správce**.

7.  V levém navigačním podokně vyberte **Konfigurace jednotného přihlašování**.

    ![Správa účtu] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Správa účtu")

8.  Na **jednotné přihlašování / SAML 2.0 konfigurace** dialogovou stránku v části **Základní nastavení**, proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Jednotné přihlašování")

    1.  Zaškrtněte políčko **Povolit SAML**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Citrix ShareFile** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do **Your Vystavitel IDP / Entity ID** textového pole.
    3.  Na portálu na **konfigurovat jednotné přihlašování v Citrix ShareFile** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    4.  Na portálu v **konfigurovat jednotné přihlašování v Citrix ShareFile** dialogovou stránku Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlásit** .
    5.  Klikněte na **změnit** vedle pole **X.509 certifikátu** a pak nahrajte certifikát, který jste si stáhli z portálu klasické Azure AD.
        ![Základní nastavení] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Základní nastavení")

9.  Na portálu Správa Citrix ShareFile klikněte na **Uložit** .

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Citrix ShareFile, musí být zřízení do Citrix ShareFile.  
V případě Citrix ShareFile zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Citrix ShareFile** .

2.  Klikněte na **Správa uživatelů \> Správa uživatelů Domů \> + vytvořit zaměstnance**.

    ![Vytvoření zaměstnance] (./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Vytvoření zaměstnance")

3.  Zadejte **e-mailu**, **křestního jména** a **Příjmení** z platné Azure AD účtu, který chcete poskytování.

    ![Základní informace] (./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Základní informace")

4.  Klikněte na **Přidat uživatele**.

    >[AZURE.NOTE] Majitele účtu AAD dostávat e-mailu a potvrďte svůj účet, než se aktivuje, klepnutím na odkaz.

>[AZURE.NOTE] Můžete použít jakékoli další Citrix ShareFile uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Citrix ShareFile k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>Přiřazení uživatelů k Citrix ShareFile, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Citrix ShareFile **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
