<properties
    pageTitle="Kurz: Azure Active Directory integrace s TargetProcess | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TargetProcess."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Kurz: Azure Active Directory integrace s TargetProcess

Cílem tohoto kurzu je vidíte, jak integrovat TargetProcess s Azure Active Directory (Azure AD).

Integrace TargetProcess s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k TargetProcess 
- Povolení uživatelům, aby automaticky získat přihlášení na k TargetProcess (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure Active Directory

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí TargetProcess, musíte následující položky:

- Předplatné Azure AD
- TargetProcess jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání TargetProcess z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-targetprocess-from-the-gallery"></a>Přidání TargetProcess z Galerie
Pokud chcete nakonfigurovat integraci TargetProcess do Azure AD, potřebujete přidat TargetProcess z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat TargetProcess z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **TargetProcess**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_01.png)

7. V podokně výsledků vyberte **TargetProcess**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_10.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí TargetProcess založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému TargetProcess uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v TargetProcess jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v TargetProcess.
 
Testování a konfigurace Azure AD jednotné přihlašování s TargetProcess, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření TargetProcess testování uživatele](#creating-a-targetprocess-test-user)** – a protějšek Britta Simon TargetProcess, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci TargetProcess. V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64. Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

Konfigurace jednotného přihlašování pro TargetProcess, musíte registrované domény. Pokud nemáte registrované domény ještě, kontakt vaší TargetProcess podpoře prostřednictvím [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s TargetProcess, proveďte následující kroky:**

1. V klasickém portálu Azure AD na stránce **TargetProcess** aplikace integrace klikněte **Konfigurace jednotného přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6]

2. Na stránce **jakým způsobem uživatelé přihlásit k TargetProcess** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_02.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_03.png) 


    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace TargetProcess (například: *https://fabrikam.TargetProcess.com/*).

    b. Klikněte na tlačítko **Další**.
 
 
4. Na stránce **konfigurovat jednotné přihlašování v TargetProcess** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_04.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


1. Přihlašování do aplikace TargetProcess jako správce.


1. V nabídce na horní klepněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

1. Klikněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

1. Klikněte na **jednotné přihlašování**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

1. Na jednotné přihlašování dialogové okno nastavení proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png) 

    na. Zaškrtněte políčko **Povolit jednotného přihlašování**.

    b. Na portálu na stránce **konfigurovat jednotné přihlašování v TargetProcess** Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL** .

    c. Otevřete stažený certifikát v poznámkovém bloku, zkopírujte obsah a potom je vložte do textového pole **certifikát** .

    d. Klikněte na tlačítko **Povolit zřizování za běhu**.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.
V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png)  

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-targetprocess-test-user"></a>Vytvoření uživatele TargetProcess test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v TargetProcess.
TargetProcess podporuje za běhu zřizování. Už povolili jste ho v [Konfigurace Azure AD jednotného přihlašování](#configuring-azure-ad-single-single-sign-on).

Neexistuje žádná akce položku, kterou v této části.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k TargetProcess.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon TargetProcess, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **TargetProcess**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_09.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici TargetProcess na panelu aplikace Access můžete by měla získat automaticky přihlášení z TargetProcess aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_205.png






