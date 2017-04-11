<properties
    pageTitle="Kurz: Azure Active Directory integrace s ITRP | Microsoft Azure" 
    description="Naučte se používat ITRP s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Kurz: Azure Active Directory integrace s ITRP
  
Cílem tohoto kurzu je zobrazit integrace Azure a ITRP.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   ITRP klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti ITRP (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili ITRP.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro ITRP
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Scénář")
##<a name="enabling-the-application-integration-for-itrp"></a>Povolení integrace aplikací pro ITRP
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace ITRP.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ITRP, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ITRP**.

    ![Galerie aplikace] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ITRP**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ITRP s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro ITRP vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **ITRP** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ITRP** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **ITRP znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. ITRP.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v ITRP** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\ITRP.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti ITRP jako správce.

6.  Na panelu nástrojů na horní klikněte na **Nastavení**.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  V levém navigačním podokně vyberte **Jednotného přihlašování**.

    ![Jednotné přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Jednotné přihlašování")

8.  V jednotné přihlašování části konfigurace proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Jednotné přihlašování")

    ![Jednotné přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Jednotné přihlašování")

    1.  Klikněte na **Povolit**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v ITRP** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL vzdálené odhlásit** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v ITRP** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotné přihlašování URL** a potom je vložte do textového pole **URL SAML jednotné přihlašování** .
    4.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikát** .
        
        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    5.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit ITRP, musí být zřízení do ITRP.  
V případě ITRP zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **ITRP** .

2.  Na panelu nástrojů na horní klikněte na **záznamy**.

    ![Správce] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Správce")

3.  V místní nabídce vyberte **lidé**.

    ![Lidé] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Lidé")

4.  Klikněte na **Přidat novou osobu** ("a").

    ![Správce] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Správce")

5.  V dialogovém okně Přidat novou osobu proveďte následující kroky:

    ![Uživatel] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Uživatel")

    1.  Zadejte **jméno**, **e-mailu** platného AAD účtu, který chcete poskytování.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další ITRP uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ITRP k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Přiřazení uživatelů k ITRP, proveďte následující kroky:

1.  Na portálu Azure AD vytvořte testovací účet.

2.  Na stránce integrace **ITRP **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).