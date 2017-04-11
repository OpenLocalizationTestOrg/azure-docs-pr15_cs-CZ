<properties 
    pageTitle="Kurz: Azure Active Directory integrací Aha! | Microsoft Azure" 
    description="Naučte se používat Aha! službou Azure Active Directory, chcete-li povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Kurz: Azure Active Directory integrací Aha!

Cílem tohoto kurzu je zobrazíte integrace Azure a Aha!  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Podívejme! jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, uživatelům Azure AD jste přiřadili Aha! budou moct jednotného přihlášení do aplikace na vaše Aha! web společnosti (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Aha!
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-aha-tutorial/IC798944.png "Scénář")
##<a name="enabling-the-application-integration-for-aha"></a>Povolení integrace aplikací pro Aha!

Cílem tento oddíl je přehledu jak povolit integraci aplikace Aha!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace Aha!, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-aha-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-aha-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Aha!**.

    ![Galerie aplikace] (./media/active-directory-saas-aha-tutorial/IC798945.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Aha!**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Podívejme!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Podívejme!")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Aha! s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu Azure klasické na **Aha!** Integrace aplikace stránky, klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-aha-tutorial/IC798946.png "Konfigurace jednotného přihlašování")

2.  V **jakým způsobem uživatelé přihlásit k Aha!** stránka, vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-aha-tutorial/IC798947.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v **Aha! Přihlaste se na adresu URL** textové pole, zadejte adresu URL používá uživatelů k přihlašování k vaší Aha! Aplikace (například: "*https://company.aha.io/session/new*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-aha-tutorial/IC798948.png "Konfigurace aplikace URL")

4.  V **Konfigurace jednotného přihlašování v Aha!** stránka ke stažení souboru metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-aha-tutorial/IC798949.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k vaší Aha! web společnosti jako správce.

6.  V nabídce na horní klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-aha-tutorial/IC798950.png "Nastavení")

7.  Klepněte na **účet**.

    ![Profil] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profil")

8.  Klikněte na **zabezpečení a jednotné přihlašování**.

    ![Zabezpečení a jednotné přihlašování] (./media/active-directory-saas-aha-tutorial/IC798952.png "Zabezpečení a jednotné přihlašování")

9.  V části **Jednotného přihlašování** jako **Zprostředkovatele identit**, vyberte **SAML2.0**.

    ![Zabezpečení a jednotné přihlašování] (./media/active-directory-saas-aha-tutorial/IC798953.png "Zabezpečení a jednotné přihlašování")

10. Na **Jednotné přihlašování** stránce konfigurace proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-aha-tutorial/IC798954.png "Jednotné přihlašování")

    1.  Do textového pole **název** zadejte název pro konfiguraci.
    2.  Pro **použití konfigurovat**vyberte **Soubor metadat**.
    3.  Nahrajte soubor stažený metadata, klikněte na **Procházet**.
    4.  Klikněte na **Aktualizovat**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-aha-tutorial/IC798955.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Aha!, musí být zřízení do Aha!.  
V případě Aha!, zřizování je automatické úkolu.  
Existuje žádné úkol za vás.
  
Automatické vytvoření uživatelů v případě potřeby při prvním jednoho přihlašování pokusu o.

>[AZURE.NOTE] Můžete použít jiné Aha! Nástroje pro vytvoření účtu uživatele nebo rozhraní API poskytovanou Aha! zřízení AAD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Přiřazení uživatelů k Aha!, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  V **Podívejme!** Integrace aplikace klepněte na tlačítko **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-aha-tutorial/IC798956.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-aha-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
