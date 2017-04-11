<properties
    pageTitle="Kurz: Azure Active Directory integrace s Everbridge | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Everbridge."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Kurz: Azure Active Directory integrace s Everbridge

Cílem tohoto kurzu je předvedení Everbridge integrace se službou Azure Active Directory (Azure AD).

Integrace Everbridge s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Everbridge
- Povolení uživatelům, aby automaticky získat přihlášení z k Everbridge (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s Everbridge, musíte následující položky:

- Předplatné Azure AD
- Everbridge jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Everbridge z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-everbridge-from-the-gallery"></a>Přidání Everbridge z Galerie
Pokud chcete nakonfigurovat integraci Everbridge do Azure AD, potřebujete přidat Everbridge z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Everbridge z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Everbridge**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_01.png)
7. V podokně výsledků vyberte **Everbridge**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Everbridge založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Everbridge uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Everbridge jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Everbridge.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Everbridge, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Everbridge otestovat uživatele](#creating-a-everbridge-test-user)** – a protějšek Britta Simon Everbridge, která je propojená s Azure AD znázornění ní.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování Everbridge aplikace.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Everbridge, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Everbridge** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Everbridge** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_04.png)

    na. **Identifikátor URI** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://sso.everbridge.net/{<company name>}`

    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce:`https://manager.everbridge.net/saml/SSO/{<company name>}/alias/defaultAlias`

    c. Klikněte na tlačítko **Další**

4. Na stránce **konfigurovat jednotné přihlašování v Everbridge** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_05.png)

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.

5. Získat SSO nakonfigurován pro aplikaci, musíte přihlašování ke tenantovi Everbridge jako správce.

6. V nabídce na horní klikněte na kartu **Nastavení** a vyberte **Jednotné přihlašování** v části **zabezpečení**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)

    na. Do textového pole **název** zadejte název poskytovatele identifikátor (například: název vaší společnosti).

    b. Do textového pole **Rozhraní API název** zadejte název rozhraní API.

    C. Klikněte na tlačítko **Zvolte soubor** k nahrání souboru metadat, který jste stáhli v **kroku 4**.

    d. Jako **Místo identit SAML**vyberte položku "Identity v elementu NameIdentifier příkazu předmět".

    e. Kopírovat SAML SSO adresu URL z Azure AD na **adresu URL přihlašovací zprostředkovatele identit** v Everbridge.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)

    f. Jako **Služby, které iniciuje požádat o vazbu zprostředkovatele**vyberte HTTP přesměrování.

7. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

8. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem v této části je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

V seznamu uživatelů zvolte **Britta Simon**.
    
![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-everbridge-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-everbridge-test-user"></a>Vytvoření uživatele Everbridge test

V této části vytvoříte volali Britta Simon Everbridge uživatele. Zkontrolujte pracovat se na tým podpory Everbridge prostřednictvím <mailto:support@everbridge.com> přidáte uživatele v platformu Everbridge.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Everbridge.
    
![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon Everbridge, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **Everbridge**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_50.png)

3. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Everbridge na panelu aplikace Access můžete by měla získat automaticky přihlášení z Everbridge aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_205.png
