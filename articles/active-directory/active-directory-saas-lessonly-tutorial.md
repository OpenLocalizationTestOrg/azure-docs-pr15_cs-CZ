<properties
    pageTitle="Kurz: Azure Active Directory integrace s Lessonly | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lessonly."
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


# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a>Kurz: Azure Active Directory integrace s Lessonly

Cílem tohoto kurzu je vidíte, jak integrovat Lessonly s Azure Active Directory (Azure AD).

Integrace Lessonly s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Lessonly
- Povolení uživatelům, aby automaticky získat přihlášení na k Lessonly (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Lessonly, musíte následující položky:

- Předplatné Azure AD
- Lessonly jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Lessonly z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-lessonly-from-the-gallery"></a>Přidání Lessonly z Galerie
Pokud chcete nakonfigurovat integraci Lessonly do Azure AD, potřebujete přidat Lessonly z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Lessonly z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.
 
    ![Aplikace][4]

6. Do pole Hledat zadejte **Lessonly**.
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_01.png)

7. V podokně výsledků vyberte **Lessonly**a potom klikněte na **Dokončit** přidat aplikaci.


![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Lessonly založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Lessonly uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Lessonly jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Lessonly.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Lessonly, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Lessonly testování uživatele](#creating-a-Lessonly-test-user)** – a protějšek Britta Simon Lessonly, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Lessonly s účtem v Azure AD pomocí federace podle protokolu SAML.

Aplikace Lessonly očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .
Následující obrázek ukazuje příklad pro tuto.

![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png) 

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Lessonly, proveďte následující kroky:**

1. Na portálu na stránce integrace Lessonly aplikace klikněte v nabídce v horní části Azure klasické klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_07.png) 

2. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_08.png) 

    na. Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.

    b. Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.

    c. **Hodnota atributu** seznamu vyberte hodnotu atributu zobrazí pro tento řádek.

    d. Klikněte na **dokončení**

3. Klikněte na **použít změny**.

4. V prohlížeči klikněte na **zpět** a znovu otevřete dialogové okno Snadné spuštění

5. Klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

6. Na stránce **jakým způsobem uživatelé přihlásit k Lessonly** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_03.png) 

7. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_04.png) 


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Lessonly pomocí následujícího vzorce: **"https://companyname.lesson.ly/signin"**. Při odkazování na obecný název této **NázevSpolečnosti** musí nahrazuje skutečný název.


8. Na stránce **konfigurovat jednotné přihlašování v Lessonly** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


9. Získat SSO nakonfigurován pro aplikaci, kontaktujte tým podpory Lessonly prostřednictvím dev@lessonly.com. Připojit soubor stažený certifikát pošty a nasdílet Lessonly týmu k nastavení jednotné přihlašování ve své straně adresy URL metadata (Entity ID, jednotné přihlašování přihlašovací adresu URL a přihlaste se na adresy URL).


10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

11. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lessonly-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-lessonly-test-user"></a>Vytvoření uživatele Lessonly test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Lessonly. Lessonly podporuje za běhu zřizování, což je standardně povolený.

Neexistuje žádná akce položku, kterou v této části. Při pokusu o přístup k Lessonly, pokud ho ještě neexistuje se vytvoří nový uživatel. [Konfigurace Azure AD jednotné přihlášení](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory Lessonly.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Lessonly.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Lessonly, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Lessonly**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Lessonly na panelu aplikace Access můžete by měla získat automaticky přihlášení z Lessonly aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_205.png
