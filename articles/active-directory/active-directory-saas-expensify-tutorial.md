<properties
    pageTitle="Kurz: Azure Active Directory integrace s Expensify | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Expensify."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Kurz: Azure Active Directory integrace s Expensify

V tomto kurzu se naučíte, jak integrovat Expensify s Azure Active Directory (Azure AD).

Integrace Expensify s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Expensify
- Povolení uživatelům, aby automaticky získat přihlášení z k Expensify (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s Expensify, musíte následující položky:

- Předplatné Azure AD
- Expensify jednotné přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Expensify z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-expensify-from-the-gallery"></a>Přidání Expensify z Galerie
Pokud chcete nakonfigurovat integraci Expensify do Azure AD, potřebujete přidat Expensify z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Expensify z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Expensify**.
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_01.png)

7. V podokně výsledků vyberte **Expensify**a potom klikněte na **Dokončit** přidat aplikaci.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Expensify založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Expensify je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Expensify jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Expensify.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Expensify, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření Expensify testování uživatele](#creating-an-expensify-test-user)** – a protějšek Britta Simon Expensify, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Expensify.


**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Expensify, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Expensify** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Expensify** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_04.png) 


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Expensify pomocí následujícího vzorce: **"https://www.expensify.com/authentication/saml/login"**.


4. Na stránce **konfigurovat jednotné přihlašování v Expensify** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_05.png) 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5. Chcete-li povolit jednotné přihlašování v Expensify musíte nejdřív povolení **Ovládacího prvku domény** v aplikaci. V aplikaci můžete povolit ovládací prvek domény. Postup najdete [tady](http://help.expensify.com/domain-control). Další podporu můžete kontaktovat podporu Expensify prostřednictvím [help@expensify.com](mailto:help@expensify.com). Až budete mít povolený ovládací prvek domény, postupujte takto:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png) 

    na. Přihlaste se do aplikace Expensify.
   
    b. Na panelu nástrojů na horní klikněte na **Správce**.

    c. V levém podokně klikněte na **Ovládací prvek domény**.

    d. Klikněte na své jméno ověřenou doménou.

    e. V levém podokně klikněte na **SAML**a vyberte **Povolit**.

    f. Stažený federace Metadata Open z Azure AD, zkopírujte obsah a potom je vložte do textového pole **Metadat poskytovatele Identity** .

  
6. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-expensify-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-expensify-test-user"></a>Vytvoření uživatele test Expensify

V této části vytvoříte uživatele s názvem Britta Simon v Expensify. Zkontrolujte pracujte se na tým podpory Expensify přidáte uživatele v platformu Expensify.

> [AZURE.NOTE] Pokud potřebujete k vytvoření uživatele ručně, budete muset kontaktovat tým podpory Expensify.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Expensify.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Expensify, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Expensify**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Expensify na panelu aplikace Access můžete by měla získat automaticky přihlášení z Expensify aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_205.png
