<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Syncplicity | Microsoft Azure" 
    description="Naučte se používat Syncplicity s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Kurz: Azure Active Directory integrace s Syncplicity
  
Cílem tohoto kurzu je ukazují, jak nastavit jednotné přihlašování mezi Azure Active Directory (Azure AD) a Syncplicity.
  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Syncplicity klienta
  
Po dokončení tohoto kurzu, Azure AD uživatelů, kterým máte přiřadit Syncplicity přístup bude moct jeden znak do aplikace na webu společnosti Syncplicity (service provider, které iniciuje přihlášení) nebo pomocí panelů Azure AD přístup.

1.  Povolení integrace aplikací pro Syncplicity
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Scénář")

##<a name="enabling-the-application-integration-for-syncplicity"></a>Povolení integrace aplikací pro Syncplicity
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Syncplicity.

###<a name="to-enable-the-application-integration-for-syncplicity-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Syncplicity, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Syncplicity**.

    ![Galerie Syncplicity aplikace] (./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Galerie Syncplicity aplikace")

7.  V podokně výsledků vyberte **Syncplicity**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Tato část popisuje, jak povolit uživatelům ověřit Syncplicity s účtem služby Azure Active Directory pomocí federace na základě SAML protokolu.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Syncplicity** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Syncplicity** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD jednotné přihlášení] (./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD jednotné přihlášení")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Syncplicity znak v adrese URL** typ používané uživatelé adresy URL a přihlaste se do aplikace Syncplicity klepněte na tlačítko **Další**. 

    Adresa URL aplikace je svoji adresu URL Syncplicity klienta (například: *http://company.Syncplicity.com*):

    ![Konfigurace aplikace URL] (./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Syncplicity** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně do svého počítače.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Konfigurace jednotného přihlašování")

5.  Přihlaste se k vašemu tenantovi **Syncplicity** .

6.  V nabídce v horní klikněte na **Správce**, vyberte **Nastavení**a potom klikněte na **vlastní doménu a jednotné přihlašování**.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")

7.  Na stránce dialogové okno **Jednoho přihlašování (SSO)** proveďte následující kroky:

    ![Jednotné přihlášení \(jednotné přihlašování\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \(SSO\)")

    1.  **Vlastní domény** do textového pole zadejte název vaší domény.
    2.  Vyberte **Povolit** jako **stav přihlašování**.
    3.  Na portálu na stránce **konfigurovat jednotné přihlašování v Syncplicity** Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **Entity Id** .
    4.  Na portálu na stránce **konfigurovat jednotné přihlašování v Syncplicity** Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL stránky** .
    5.  Na portálu na stránce **konfigurovat jednotné přihlašování v Syncplicity** Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL stránky odhlásit** .
    6.  V **Certifikátů zprostředkovatele identit**klikněte na **Zvolit soubor**a pak nahrajte certifikát, který jste stáhli z portálu Microsoft Azure klasické.
    7.  Klikněte na **Uložit změny**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Potvrzení] (./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Potvrzení")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pro AAD aby uživatelé mohli přihlásit musí být zřízení Syncplicity aplikaci. Tato část popisuje, jak vytvořit AAD uživatelských účtů v Syncplicity.

###<a name="to-provision-a-user-account-to-syncplicity-perform-the-following-steps"></a>Zřízení uživatelského účtu Syncplicity, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Syncplicity** (například: *https://company.Syncplicity.com*).

2.  Klikněte na **Správce** a vyberte **uživatelské účty**.

3.  Klikněte na **Přidat uživatele**.

    ![Správa uživatelů] (./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Správa uživatelů")

4.  Zadejte **e-mailovou adresu** AAD účtu, který chcete vytvořit, vyberte jako **Role** **uživatele** a klikněte na tlačítko **Další**.

    ![Informace o účtu] (./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Informace o účtu")

    >[AZURE.NOTE] Majitele účtu AAD pošle e-mail a odkaz na potvrďte a aktivaci účtu.

5.  Vyberte požadovanou skupinu ve vaší firmě, aby nový uživatel by měl stát se jejím členem a klikněte na tlačítko **Další**.

    ![Členství ve skupinách] (./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Členství ve skupinách")

    >[AZURE.NOTE] Pokud jsou uvedené žádné skupiny, jednoduše klikněte na **Další**.

6.  Vyberte složky, které chcete umístit pod kontrolou Syncplicity je na počítači uživatele a klikněte na tlačítko **Další**.

    ![Syncplicity složky] (./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity složky")

>[AZURE.NOTE] Můžete použít jakékoli další Syncplicity uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Syncplicity k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-syncplicity-perform-the-following-steps"></a>Přiřazení uživatelů k Syncplicity, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Syncplicity** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

