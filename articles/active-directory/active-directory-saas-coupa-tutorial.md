<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Coupa | Microsoft Azure" 
    description="Naučte se používat Coupa s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-coupa"></a>Kurz: Azure Active Directory integrace s Coupa

Cílem tohoto kurzu je zobrazit integrace Azure a Coupa.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Coupa jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Coupa.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Coupa
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-coupa-tutorial/IC791897.png "Scénář")
##<a name="enabling-the-application-integration-for-coupa"></a>Povolení integrace aplikací pro Coupa

Cílem tento oddíl je přehledu jak povolit integraci aplikace Coupa.

###<a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Coupa, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-coupa-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-coupa-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-coupa-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-coupa-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Coupa**.

    ![Galerie aplikace] (./media/active-directory-saas-coupa-tutorial/IC791898.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Coupa**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Coupa] (./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Coupa s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Coupa vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti Coupa jako správce.

2.  Přejděte na **Nastavení \> řízení zabezpečení**.

    ![Ovládací prvky zabezpečení] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Ovládací prvky zabezpečení")

3.  Stáhněte si soubor metadat Coupa do počítače, klikněte na **Stáhnout a import SP metadat**.

    ![Metadata Coupa SP] (./media/active-directory-saas-coupa-tutorial/IC791901.png "Metadata Coupa SP")

4.  V jiném okně prohlížeče Přihlaste se k portálu Azure klasické.

5.  Na stránce integrace **Coupa** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-coupa-tutorial/IC791902.png "Konfigurace jednotného přihlašování")

6.  Na stránce **jakým způsobem uživatelé přihlásit k Coupa** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-coupa-tutorial/IC791903.png "Konfigurace jednotného přihlašování")

7.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-coupa-tutorial/IC791904.png "Konfigurace aplikace URL")

    1.  **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Coupa (například: "*http://company.Coupa.com*").
    2.  Stažený soubor metadat Coupa otevřít a zkopírujte **AssertionConsumerService index nebo adresu URL**.
    3.  V textovém poli **URL odpověď Coupa** vložte hodnotu **AssertionConsumerService index nebo adresu URL** .
    4.  Klikněte na tlačítko **Další**.

8.  Na stránce **konfigurovat jednotné přihlašování v Coupa** stáhnout soubor metadata, klikněte na tlačítko **Stáhnout metadat**a uložte jej místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-coupa-tutorial/IC791905.png "Konfigurace jednotného přihlašování")

9.  Na webu společnosti Coupa, přejděte na **Nastavení \> řízení zabezpečení**.

    ![Ovládací prvky zabezpečení] (./media/active-directory-saas-coupa-tutorial/IC791900.png "Ovládací prvky zabezpečení")

10. V části **přihlášení pomocí přihlašovacích údajů Coupa** proveďte následující kroky:

    ![Přihlaste se pomocí přihlašovacích údajů Coupa] (./media/active-directory-saas-coupa-tutorial/IC791906.png "Přihlaste se pomocí přihlašovacích údajů Coupa")

    1.  Vyberte, **Přihlaste se pomocí SAML**.
    2.  Klikněte na tlačítko **Procházet** a nahrajte stažený soubor Azure Active metadata.
    3.  Klikněte na **Uložit**.

11. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-coupa-tutorial/IC791907.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Coupa, musí být zřízení do Coupa.  
V případě Coupa zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Coupa** jako správce.

2.  V nabídce v horní klikněte na **Nastavení**a potom klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-coupa-tutorial/IC791908.png "Uživatelé")

3.  Klikněte na **vytvořit**.

    ![Vytvoření uživatele] (./media/active-directory-saas-coupa-tutorial/IC791909.png "Vytvoření uživatele")

4.  V části **Vytvoření uživatele** proveďte následující kroky:

    ![Podrobné informace o uživateli] (./media/active-directory-saas-coupa-tutorial/IC791910.png "Podrobné informace o uživateli")

    1.  Zadejte **přihlašovací**, **křestního jména**, **Příjmení**, **ID přihlášení**, **e-mailu** atributy platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Klikněte na **vytvořit**.

    >[AZURE.NOTE] Majitele účtu služby Azure Active Directory pošle e-mail s odkazem pro potvrzení účtu před z něj stal aktivní.

>[AZURE.NOTE] Můžete použít jakékoli další Coupa uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Coupa k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-coupa-perform-the-following-steps"></a>Přiřazení uživatelů k Coupa, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Coupa **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-coupa-tutorial/IC791911.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-coupa-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
