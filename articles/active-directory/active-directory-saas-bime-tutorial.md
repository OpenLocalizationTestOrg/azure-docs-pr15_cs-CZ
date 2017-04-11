<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Bime | Microsoft Azure" 
    description="Naučte se používat Bime s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bime"></a>Kurz: Azure Active Directory integrace s Bime

Cílem tohoto kurzu je integrace Azure a Bime zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Bime klienta

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Bime (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Bime.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Bime
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-bime-tutorial/IC775552.png "Scénář")
##<a name="enabling-the-application-integration-for-bime"></a>Povolení integrace aplikací pro Bime

Cílem tento oddíl je přehledu jak povolit integraci aplikace Bime.

###<a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Bime, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-bime-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-bime-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-bime-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-bime-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Bime**.

    ![Galerie aplikace] (./media/active-directory-saas-bime-tutorial/IC775553.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Bime**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Bime] (./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Bime s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Bime vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Bime** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bime-tutorial/IC771709.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Bime** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bime-tutorial/IC775555.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Bime znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Bimeapp.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-bime-tutorial/IC775556.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Bime** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Bime.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bime-tutorial/IC775557.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Bime jako správce.

6.  Na panelu nástrojů klikněte na **Správce**a potom na **účet**.

    ![Správce] (./media/active-directory-saas-bime-tutorial/IC775558.png "Správce")

7.  Na stránce konfigurace účtu proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bime-tutorial/IC775559.png "Konfigurace jednotného přihlašování")

    1.  Zaškrtněte políčko **Povolit SAML ověření**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Bime** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL vzdálené přihlášení** .
    3.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku certifikát** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    4.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bime-tutorial/IC775560.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Bime, musí být zřízení do Bime.  
V případě Bime zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Bime** .

2.  Na panelu nástrojů klikněte na **Správce**a **uživatele**.

    ![Správce] (./media/active-directory-saas-bime-tutorial/IC775561.png "Správce")

3.  V **Seznamu uživatelů**klikněte na **Přidat nového uživatele** ("a").

    ![Uživatelé] (./media/active-directory-saas-bime-tutorial/IC775562.png "Uživatelé")

4.  Na stránce dialogové okno **Podrobnosti uživatele** proveďte následující kroky:

    ![Podrobné informace o uživateli] (./media/active-directory-saas-bime-tutorial/IC775563.png "Podrobné informace o uživateli")

    1.  Zadejte jméno, příjmení, přihlášení, e-mailu platného AAD účtu, který chcete poskytování.
    2.  Klikněte na Uložit.

>[AZURE.NOTE] Můžete použít jakékoli další Bime uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Bime k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-bime-perform-the-following-steps"></a>Přiřazení uživatelů k Bime, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Bime **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-bime-tutorial/IC775564.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-bime-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
