<properties
    pageTitle="Kurz: Azure Active Directory integrace s PerformanceCentre | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PerformanceCentre."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Kurz: Azure Active Directory integrace s PerformanceCentre

Cílem tohoto kurzu je vidíte, jak integrovat PerformanceCentre s Azure Active Directory (Azure AD).  
Integrace PerformanceCentre s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k PerformanceCentre 
- Povolení uživatelům, aby automaticky získat přihlášení z k PerformanceCentre (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure Active Directory

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí PerformanceCentre, musíte následující položky:

- Předplatné Azure AD
- PerformanceCentre jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání PerformanceCentre z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-performancecentre-from-the-gallery"></a>Přidání PerformanceCentre z Galerie
Pokud chcete nakonfigurovat integraci PerformanceCentre do Azure AD, potřebujete přidat PerformanceCentre z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat PerformanceCentre z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **PerformanceCentre**.
 
    ![Aplikace][5]

7. V podokně výsledků vyberte **PerformanceCentre**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí PerformanceCentre založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému PerformanceCentre uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v PerformanceCentre jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v PerformanceCentre.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s PerformanceCentre, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření PerformanceCentre testování uživatele](#creating-a-halogen-software-test-user)** – a protějšek Britta Simon PerformanceCentre, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu klasické Azure AD a konfigurace jednotného přihlašování v aplikaci PerformanceCentre.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s PerformanceCentre, proveďte následující kroky:**

1. V klasickém portálu Azure AD na stránce **PerformanceCentre** aplikace integrace klikněte **Konfigurace jednotného přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k PerformanceCentre** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][7] 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Azure AD jednotné přihlášení][8] 
 
     na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané uživatelů k přihlašování k vašemu webu PerformanceCentre (například: *http://companyname.performancecentre.com/saml/SSO*).
 
     b. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v PerformanceCentre** proveďte následující kroky:

    ![Azure AD jednotné přihlášení][9] 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.



1. Přihlášení k vašemu webu společnosti **PerformanceCentre** jako správce.

2. Na karet na levé straně klikněte na **Konfigurovat**.

    ![Azure AD jednotné přihlášení][10]

2. Na karet na levé straně klikněte na **různé**a potom klikněte na **Jednotné přihlašování**.

    ![Azure AD jednotné přihlášení][11]

2. **Protocol (protokol)**vyberte **SAML**.

    ![Azure AD jednotné přihlášení][12]

2. Otevřete soubor stažený metadata v poznámkovém bloku, zkopírujte obsah, vložte ho do textového pole **Metadat zprostředkovatele identit** a klikněte na tlačítko **Uložit**.

    ![Azure AD jednotné přihlášení][13]

2. Ověřte správnost hodnoty pro **Základní adresa URL entita** a **Adresu URL ID Entity** .

    ![Azure AD jednotné přihlášení][14]


6. Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Azure AD jednotné přihlášení][15]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][16]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_09.png)  

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_05.png)  

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-performancecentre-test-user"></a>Vytvoření uživatele PerformanceCentre test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v PerformanceCentre.

**Vytvoření uživatele s názvem Britta Simon v PerformanceCentre, proveďte následující kroky:**

1. Přihlaste se k vašemu webu společnosti PerformanceCentre jako správce.

2. V nabídce na levé straně klikněte na **Interrelate**a potom klikněte na **Vytvořit účastníka**.

    ![Vytvoření uživatele][400]

4. V dialogovém okně **Interrelate – vytvoření účastník** proveďte následující kroky:

    ![Vytvoření uživatele][401]

    na. Zadejte požadované atributy pro Britta Simon do související textová pole.
    
    > [AZURE.IMPORTANT] Atribut Britta je uživatelské jméno v PerformanceCentre musí být stejný jako uživatelské jméno v Azure AD.


    b. Vyberte **Správce klienta** jako **Zvolte roli**. 

    c. Klikněte na **Uložit**.   


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k PerformanceCentre.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon PerformanceCentre, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **PerformanceCentre**.

    ![Přiřazení uživatele][202]

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici PerformanceCentre na panelu aplikace Access můžete by měla získat automaticky přihlášení z PerformanceCentre aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_01.png
[500]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_02.png

[6]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_03.png
[8]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_04.png
[9]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_05.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png
[15]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_06.png
[16]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_50.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png
[402]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_402.png



