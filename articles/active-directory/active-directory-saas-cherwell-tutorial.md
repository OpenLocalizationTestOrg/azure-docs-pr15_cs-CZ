<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Cherwell | Microsoft Azure" 
    description="Naučte se používat Cherwell s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="10/14/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Kurz: Azure Active Directory integrace s Cherwell

Cílem tohoto kurzu je zobrazit integrace Azure a Cherwell. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Cherwell jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Cherwell (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Cherwell.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Cherwell
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-cherwell-tutorial/IC798988.png "Scénář")
##<a name="enabling-the-application-integration-for-cherwell"></a>Povolení integrace aplikací pro Cherwell

Cílem tento oddíl je přehledu jak povolit integraci aplikace Cherwell.

###<a name="to-enable-the-application-integration-for-cherwell-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Cherwell, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-cherwell-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-cherwell-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-cherwell-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-cherwell-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Cherwell**.

    ![Cherwell] (./media/active-directory-saas-cherwell-tutorial/IC798989.png "Cherwell")

7.  V podokně výsledků vyberte **Cherwell**a potom klikněte na **Dokončit** přidat aplikaci.
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

    ![Cherwell](./media/active-directory-saas-cherwell-tutorial/IC798996.png "Cherwell")

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Cherwell s účtem v Azure AD pomocí federaci podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Cherwell** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cherwell-tutorial/IC798990.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Cherwell** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cherwell-tutorial/IC798991.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-cherwell-tutorial/IC798992.png "Konfigurace aplikace URL")

    na.  **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané uživatelů a přihlaste se do **Cherwell** (například: *https://\<název společnosti\>.cherwellondemand.com/cherwellclient*).

    b.  Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v Cherwell** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cherwell-tutorial/IC798993.png "Konfigurace jednotného přihlašování")

    na.  Klikněte na tlačítko **Stáhnout certifikát**a uložte místně certifikát v počítači.

    b.  Zkopírujte **adresu URL zprostředkovatele identit**.

    c.  Zkopírujte adresu **URL služby jednotného přihlašování**.

    d.  Klikněte na tlačítko **Další**.

5.  Odeslání stažený certifikát **Adresu URL zprostředkovatele identit** a **Jeden přihlašování adresy URL služby** pro váš tým podpory Cherwell.

    >[AZURE.NOTE] Váš tým podpory Cherwell musí udělejte skutečné konfigurace jednotného přihlašování.
Zobrazí se vám oznámení při přihlašování povolil pro vaše předplatné.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-cherwell-tutorial/IC798994.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Cherwell, musí být zřízení do Cherwell.  
V případě Cherwell je potřeba vytvořit tak, že váš tým podpory Cherwell uživatelských účtů.

>[AZURE.NOTE] Můžete použít jakékoli další Cherwell uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Cherwell poskytování služby Azure Active Directory uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-cherwell-perform-the-following-steps"></a>Přiřazení uživatelů k Cherwell, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Cherwell** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-cherwell-tutorial/IC798995.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-cherwell-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
