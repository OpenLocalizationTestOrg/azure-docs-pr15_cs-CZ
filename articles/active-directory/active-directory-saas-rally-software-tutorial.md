<properties 
    pageTitle="Kurz: Azure Active Directory integrace Mítink softwarem | Microsoft Azure" 
    description="Naučte se používat Software technologie Rally s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Kurz: Azure Active Directory integrace Mítink softwarem
  
Cílem tohoto kurzu je zobrazit integrace Azure a technologie Rally softwaru.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Technologie Rally Software klienta
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro technologie Rally Software
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Scénář")
##<a name="enabling-the-application-integration-for-rally-software"></a>Povolení integrace aplikací pro technologie Rally Software
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace technologie Rally softwaru.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace technologie Rally softwaru, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **mítink**.

    ![Galerie aplikace] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Technologie Rally Software**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Technologie Rally Software] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Technologie Rally Software")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit technologie Rally softwaru s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu musíte nahrajete certifikát technologie Rally Software.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Technologie Rally Software **aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jak chcete uživatelům přihlásit k Mítink** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD jednotné přihlášení] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD jednotné přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Technologie Rally Software klienta URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. rally.com*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Mítink** klikněte na stáhnout metadat a uložte ho ve vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Konfigurace jednotného přihlašování")

5.  Přihlaste se k vašemu tenantovi **Technologie Rally Software** .

6.  Na panelu nástrojů na horní klikněte na **Nastavení**a vyberte **předplatné**.

    ![Předplatné] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Předplatné")

7.  Klikněte na tlačítko **Akce** na panelu nástrojů na horní na pravé straně a potom vyberte **Upravit předplatné**.

8.  Na stránce **předplatné** dialogové okno proveďte následující kroky a potom klikněte na **Uložit a zavřít**:

    ![Ověřování] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Ověřování")

    1.  Ověřování rozevíracím seznamu vyberte **ověřování Mítink nebo jednotné přihlašování**
    2.  Na portálu na **konfigurovat jednotné přihlašování v článku Software technologie Rally** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **Adresa URL poskytovatele Identity**
    3.  Na portálu na **konfigurovat jednotné přihlašování v článku Software technologie Rally** dialogové okno stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** .

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pro AAD aby uživatelé mohli přihlásit musí být zřízení do aplikace technologie Rally Software pomocí jejich jména uživatelů Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi technologie Rally Software.

2.  Přejděte na **Nastavení \> uživatelé**a potom klikněte na tlačítko **+ Přidat nový**.

    ![Uživatelé] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Uživatelé")

3.  Zadejte název do textového pole nový uživatel a klikněte na tlačítko **Přidat podrobnosti**.

4.  V části **Vytvořit uživatele** proveďte následující kroky:

    ![Vytvoření uživatele] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Vytvoření uživatele")

    1.  Do textového pole **Uživatelské jméno** zadejte jméno Azure AD uživatele, kterého chcete poskytování.
    2.  Do textového pole **E-mailová adresa** zadejte e-mailovou adresu, kterou chcete poskytování uživatele Azure AD.
    3.  Klikněte na **Uložit a zavřít**.

>[AZURE.NOTE]Můžete použít jakékoli další technologie Rally Software uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou technologie Rally Software k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Přiřazení uživatelů k technologie Rally Software, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Technologie Rally Software** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).




