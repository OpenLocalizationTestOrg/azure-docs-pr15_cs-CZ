<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Projectplace | Microsoft Azure" 
    description="Naučte se používat Projectplace s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Kurz: Azure Active Directory integrace s Projectplace
  
Cílem tohoto kurzu je zobrazit integrace Azure a Projectplace.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Projectplace jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Projectplace (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Projectplace.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Projectplace
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-projectplace-tutorial/IC790217.png "Scénář")
##<a name="enabling-the-application-integration-for-projectplace"></a>Povolení integrace aplikací pro Projectplace
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Projectplace.

###<a name="to-enable-the-application-integration-for-projectplace-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Projectplace, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-projectplace-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-projectplace-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-projectplace-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-projectplace-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Projectplace**.

    ![Galerie aplikace] (./media/active-directory-saas-projectplace-tutorial/IC790218.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Projectplace**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ProjectPlace] (./media/active-directory-saas-projectplace-tutorial/IC790219.png "ProjectPlace")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Projectplace s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Projectplace** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-projectplace-tutorial/IC790220.png "Konfigurace jednoho přihlášení")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Projectplace** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-projectplace-tutorial/IC790221.png "Konfigurace jednoho přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Projectplace na adresu URL** do textového pole, zadejte adresu URL svého ProjectPlace klienta (například: "*http://company.projectplace.com*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-projectplace-tutorial/IC790222.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Projectplace** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat na vašem počítači.

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-projectplace-tutorial/IC790223.png "Konfigurace jednoho přihlášení")

5.  Odešlete metadatafile Projectplace tým podpory.

    >[AZURE.NOTE] Konfigurace přihlašování musí vykonat Projectplace tým podpory. Zobrazí se vám oznámení hned po dokončení konfigurace.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednoho přihlášení] (./media/active-directory-saas-projectplace-tutorial/IC790227.png "Konfigurace jednoho přihlášení")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Projectplace, musí být zřízení do Projectplace.  
V případě Projectplace zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Projectplace** jako správce.

2.  Přejděte na **lidé**a potom kliknutím na **Členové**.

    ![Lidé] (./media/active-directory-saas-projectplace-tutorial/IC790228.png "Lidé")

3.  Klikněte na **Přidat člena**.

    ![Část o přidávání členů] (./media/active-directory-saas-projectplace-tutorial/IC790232.png "Část o přidávání členů")

4.  V části **Přidat člena** proveďte následující kroky:

    ![Nové členy] (./media/active-directory-saas-projectplace-tutorial/IC790233.png "Nové členy")

    1.  **Nových členů** do textového pole zadejte e-mailovou adresu platného AAD účtu, který chcete poskytování do související textová pole.
    2.  Klepněte na tlačítko **Odeslat**

        >[AZURE.NOTE] E-mailu a odkaz na potvrďte účet, než se aktivuje, jsou odeslány majitele účtu služby Azure Active Directory
    
>[AZURE.NOTE]Můžete použít jakékoli další Projectplace uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Projectplace k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-projectplace-perform-the-following-steps"></a>Přiřazení uživatelů k Projectplace, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Projectplace **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-projectplace-tutorial/IC790234.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-projectplace-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).