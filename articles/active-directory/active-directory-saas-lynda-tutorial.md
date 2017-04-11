<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Lynda.com | Microsoft Azure" 
    description="Naučte se používat Lynda.com s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Kurz: Azure Active Directory integrace s Lynda.com
  
Cílem tohoto kurzu je zobrazit integrace Azure a Lynda.com.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Lynda.com klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Lynda.com (služby poskytovatelem, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Lynda.com.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Lynda.com
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-lynda-tutorial/IC781046.png "Scénář")
##<a name="enabling-the-application-integration-for-lyndacom"></a>Povolení integrace aplikací pro Lynda.com
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Lynda.com.

###<a name="to-enable-the-application-integration-for-lyndacom-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Lynda.com, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-lynda-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-lynda-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-lynda-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-lynda-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Lynda.com**.

    ![Galerie aplikace] (./media/active-directory-saas-lynda-tutorial/IC777524.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Lynda.com**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Lynda.com] (./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Lynda.com s účtem v Azure AD pomocí federace podle protokolu SAML.

>[AZURE.IMPORTANT]Aby bylo možné pro konfiguraci vašeho klienta Lynda.com jednotné přihlašování, budete muset nejdřív kontaktujte technickou podporu Lynda.com získat tuto funkci povolit.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Lynda.com** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lynda-tutorial/IC777526.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Lynda.com** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lynda-tutorial/IC777527.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Lynda.com znak v adrese URL** zadejte adresu URL svého Lynda.com klienta (například: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target= https://shib.lynda.com/InCommon*) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-lynda-tutorial/IC781047.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Lynda.com** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lynda-tutorial/IC777529.png "Konfigurace jednotného přihlašování")

5.  Odeslání souboru stažený metadat Lynda.com tým podpory. Tým podpory Lynda.com znamená Konfigurace jednotného přihlašování pro vás.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-lynda-tutorial/IC777530.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Existuje žádné akce položky konfigurace uživatele zřizování Lynda.com.  
Při přiřazený uživatel se pokusí přihlásit Lynda.com pomocí panelu aplikace access, Lynda.com kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Lynda.com.

>[AZURE.NOTE]Můžete použít jakékoli další Lynda.com uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Lynda.com k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-lyndacom-perform-the-following-steps"></a>Přiřazení uživatelů k Lynda.com, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Lynda.com **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-lynda-tutorial/IC777531.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-lynda-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).