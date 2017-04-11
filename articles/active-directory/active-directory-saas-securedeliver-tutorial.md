<properties
    pageTitle="Kurz: Azure Active Directory integrace s Novatus | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ZABEZPEČENÉ PŘEDVÁDĚNÍ."
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


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Kurz: Azure Active Directory integrace s ZABEZPEČENÉ PŘEDVÁDĚNÍ

Cílem tohoto kurzu je vidíte, jak integrovat do ZABEZPEČENÉ PŘEDVÁDĚNÍ Azure Active Directory (Azure AD).  
Integrace ZABEZPEČENÉ DORUČOVÁNÍ s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo bude mít přístup k ZABEZPEČENÉ PŘEDVÁDĚNÍ
- Povolení uživatelům, aby automaticky získat přihlášení na SECURE předvádění (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ZABEZPEČENÉ poskytovat, musíte následující položky:

- Předplatné Azure
- SECURE PŘEDVÁDĚNÍ jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ZABEZPEČENÉ PŘEDVÁDĚNÍ z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-secure-deliver-from-the-gallery"></a>Přidání ZABEZPEČENÉ PŘEDVÁDĚNÍ z Galerie
Abyste mohli nakonfigurovat integraci ZABEZPEČENÉ DORUČOVAT do Azure AD, budete muset ZABEZPEČENÉ PŘEDVÁDĚNÍ z galerie přidat do seznamu spravované SaaS aplikace.

**Přidat ZABEZPEČENÉ DORUČOVÁNÍ z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **ZABEZPEČENÉ doručení**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. V podokně výsledků vyberte **ZABEZPEČENÉ PŘEDVÁDĚNÍ**a potom klikněte na **Dokončit** přidat aplikaci.
    
    ![Logo aplikace a název v galerii](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem Tato část je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí ZABEZPEČENÉ poskytovat založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému ZABEZPEČENÉ poskytovat uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v ZABEZPEČENÉ PŘEDVÁDĚNÍ jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ZABEZPEČENÉ PŘEDVÁDĚNÍ.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s ZABEZPEČENÉ poskytovat, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření ZABEZPEČENÉ PŘEDVÁDĚNÍ testování uživatele](#creating-a-secure-deliver-test-user)** – mít protějšek Britta Simon v části lidé, které otevře Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci ZABEZPEČENÉ PŘEDVÁDĚNÍ.



**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s ZABEZPEČENÉ poskytovat, proveďte následující kroky:**

1. Na portálu na **SECURE PŘEDVÁDĚNÍ** aplikace integrace stránce Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k ZABEZPEČENÉ PŘEDVÁDĚNÍ** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a potom klikněte na **Další**:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace ZABEZPEČENÉ poskytovat pomocí následujícího vzorce: **"https://i-securedeliver.jp/sd/\<název společnosti\>/jsf/login/sso"**.

    b. Obraťte se SECURE PŘEDVÁDĚNÍ tým podpory prostřednictvím [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) získat adresu URL klienta Pokud neznáte hodnotu.

    c. **Identifikátor URI** do textového pole zadejte adresu URL klienta. 

    d. Klikněte na tlačítko **Další**


4. Na stránce **konfigurovat jednotné přihlašování v ZABEZPEČENÉ PŘEDVÁDĚNÍ** proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Získat SSO nakonfigurován pro aplikaci, kontaktujte tým podpory ZABEZPEČENÉ PŘEDVÁDĚNÍ prostřednictvím [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) a dejte jí následující:
    
    • Soubor stažený certifikátu

    • **ID Entity.**

    • Adresu **URL služby jednotného přihlašování**

    • **Adresy URL jednoho odhlašovací služby**



6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření zkušební uživatele ZABEZPEČENÉ PŘEDVÁDĚNÍ v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
  
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-secure-deliver-test-user"></a>Vytvoření uživatele ZABEZPEČENÉ PŘEDVÁDĚNÍ test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v ZABEZPEČENÉ PŘEDVÁDĚNÍ. Prosím pracujte se na tým podpory ZABEZPEČENÉ PŘEDVÁDĚNÍ přidáte uživatele v okně ZABEZPEČENÉ PŘEDVÁDĚNÍ účet.

> [AZURE.NOTE] Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory ZABEZPEČENÉ PŘEDVÁDĚNÍ.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup k ZABEZPEČENÉ PŘEDVÁDĚNÍ.

![Přiřazení uživatele][200] 

**Přiřadit Britta Simon ZABEZPEČENÉ poskytovat, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **ZABEZPEČENÉ PŘEDVÁDĚNÍ**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici ZABEZPEČENÉ PŘEDVÁDĚNÍ na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci ZABEZPEČENÉ PŘEDVÁDĚNÍ.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
