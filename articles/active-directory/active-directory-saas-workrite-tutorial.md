<properties
    pageTitle="Kurz: Azure Active Directory integrace s Workrite | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Workrite."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-workrite"></a>Kurz: Azure Active Directory integrace s Workrite

Cílem tohoto kurzu je vidíte, jak integrovat Workrite s Azure Active Directory (Azure AD).  
Integrace Workrite s Azure AD poskytuje následující výhody: 


- Můžete určit v Azure AD, kdo má přístup k Workrite 
- Povolení uživatelům, aby automaticky získat přihlášení z k Workrite (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí Workrite, musíte následující položky:

- Předplatné Azure AD
- Workrite jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Workrite z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-workrite-from-the-gallery"></a>Přidání Workrite z Galerie
Pokud chcete nakonfigurovat integraci Workrite do Azure AD, potřebujete přidat Workrite z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Workrite z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 
 
    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.
 
    ![Aplikace][4]

6. Do pole Hledat zadejte **Workrite**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **Workrite**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Workrite založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Workrite uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Workrite jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Workrite.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Workrite, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Workrite testování uživatele](#creating-a-halogen-software-test-user)** – a protějšek Britta Simon Workrite, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Workrite.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Workrite, proveďte následující kroky:**

1. Na portálu na stránce **Workrite** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Workrite** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][7] 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
    
    ![Azure AD jednotné přihlášení][8] 
 
     na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané uživatelů k přihlašování k vašemu webu Workrite (například: *https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=1a82b5aa-4dd6-4472-9721-7d0193f59e22*).

     > [AZURE.NOTE] Obraťte se na tým podpory Workrite [support@workrite.co.uk](mailto:support@workrite.co.uk) Pokud neznáte přínosu znaménko na adresu URL.

     b. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v Workrite** proveďte následující kroky:

    ![Azure AD jednotné přihlášení][9] 

    na. Klikněte na Stáhnout certifikát a uložte soubor na svém počítači.

    b. Kontaktujte tým podpory Workrite [support@workrite.co.uk](mailto:support@workrite.co.uk), peovide je s stažený certifikátů, adresu **URL Vystavitel** (Entity ID), **Jednoho přihlašování adresy URL služby**, **Jednoho Sign-Out URL**a potom zeptejte se na nastavení jednotného přihlašování k aplikaci Workrite. 

    c. Klikněte na tlačítko **Další**.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_09.png)  

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_05.png)  

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-workrite-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-workrite-test-user"></a>Vytvoření uživatele Workrite test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Workrite.

**Vytvoření uživatele s názvem Britta Simon v Workrite, proveďte následující kroky:**

1. Přihlaste se k vašemu webu společnosti workrite jako správce.

2. V navigačním podokně klikněte na **Správce**.

    ![Přiřazení uživatele][400]

3. Přejděte na odkazy a potom klikněte na **Vytvořit uživatele**. 

    ![Přiřazení uživatele][401]

4. V dialogovém okně **Vytvořit uživatele** proveďte následující kroky:

    ![Přiřazení uživatele][402]

    na. Zadejte **e-mailu**, **Křestní jméno** a **Příjmení** platné Azure AD uživatele, bude trvat.

    b. Vyberte **Správce klienta** jako **Zvolte roli**. 

    c. Klikněte na **Uložit**.   


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Workrite.

    ![Assign User][200] 

**Pokud chcete přiřadit Britta Simon Workrite, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Workrite**.

    ![Přiřazení uživatele][202] 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 
1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Workrite na panelu aplikace Access můžete by měla získat automaticky přihlášení z Workrite aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_01.png
[500]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_05.png

[6]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_02.png
[8]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_03.png
[9]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_04.png
[10]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_07.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png




