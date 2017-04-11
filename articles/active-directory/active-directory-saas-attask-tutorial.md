<properties
    pageTitle="Kurz: Azure Active Directory integrace s @Task| Microsoft Azure"
    description="Zjistěte, jak konfigurovat jednotné přihlašování mezi Azure Active Directory a @Task."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Kurz: Azure Active Directory integrace se službou@Task

Cílem tohoto kurzu je ukazují, jak integrovat @Task službou Azure Active Directory (Azure AD).  
Integrace @Task s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k@Task
- Povolení uživatelům automaticky získat přihlášení na k @Task (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Konfigurace Azure AD integrace s @Task, potřebujete následující položky:

- Předplatné Azure AD
- @Task Jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání @Task z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-task-from-the-gallery"></a>Přidání @Task z Galerie
Nakonfigurovat integraci @Task do Azure AD, budete muset přidat @Task z Galerie do seznamu spravované SaaS aplikace.

**Chcete-li přidat @Task z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1] 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2] 

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3] 

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4] 

6. Do pole Hledat zadejte **@Task**.

    ![Aplikace][5] 

7. V podokně výsledků vyberte **@Task**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení

Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s @Task založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pro práci, potřebujete vědět, jaké protějšek uživatele v Azure AD @Task uživateli v Azure AD je. Jinými slovy, odkaz relaci mezi Azure Active Directory a související uživatele v @Task třeba stanovit.   
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v @Task.
 
Nastavení a otestování Azure AD jednotné přihlašování s @Task, musíte udělat následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření @Tasktest uživatele](#creating-a-halogen-software-test-user) ** - mít protějšek Britta Simon v @Taskthat je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotné přihlašování ve vaší @Task aplikace.

**Konfigurace Azure AD jednotné přihlašování s @Task, proveďte následující kroky:**

1. Na portálu Azure klasické na **@Task** integraci aplikace stránky, klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. V **jakým způsobem uživatelé přihlášení ke @Task ** stránky, vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][7] 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace nastavení aplikace][8] 
 
     na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané uživatelů k přihlašování k vaší @Task aplikace (například:*https://<Tenant name>.attask ondemand.com*).

     b. Klikněte na tlačítko **Další**.

4. V **Konfigurace jednotného přihlašování v @Task ** stránky, klikněte na tlačítko **Stáhnout metadata**, uložit metadata místně na vašem počítači a klikněte na tlačítko **Další**.

    ![Co je Azure AD Connect][9] 



1. Přihlášení k vaší @Task společnosti jako správce.

2. Přejděte na **jednotné přihlašování konfigurace**.


1. V dialogovém okně **Jednotného přihlašování** proveďte následující kroky

    ![Konfigurace jednotného přihlašování][23]

    na. Jako **Typ**vyberte **SAML 2.0**.

    b. Vyberte **ID poskytovatele služeb**.

    c. Na portálu Azure klasické zkopírujte **Vzdálené přihlašovací adresa URL**a potom je vložte do textového pole **Přihlašovací adresa URL portálu** .

    d. Na portálu Azure klasické zkopírujte **Jedné adresy URL služby Sign-Out**a potom je vložte do textového pole **Adresa URL Sign-Out** .

    e. Na portálu Azure klasické zkopírujte adresu **URL změnit heslo**a potom je vložte do textového pole **Heslo změnit adresu URL** .

    f. Klikněte na **Uložit**.

6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Co je Azure AD Connect][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Co je Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-an-task-test-user"></a>Vytvoření @Task testovací uživatelské jméno

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon @Task.


**Vytvoření uživatele s názvem Britta Simon @Task, proveďte následující kroky:**

1. Přihlaste se k vaší @Task společnosti jako správce.

2. V nabídce na horní klikněte na **lidé**.

3. Klikněte na **novou osobu**. 

4. V dialogovém okně nové osoby proveďte následující kroky:

    ![Vytvoření @Task testovací uživatelské jméno][21] 

    na. Do textového pole **jméno** zadejte "Britta".

    b. **Příjmení** do textového pole zadejte "Simon".

    c. Do textového pole **E-mailová adresa** zadejte Britta Simon e-mailovou adresu v Azure Active Directory.

    d. Klikněte na **Přidat uživatele**.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k @Task.

![Přiřazení uživatele][200] 

**Přiřazení Britta Simon k @Task, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **@Task**.

    ![Přiřazení uživatele][202] 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí @Task dlaždice na panelu aplikace Access by měla získat automaticky přihlášení z k vaší @Task aplikace.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






