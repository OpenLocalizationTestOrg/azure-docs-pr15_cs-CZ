<properties
    pageTitle="Kurz: Azure Active Directory integrace se serverem Tableau | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tableau serveru."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Kurz: Azure Active Directory integrace se serverem Tableau

Cílem tohoto kurzu je ukazují, jak integrovat Tableau Server Azure Active Directory (Azure AD).

Integrace Tableau serveru s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Tableau serveru
- Povolení uživatelům, aby automaticky získat přihlášení na server Tableau (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace se serverem Tableau, musíte následující položky:

- Předplatné Azure AD
- Serveru Tableau jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Tableau serveru z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-tableau-server-from-the-gallery"></a>Přidání Tableau serveru z Galerie
Pokud chcete nakonfigurovat integraci Tableau serveru do Azure AD, potřebujete přidat Tableau serveru z Galerie do seznamu spravované SaaS aplikace.

**Přidání Tableau serveru z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 
 
    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Tableau serveru**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. V podokně výsledků vyberte **Tableau Server**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Tableau serveru založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Tableau serveru pro uživatele v Azure AD. Odkaz relace mezi Azure AD uživatel a související na serveru Tableau jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** na Tableau serveru.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Tableau serveru, musíte provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření serveru Tableau otestovat uživatele](#creating-a-tableauserver-test-user)** – mít protějšek Britta Simon Tableau serveru, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Tableau serveru.

Tableau serverové aplikace očekává výrazy SAML v určitém formátu. Následující obrázek ukazuje příklad pro tuto. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Nakonfigurovat Tableau Server Azure AD jednotné přihlašování, proveďte následující kroky:**


1. Na portálu na stránce integrace aplikace **Tableau serveru** v nabídce v horní části Azure klasické klepněte na položku **atributy**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. V dialogovém okně **tokenu atributy SAML** proveďte následující kroky:

    

    na. Klikněte na **Přidat uživatele atribut** otevřete dialogové okno **Přidat Attribure uživatele** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Do textového pole **Název Attrubute** zadejte **uživatelské jméno**.

    c. Ze seznamu **Hodnot atribut** selsect **user.displayname**.

    d. Klikněte na **dokončení**.  
    



1. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 



2. Na stránce **jakým způsobem uživatelé přihlásit k serveru Tableau** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    na. Do textového pole **Symbol v adrese URL** zadejte adresu URL serveru Tableau. 

    b. Do identifikátor pole Kopírovat 

    c. Klikněte na tlačítko **Další**


4. Na stránce **Konfigurace jednotného přihlašování na serveru Tableau** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


6. Získat SSO nakonfigurován pro aplikaci, musíte přihlašování ke tenantovi Tableau serveru jako správce.

    na. V konfiguraci Tableau serveru klikněte na kartu **SAML** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Zaškrtněte políčko **Použít SAML pro jednotné přihlašování**.

    c. Vyhledejte soubor federace metadat stahují z Azure klasické portál a pak ho nahrajte **soubor SAML Idp metadat**.

    d. Adresa URL pro návrat tableau Server – zadaná adresa URL serveru Tableau uživatelé budou, například http://tableau_server. Použití http://localhost se nedoporučuje. Není podporované použití adresy URL koncových lomítko (například http://tableau_server/). Kopírování **Tableau Server vrátit adresy URL** a vložte ho do textového pole Azure AD **Znaménko na adresu URL** , jak je vidět v kroku 3

    e. SAML entity ID – entity ID jednoznačně identifikuje instalaci Tableau serveru pro IdP. Můžete zadat svoji adresu URL serveru Tableau znovu tady, pokud chcete, ale nemá být svoji adresu URL serveru Tableau. **SAML entity ID** zkopírovat a vložit ho do textového pole Azure AD **IDENTIFIKÁTOREM** uvedeno v kroku 3.

    f. Klikněte na **Export souboru metadat** a potom ho otevřete v textový editor aplikace. Vyhledání výrazu adresy URL služby příjemce se Http publikovat a Index 0 a zkopírujte adresu URL. Vložte ho do textového pole Azure AD **Odpovědět URL** uvedeno v kroku 3. 

    g. Na stránce Tableau serveru Configiuration na tlačítko **OK** .

    > [AZURE.NOTE] Pokud potřebujete pomoc konfiguraci SAML na serveru Tableau potom naleznete v tomto článku [Konfigurace SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

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

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    na. Jako **Typ uživatele**vyberte **nového uživatele ve vaší organizaci**.

    b. Do textového pole **Uživatelské jméno** zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-tableau-server-test-user"></a>Vytvoření uživatele test Tableau serveru

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon na Tableau serveru. Potřebujete zřízení všichni uživatelé na serveru Tableau. Nezapomeňte, že uživatelské jméno uživatele, by měly odpovídat hodnoty, které jste nakonfigurovali v Azure AD vlastní atributu **uživatelské jméno**. Mapování správné měli integrace spolupracovat [Konfigurace Azure AD jednotného přihlašování](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat správce serveru Tableau ve vaší organizaci.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup Tableau server.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Tableau serveru, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
 
    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Tableau serveru**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Tableau serveru na panelu aplikace Access můžete by měla získat automaticky přihlášení na serveru Tableau aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
