<properties
    pageTitle="Kurz: Azure Active Directory integrace s Origami | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Origami."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-origami"></a>Kurz: Azure Active Directory integrace s Origami

V tomto kurzu se naučíte, jak integrovat Origami s Azure Active Directory (Azure AD).

Integrace Origami s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Origami
- Povolení uživatelům, aby automaticky získat přihlášení z k Origami (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Origami, musíte následující položky:

- Předplatné Azure AD
- Origami jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Origami z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-origami-from-the-gallery"></a>Přidání Origami z Galerie
Pokud chcete nakonfigurovat integraci Origami do Azure AD, potřebujete přidat Origami z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Origami z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Origami**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/tutorial_origami_01.png)
7. V podokně výsledků vyberte **Origami**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/tutorial_origami_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Origami založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Origami je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Origami jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Origami.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Origami, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Origami testování uživatele](#creating-a-origami-test-user)** – a protějšek Britta Simon Origami, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Origami.


**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Origami, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Origami** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Origami** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Origami pomocí následujícího vzorce: **https://live.origamirisk.com/origami/account/login?account=\<název společnosti\> **
    
    b. Klikněte na tlačítko **Další**
 
4. Na stránce **konfigurovat jednotné přihlašování v Origami** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.



1. Přihlaste se k účtu Origami s oprávněními správce.

1. V nabídce na horní klikněte na **Správce**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)
  

1. Na stránce dialogové okno jeden znak v nastavení proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/123.png)

    na. Zaškrtněte políčko **Povolit jednotného přihlášení na**.

    b. Na portálu Azure klasické zkopírujte adresu **URL SAML jednotného přihlašování**a potom je vložte do textového pole **Zprostředkovatele identit adresa URL přihlašovací stránky** .

    c. Na portálu Azure klasické zkopírujte **Jednoho SIGN OUT adresy URL služby**a potom je vložte do textového pole **Adresa URL zprostředkovatele identit Sign-out stránky** .

    d. Klikněte na tlačítko **Procházet** k nahrání na certifikát, který jste stáhli z portálu Microsoft Azure klasické.

    e. Klikněte na **Uložit změny**.


6. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-origami-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-origami-test-user"></a>Vytvoření uživatele test Origami

V této části vytvoříte uživatele s názvem Britta Simon v Origami. 

1. Přihlaste se k účtu Origami s oprávněními správce.

2. V nabídce na horní klikněte na **Správce**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. V dialogovém okně **uživatelů a zabezpečení** klikněte na **uživatele**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. Klikněte na **Přidat nového uživatele**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. V dialogovém okně Přidat nového uživatele proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    na. Do textového pole **Uživatelské jméno** zadejte uživatelské jméno Britta Simon Azure klasické portálu.

    b. Do textového pole **heslo** zadejte passwotd.

    c. V textovém poli **Potvrdit heslo** zadejte heslo znovu.

    d. Do textového pole **jméno** zadejte **Britta**.

    e. **Příjmení** do textového pole zadejte **Simon**.

    f. Klikněte na **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

1. Přiřazení **Role uživatelů** a **Klientského přístupu** pro uživatele. 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Origami.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Origami, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Origami**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-origami-tutorial/tutorial_origami_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Origami na panelu aplikace Access můžete by měla získat automaticky přihlášení z Origami aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-origami-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-origami-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-origami-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-origami-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-origami-tutorial/tutorial_general_205.png
