<properties
    pageTitle="Kurz: Azure Active Directory integrace s hostované grafitová | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a hostované grafitová."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hosted-graphite"></a>Kurz: Azure Active Directory integrace s hostované grafitová

Cílem tohoto kurzu je předvedení integrace hostované grafitová s Azure Active Directory (Azure AD).

Integrace hostované grafitová s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k hostované grafitová
- Povolení uživatelům, aby automaticky získat přihlášení z k hostované grafitová (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s hostované grafitová, musíte následující položky:

- Předplatné Azure AD
- Hostované grafitová jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání hostované grafitová z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-hosted-graphite-from-the-gallery"></a>Přidání hostované grafitová z Galerie
Abyste mohli nakonfigurovat integraci hostované grafitová do Azure AD, potřebujete přidat hostované grafitová z Galerie do seznamu spravované SaaS aplikace.

**Přidat hostované grafitová z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Hostované grafitová**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_01.png)

7. V panelu Výsledky vyberte **Hostované grafitová**a klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí hostované grafitová založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému hostované grafitová uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v hostované grafitová jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v hostované grafitová.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s hostované grafitová, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření grafitová hostované otestujte uživatele](#creating-a-hosted-graphite-test-user)** – a protějšek Britta Simon hostované grafitová, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci hostované grafitová.

**Nakonfigurovat hostované grafitová Azure AD jednotné přihlašování, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Hostované grafitová** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k hostované grafitová** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno Pokud chcete nakonfigurovat aplikaci **IDP, které iniciuje režimu**, proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_04.png)

    na. **Identifikátor URI** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/metadata/<user id>`

    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/complete/saml/<user id>`

    c. Klikněte na tlačítko **Další**

4. Pokud chcete nakonfigurovat aplikaci **SP, které iniciuje režimu** na stránce **Konfigurovat nastavení aplikace** dialogové okno, klikněte na příkaz **"Zobrazit rozšířená nastavení (volitelné)"** zadejte **Znaménko na adresu URL** a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_10.png)

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://www.hostedgraphite.com/login/saml/<user id>/`

    b. Klikněte na tlačítko **Další**

    > [AZURE.NOTE] Upozorňujeme, že nejsou skutečnou hodnotou. Budete muset aktualizovat tyto hodnoty skutečné znaménko na adresu URL, identifikátor a adresu URL odpověď. Aby tyto hodnoty, můžete přejít na přístup -> Nastavení SAML na straně aplikace nebo požádejte hostované grafitová.

5. Na stránce **konfigurovat jednotné přihlašování v hostované grafitová** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.

6. Přihlášení k vašemu klientovi hostované grafitová jako správce.

7. Přejděte na **stránku nastavení SAML** na bočním panelu (**přístup -> SAML nastavení**).

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_000.png)

8. Potvrďte, že tyto adresy URL neodpovídá konfiguraci v kroku 3.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_001.png)

9. Zkopírujte **adresu URL Vystavitel** a **adresu URL SAML SSO** z Azure AD **Entity nebo ID Vystavitel** a **Přihlašovací adresa URL jednotné přihlašování** v hostovanou grafitová.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_002.png)

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_003.png)

9. Vyberte "**jen pro čtení**" jako **výchozí Role uživatele**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_004.png)

10. Zkopírujte obsah souboru stažený certifikát a potom je vložte do textového pole **X.509 certifikát** .

     ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_005.png)

11. Klikněte na tlačítko **Uložit** .

12. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

13. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem v této části je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hostedgraphite-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-hosted-graphite-test-user"></a>Vytvoření uživatele hostovaná grafitová test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v hostované grafitová. Hostovanou grafitová podporuje za běhu zřizování, což je standardně povolený.

Neexistuje žádná akce položku, kterou v této části. Při pokusu o přístup k hostované grafitová, pokud dosud neexistuje se vytvoří nový uživatel.

> [AZURE.NOTE] Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory hostované grafitová prostřednictvím <mailto:help@hostedgraphite.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup hostované grafitová.
    
![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon hostované grafitová, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **Hostované grafitová**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hostedgraphite-tutorial/tutorial_hostedgraphite_50.png)

1. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici hostované grafitová na panelu aplikace Access můžete by měla získat automaticky přihlášení z hostované grafitová aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hostedgraphite-tutorial/tutorial_general_205.png
