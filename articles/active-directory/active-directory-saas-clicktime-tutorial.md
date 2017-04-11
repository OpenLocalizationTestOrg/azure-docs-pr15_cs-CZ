<properties 
    pageTitle="Kurz: Azure Active Directory integrace s ClickTime | Microsoft Azure" 
    description="Naučte se používat ClickTime s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Kurz: Azure Active Directory integrace s ClickTime

V tomto kurzu se naučíte, jak integrovat ClickTime s Azure Active Directory (Azure AD).

Integrace ClickTime s Azure AD poskytuje následující výhody:

- Můžete určit, v Azure AD, kdo má přístup k ClickTime
- Povolení uživatelům, aby automaticky získat přihlášení z k ClickTime (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ClickTime, musíte následující položky:

- Předplatné Azure AD
- ClickTime jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ClickTime z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení

##<a name="adding-clicktime-from-the-gallery"></a>Přidání ClickTime z Galerie

Cílem tento oddíl je přehledu jak povolit integraci aplikace ClickTime.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ClickTime, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ClickTime**.

    ![Galerie aplikace] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ClickTime**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s ClickTime založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v ClickTime je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v ClickTime jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ClickTime.

Testování a konfigurace Azure AD jednotné přihlašování s ClickTime, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření ClickTime testování uživatele](#creating-a-clicktime-test-user)** – a protějšek Britta Simon ClickTime, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ClickTime s svůj účet v Azure AD pomocí federace na základě SAML protokolu.  


>[AZURE.IMPORTANT] Aby bylo možné pro konfiguraci vašeho klienta ClickTime jednotné přihlašování, budete muset nejdřív kontaktujte technickou podporu ClickTime získat tuto funkci povolit.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s ClickTime, proveďte následující kroky:**

1.  Na portálu na stránce **ClickTime** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ClickTime** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Konfigurace jednotného přihlašování")

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    na. **IdentifierL** do textového pole, zadejte adresu URL pomocí následujícího vzorce: **https://app.clicktime.com/sp/**
    
    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce: **https://app.clicktime.com/Login/**

    c. Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v ClickTime** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Konfigurace jednotného přihlašování")

4.  V okně různých webového prohlížeče Přihlaste se k webu společnosti ClickTime jako správce.

5.  Na panelu nástrojů na horní klikněte na **Předvolby**a potom klikněte na **Nastavení zabezpečení**.

6.  V části **Předvolby přihlašování** konfigurace proveďte následující kroky:

    ![Nastavení zabezpečení] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Nastavení zabezpečení")

    na.  Zaškrtněte políčko **Povolit** přihlášení **Azure AD**pomocí jednoho přihlašování (SSO).
    
    b.  Na portálu na **konfigurovat jednotné přihlašování v ClickTime** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Koncový bod poskytovatele Identity** .

    c.  Otevřete zakódovaný certifikát základu-64 v **poznámkovém bloku**, zkopírujte obsah a potom je vložte do textového pole **X.509 certifikát** .
    
    d.  Klikněte na **Uložit**.

7.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit ClickTime, musí být zřízení do ClickTime.  
V případě ClickTime zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **ClickTime** .

2.  Na panelu nástrojů na horní klikněte na **společnosti**a pak klikněte na **lidé**.

    ![Lidé] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Lidé")

3.  Klikněte na **Přidat uživatele**.

    ![Přidání uživatele] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Přidání uživatele")

4.  V části novou osobu proveďte následující kroky:

    ![Lidé] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Lidé")

    na.  Do textového pole **e-mailová adresa** zadejte e-mailovou adresu účtu Azure AD.
    
    b.  **Úplný název** do textového pole zadejte název účtu Azure AD.  

    >[AZURE.NOTE] Pokud chcete, můžete nastavit další vlastnosti objektu nového uživatele.

    c.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další ClickTime uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ClickTime zřízení Azure AD uživatelské účty.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení přístup ClickTime.

![Přiřazení uživatele][200]

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

**Pokud chcete přiřadit Britta Simon ClickTime, proveďte následující kroky**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **ClickTime**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]

## <a name="testing-single-sign-on"></a>Testování jednotné přihlašování
V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici ClickTime na panelu aplikace Access můžete by měla získat automaticky přihlášení z ClickTime aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png