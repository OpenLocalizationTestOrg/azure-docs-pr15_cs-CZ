<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Thirdlight | Microsoft Azure" 
    description="Naučte se používat Thirdlight s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Kurz: Azure Active Directory integrace s Thirdlight
  
Cílem tohoto kurzu je zobrazit integrace Azure a Thirdlight.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Thirdlight jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Thirdlight (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Thirdlight.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Thirdlight
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Scénář")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Povolení integrace aplikací pro Thirdlight
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Thirdlight.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Thirdlight, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Thirdlight**.

    ![Galerie aplikace] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Thirdlight**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Thirdlight s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Thirdlight vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Thirdlight** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Thirdlight** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Thirdlight znak v adrese URL** zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Thirdlight (například: "*http://azuresso2.thirdlight.com/*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Thirdlight** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Thirdlight jako správce.

6.  Přejděte na **Konfigurace \> systému správy**a potom klikněte na **SAML2**.

    ![Správa systému] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Správa systému")

7.  V části Konfigurace SAML2 proveďte následující kroky:

    ![SAML jednotné přihlašování] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML jednotné přihlašování")

    1.  Zaškrtněte políčko **Povolit SAML2 jednotného přihlašování**.
    2.  Jako **zdroj pro IdP Metadata**vyberte **Zatížení IdP metadat z XML**.
    3.  Otevřete soubor stažený metadat, zkopírujte obsah a potom je vložte do textového pole **IdP Metadata XML** .
    4.  Klikněte na **Uložit SAML2 nastavení**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Thirdlight, musí být zřízení do Thirdlight.  
V případě Thirdlight zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Thirdlight** jako správce.

2.  Přejděte na kartu **Uživatelé** .

3.  Vyberte **Uživatelé a skupiny**.

4.  Klikněte na tlačítko **Přidat nového uživatele** .

5.  Zadejte **uživatelské jméno, název nebo popis, e-mailu, zvolte přednastavení nebo skupiny nové členy** platného AAD účtu, který chcete k poskytování.

6.  Klikněte na **vytvořit**.

>[AZURE.NOTE] Můžete použít jakékoli další Thirdlight uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Thirdlight k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Přiřazení uživatelů k Thirdlight, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Thirdlight **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).