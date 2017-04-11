<properties
    pageTitle="Kurz: Azure Active Directory integrace s Keylight | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Keylight."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Kurz: Azure Active Directory integrace s Keylight

V tomto kurzu se naučíte, jak integrovat Keylight s Azure Active Directory (Azure AD).

Integrace Keylight s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Keylight
- Povolení uživatelům, aby automaticky získat přihlášení na k Keylight (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Keylight, musíte následující položky:

- Předplatné Azure
- Keylight jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Keylight z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-keylight-from-the-gallery"></a>Přidání Keylight z Galerie
Pokud chcete nakonfigurovat integraci Keylight do Azure AD, potřebujete přidat Keylight z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Keylight z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Keylight**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. V podokně výsledků vyberte **Keylight**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Keylight založeného na uživateli testu s názvem "Britta Simon".

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Keylight, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Keylight testování uživatele](#creating-a-keylight-test-user)** – a protějšek Britta Simon Keylight, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Keylight.


**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Keylight, proveďte následující kroky:**

1. Na portálu na stránce **Keylight** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 


2. Na stránce **jakým způsobem uživatelé přihlásit k Keylight** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Keylight pomocí následujícího vzorce: **"https://\<název společnosti\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Na stránce **konfigurovat jednotné přihlašování v Keylight** proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Povolit jednotné přihlašování v Keylight, proveďte následující kroky:
 
    na. Přihlašování ke svému účtu Keylight jako správce.

    b. V nabídce na horní klikněte na **osobu**a vyberte **Keylight nastavení**.
       
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/401.png) 

    c. V ovládacím prvku treeview na levé straně klikněte na **SAML**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. V dialogovém okně **Nastavení SAML** klikněte na **Upravit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Na stránce dialogové okno **Upravit nastavení SAML** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/405.png) 

    na. Nastavení **ověřování SAML** na **aktivní**.


    b. Klasický portálu Azure AD zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .

    c. Azure AD klasické portálu zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Adresa URL odhlášení poskytovatele Identity** .

    d. Klikněte na **Zvolit soubor** a vyberte stažený Keylight certifikát a klikněte na tlačítko **Otevřít** nahrát certifikát.


    e. Nastavit **umístění Id uživatele SAML** **NameIdentifier element příkazu předmět**.
   
    f. Poskytnutí **Keylight poskytovatele pomocí následujícího vzorce: **https://&lt;název společnosti&gt;. keylightgrc.com**.

    g. Nastavení **automatického zřizování uživatelů** na **aktivní**.

    h. Nastavit **Typ účtu automaticky zajištění** **Úplné**uživateli.

    Můžu. Jako **role zabezpečení automatického poskytování**klikněte na **Standardní uživatele SAML**.
   
    j. Jako **poskytování automatické konfigurace zabezpečení**vyberte **Standardní konfigurace uživatele**.
   
    k. Do textového pole e-mailu atribut zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Do textového pole **atribut křestní jméno** zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. **Poslední atribut název** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Klikněte na **Uložit**.
   
  
   
  
6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu Azure klasické s názvem Britta Simon.

V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]



**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-keylight-test-user"></a>Vytvoření uživatele Keylight test

V této části vytvoříte uživatele s názvem Britta Simon v Keylight. Keylight podporuje za běhu zřizování, která je standardně zapnutá.

Neexistuje žádná akce položku, kterou v této části. Vytvoří nový uživatel se vytvoří při otevření Keylight, pokud uživatel dosud neexistuje. 

> [AZURE.NOTE] Pokud potřebujete k vytvoření uživatele ručně, budete muset kontaktovat tým podpory Keylight.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Keylight.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Keylight, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Keylight**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Keylight na panelu aplikace Access můžete by měla získat automaticky přihlášení z Keylight aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
