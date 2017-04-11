<properties 
    pageTitle="Kurz: Azure Active Directory integrace s životního cyklu SCC | Microsoft Azure" 
    description="Naučte se používat životního cyklu SCC s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Kurz: Azure Active Directory integrace s SCC cyklus
  
Cílem tohoto kurzu je zobrazit integrace Azure a životního cyklu SCC.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Životního cyklu SCC jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili životního cyklu SCC bude moct jednotného přihlášení do aplikace na webu společnosti životního cyklu SCC (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro SCC cyklus
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scénář")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>Povolení integrace aplikací pro SCC cyklus
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace životního cyklu SCC.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>Postup povolení integrace aplikace pro SCC životního cyklu, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Životního cyklu SCC**.

    ![Galerie aplikace] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Životního cyklu SCC**a potom klikněte na **Dokončit** přidat aplikaci.

    ![SCC cyklus] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC cyklus")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit životního cyklu SCC s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Životního cyklu SCC** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k životního cyklu SCC** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit na adresu URL** do textového pole zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci životního cyklu SCC pomocí následujícího vzorce "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v životního cyklu SCC** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Konfigurace jednotného přihlašování")

5.  Přepošlete tento soubor metadat pro tým SCC životního cyklu podpory.

    >[AZURE.NOTE]Jednotné přihlašování musí povolit tak, že tým podpory životního cyklu SCC.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit SCC životního cyklu, musí být zřízení do životního cyklu SCC.
  
Není žádná akce položka pro konfiguraci zřizování uživatele k životního cyklu SCC.  
Pokud přiřazené uživatel se pokusí přihlásit SCC životního cyklu, životního cyklu SCC účet se automaticky vytvoří v případě potřeby.

>[AZURE.NOTE]Můžete použít libovolný další SCC cyklus uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou životního cyklu SCC k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Přiřazení uživatelů k SCC životního cyklu, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Životního cyklu SCC **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).