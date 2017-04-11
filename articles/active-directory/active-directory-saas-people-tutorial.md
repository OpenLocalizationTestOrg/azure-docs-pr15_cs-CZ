<properties
    pageTitle="Kurz: Azure Active Directory integrace s lidmi | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a lidé."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-people"></a>Kurz: Azure Active Directory integrace s lidmi

Cílem tohoto kurzu je předvedení integrace lidé s Azure Active Directory (Azure AD).

Integrace lidé s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo bude mít přístup uživatelům
- Povolení uživatelům, aby automaticky získat přihlášení z lidem (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s lidmi, musíte následující položky:

- Předplatné Azure
- Lidé jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání lidí z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-people-from-the-gallery"></a>Přidání lidí z Galerie
Abyste mohli nakonfigurovat integraci lidi do Azure AD, budete muset přidání lidí do seznamu spravované SaaS aplikace z galerie.

**Jak přidat lidi z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 
 
    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **lidé**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. V podokně výsledků vyberte **Uživatelé**a klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s lidmi založeného na uživateli testu s názvem "Britta Simon".

Testování a konfigurace Azure AD jednotné přihlašování s lidmi, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření lidé testování uživatele](#creating-a-people-test-user)** – mít protějšek Britta Simon v části lidé, které otevře Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci lidé.



**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s lidmi, proveďte následující kroky:**

1. Na portálu na stránce **lidé** v aplikaci integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    [Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k lidé** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a potom klikněte na **Další**:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace lidé pomocí následujícího vzorce: **"https://\<název společnosti\>.peoplehr.com/"**. 

    b. Pokud si nejste jisti svoji adresu URL klienta, obraťte se na tým podpory lidé prostřednictvím [customerservices@peoplehr.com](mailto:customerservices@peoplehr.com) ho.  

    c. **Identifikátor URI** do textového pole zadejte adresu URL klienta. 

    d. V textovém poli **Odpovědět URL** zadejte adresu URL do následujícího vzorce: "**https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**".

    e. Klikněte na tlačítko **Další**


4. Na stránce **Konfigurace jednotného přihlašování lidí** proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_05.png) 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5. Získat SSO nakonfigurován pro aplikaci, musíte přihlašování ke tenantovi lidé jako správce.
    
    na. V nabídce na levé straně klikněte na **Nastavení**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_001.png) 

    b. Klikněte na **"Společnosti"**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_002.png) 

    c. Na **"nahrát" jednotného přihlášení na "SAML meta datového souboru"**, klikněte na tlačítko **Procházet** k nahrání souboru stažený metadat.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
 
    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.
V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]


**V Azure AD vytvořit testovací uživatelské jméno osoby, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-people-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-people-test-user"></a>Vytvoření uživatele test lidé

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v oblasti lidé zobrazuje. Lidé nepodporuje za běhu zřizování tak potřebujete kontaktovat tým podpory lidé ručně vytvořit uživatele.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup uživatelům.

![Přiřazení uživatele][200] 

**Přiřazení Britta Simon lidem, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **lidé**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-people-tutorial/tutorial_people_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
Po kliknutí na dlaždici lidé na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci lidé.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png
