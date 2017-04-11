<properties
    pageTitle="Kurz: Azure Active Directory integrace s HR2day tak, že Merces | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HR2day tak, že Merces."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Kurz: Azure Active Directory integrace s HR2day tak, že Merces

Cílem tohoto kurzu je vidíte, jak integrovat HR2day tak, že Merces s Azure Active Directory (Azure AD).  
Integrace HR2day tak, že Merces s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k HR2day tak, že Merces
- Povolení uživatelům, aby automaticky získat přihlášení z k HR2day tak, že Merces (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s HR2day tak, že Merces, musíte následující položky:

- Předplatné Azure AD
- HR2day tak, že Merces jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání HR2day tak, že Merces z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>Přidání HR2day tak, že Merces z Galerie
Pokud chcete nakonfigurovat integraci HR2day tak, že Merces do Azure AD, potřebujete přidat HR2day tak, že Merces z Galerie do seznamu spravované SaaS aplikace.

**Přidání HR2day tak, že Merces z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
 
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **HR2day tak, že Merces**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. V podokně výsledků vyberte **HR2day tak, že Merces**a potom klikněte na **Dokončit** přidat aplikaci.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s HR2day tak, že Merces založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému HR2day tak, že Merces uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v HR2day tak, že Merces jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v HR2day tak, že Merces.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s HR2day tak, že Merces, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření HR2day tak, že Merces testování uživatele](#creating-a-hr2day-by-merces-test-user)** – a protějšek Britta Simon HR2day tak, že Merces, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit HR2day tak, že Merces s účtem v Azure AD pomocí federace podle protokolu SAML.

HR2day aplikací Merces očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace SAML tokenu atributy. Následující obrázek ukazuje příklad pro tuto. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Před výraz SAML můžete nakonfigurovat, budete muset kontaktovat tým podpory HR2day prostřednictvím [servicedesk@merces.nl](mailto:servicedesk@merces.nl) a požádat o přínosu atribut jedinečný identifikátor pro vašeho klienta. Budete muset tuto hodnotu na postupujte podle pokynů v následující části.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s HR2day tak, že Merces, proveďte následující kroky:**

1. Na portálu na stránce integrace aplikace **HR2day tak, že Merces** v nabídce v horní části Azure klasické klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** . 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky, proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    na. Klikněte na **Přidat uživatele atribut**.

    b. **Název atributu** do textového pole zadejte **"ATTR_LOGINCLAIM"**.

    c. **Hodnota atributu** seznamu vyberte **Join()**. 

    d. V seznamu **řetězec_1** vyberte **User.mail**. 

    e. Do textového pole **řetězec_2** zadejte **Jedinečný identifikátor** poskytnutých týmem HR2day. 

    f. **Oddělovač** do textového pole, zadejte **@**.

    g. Klikněte na **dokončení**.

  
3. Klikněte na **použít změny**.


1. V nabídce na horní klikněte na tlačítko **Rychlý Start** otevřete dialogové okno **Rychlý Start** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k HR2day tak, že Merces** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL používané uživatelů k přihlašování k vaší HR2day aplikací Merces pomocí následujícího vzorce: **"https://\<název klienta\>.force.com/\<název instance\>"**.

    b. Klikněte na tlačítko **Další**.

4. Na stránce **konfigurovat jednotné přihlašování v HR2day tak, že Merces** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci získáte kontaktování vašeho HR2day tým podpory Merces prostřednictvím [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) soubor stažený certifikát a připojíte k e-mailu. Také zadejte URL SAML jednotné přihlašování, Sign Out adresy URL a adresu URL Vystavitel tak, aby je možné konfigurovat integrace jednotného přihlašování k.


> [AZURE.NOTE] Zkontrolujte zmínit Merces týmu, že tato integrace potřebovat ID Entity nastavovaná s Tento vzorek **https://hr2day.force.com/INSTANCENAME**



6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>Vytváření HR2day Merces test uživatelem

Cílem tento oddíl je vytvoření uživatele volat Britta Simon v HR2day Merces. Zkontrolujte s nastavenými HR2day tým podpory Merces přidáte uživatele v okně HR2day účet. 


> [AZURE.NOTE]Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat HR2day tak, že tým podpory Merces.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup HR2day tak, že Merces.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon HR2day tak, že Merces, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **HR2day tak, že Merces**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí HR2day tak, že Merces dlaždice na panelu aplikace Access můžete by měla získat automaticky přihlášení z k vaší HR2day Merces aplikací.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
