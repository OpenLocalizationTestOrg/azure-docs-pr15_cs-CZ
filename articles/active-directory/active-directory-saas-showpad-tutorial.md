<properties
    pageTitle="Kurz: Azure Active Directory integrace s Showpad | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Showpad."
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


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Kurz: Azure Active Directory integrace s Showpad

Cílem tohoto kurzu je vidíte, jak integrovat Showpad s Azure Active Directory (Azure AD).

Integrace Showpad s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Showpad
- Povolení uživatelům, aby automaticky získat přihlášení z k Showpad (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – portálu Azure Active Directory

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s Showpad, musíte následující položky:

- Předplatné Azure AD
- Předplatné Showpad


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Showpad z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-showpad-from-the-gallery"></a>Přidání Showpad z Galerie
Pokud chcete nakonfigurovat integraci Showpad do Azure AD, potřebujete přidat Showpad z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Showpad z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Aplikace][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.
 
    ![Aplikace][4]

6. Do pole Hledat zadejte **Showpad**.

    ![Aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. V podokně výsledků vyberte **Showpad**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Showpad založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD ví, co je databáze odpovídajících funkcí pro uživatele systému Showpad uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Showpad jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Showpad.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Showpad, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Showpad otestujte uživatele](#creating-a-showpad-test-user)** – a protějšek Britta Simon Showpad, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Showpad.



**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Showpad, proveďte následující kroky:**

1. Na portálu na stránce **Showpad** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Showpad** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Showpad pomocí následujícího vzorce:`https://<company name>.showpad.biz/login`

    b. **Identifikátor URI** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.showpad.biz`

    c. Klikněte na tlačítko **Další**


4. Na stránce **konfigurovat jednotné přihlašování v Showpad** proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5. Přihlášení k vašemu tenantovi Showpad jako správce.

6. V nabídce na horní klikněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Přejděte na "**Jednotné přihlašování**" a klikněte na možnost "**Povolit**".
 
    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. V dialogovém okně **Přidat službu SAML 2.0** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    na. Do textového pole **název** zadejte název poskytovatele identifikátor (například: název vaší společnosti).

    b. Jako **Zdroj metadat**vyberte **XML**.

    c. Zkopírujte obsah souboru XML stažený metadat a potom je vložte do textového pole **Metadata XML** .

    d. Vyberte **Automatické poskytování účty pro nové uživatele při přihlášení**.

    e. Klikněte na **Odeslat**.


10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]


11. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]






### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření Showpad testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    na. Do textového pole **Uživatelské jméno** zadejte **BrittaSimon**.

    b. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. **Jako **Role**vyberte ho.**

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.


### <a name="creating-a-showpad-test-user"></a>Vytvoření uživatele Showpad test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Showpad. 

Showpad podporuje za běhu zřizování. Povolili jste zřizování v **[Konfigurace Azure AD jednotného přihlašování](#configuring-azure-ad-single-single-sign-on)**. 

Neexistuje žádná akce položku, kterou v této části. 




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Showpad.

![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon Showpad, proveďte následující kroky:**

1. Na portálu v nabídce v horní části Azure klasické klikněte na **aplikace**.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací klepněte na **Showpad**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]




### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici **Showpad** na panelu aplikace Access můžete by měla získat automaticky přihlášení z Showpad aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
