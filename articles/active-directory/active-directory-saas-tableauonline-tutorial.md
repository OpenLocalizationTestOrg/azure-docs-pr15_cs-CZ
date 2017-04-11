<properties
    pageTitle="Kurz: Azure Active Directory integrace s Tableau Online | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tableau Online."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Kurz: Azure Active Directory integrace s Tableau Online

V tomto kurzu se naučíte, jak integrovat Tableau Online pomocí služby Azure Active Directory (Azure AD).

Integrace s Azure AD Tableau Online poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Tableau Online
- Povolení uživatelům, aby automaticky získat přihlášení z Tableau online (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Tableau Online, musíte následující položky:

- Předplatné Azure AD
- **Tableau Online** jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Tableau Online z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-tableau-online-from-the-gallery"></a>Přidání Tableau Online z Galerie
Pokud chcete nakonfigurovat integraci Tableau Online do Azure AD, budete muset Tableau Online z galerie přidat do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Tableau Online z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Tableau Online**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. V podokně výsledků vyberte **Tableau Online**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Tableau Online založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Tableau Online je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v režimu Online Tableau jinými slovy, je nutné stanovit.
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Tableau Online.

Testování a konfigurace Azure AD jednotné přihlašování s Tableau Online, musíte provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření Tableau Online testování uživatele](#creating-a-Tableau-Online-test-user)** – a protějšek Britta Simon Tableau Online, který je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Tableau Online.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Tableau Online, proveďte následující kroky:**

1. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování][6]
2. Na portálu klasické na stránce integrace **Tableau Online** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][7] 

3. Na stránce **jakým způsobem uživatelé přihlásit k Tableau Online** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    na. Přihlaste se na adresu URL do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://sso.online.tableau.com`

    c. Klikněte na tlačítko **Další**.

5. Na stránce **konfigurovat jednotné přihlašování v Tableau Online** klikněte na **Stáhnout metadata**a pak uložit soubor v počítači.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]
8. V jiném okně prohlížeče přihlašování do aplikace Tableau Online. Přejděte na **Nastavení** a potom **ověřování**

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. V části **Typy ověřování** . Zaškrtněte políčko **jednotné přihlašování s SAML** povolit SAML.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Přejděte dolů do části **Importovat soubor metadat do Tableau Online** .  Klikněte na tlačítko Procházet a soubor metadat, který jste stáhli z Azure AD importovat. Potom klikněte na **použít**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. V části **POZVYHLEDAT výrazy** vložte odpovídající zprostředkovatele identit výraz název e-mailovou adresu, křestní jméno a příjmení. Chcete-li získat tyto informace z Azure AD:

    na. Přejděte zpět do Azure AD. Na portálu na stránce **Tableau Online** integrace aplikace v nabídce v horní části Azure klasické klepněte na položku **atributy**. Název hodnoty zkopírovat: userprincipalname, givenname a příjmení.
     
    ![Azure AD jednotné přihlášení](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Přepnutí do Tableau Online aplikací a pak nastavte v části **Online atributy Tableau** takto:
    
    -  E-mailu: **Pošta** nebo **userprincipalname**
    -  Křestní jméno: **givenName (jméno)**
    -  Příjmení: **Surname (příjmení)**

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-tableau-online-test-user"></a>Vytvoření Tableau Online zkušební uživatele

V této části vytvoříte uživatele s názvem Britta Simon Tableau online.

1. Na **Tableau Online**klikněte na **Nastavení** a potom **ověřování** oddíl. Přejděte dolů k oddílu **Vyberte uživatele** . Klikněte na **Přidat uživatele** a potom **Zadejte e-mailové adresy**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Vyberte **Přidat uživatele standardu jednoho přihlašování (SSO)**. Do textového pole **Zadejte e-mailové adresy** přidátebritta.simon@contoso.com

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klikněte na **vytvořit**.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Tableau Online.

![Přiřazení uživatele][200] 

**Chcete-li přiřadit Britta Simon Tableau Online, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

3. V seznamu aplikací vyberte **Tableau Online**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

5. V seznamu všichni uživatelé vyberte **Britta Simon**.

6. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Tableau Online na panelu aplikace Access můžete by měla získat automaticky přihlášení z Tableau Online aplikaci.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
