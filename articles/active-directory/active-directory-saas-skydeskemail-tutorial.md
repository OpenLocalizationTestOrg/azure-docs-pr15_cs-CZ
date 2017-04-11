<properties
    pageTitle="Kurz: Azure Active Directory integrace s e-mailem Skydesk | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Skydesk e-mailu."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Kurz: Azure Active Directory integrace s Skydesk e-mailem

Cílem tohoto kurzu je ukazují, jak integrovat Skydesk e-mailovou službou Azure Active Directory (Azure AD).

Integrace e-mailu Skydesk s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k e-mailu Skydesk
- Povolení uživatelům, aby automaticky získat přihlášení z k e-mailu Skydesk (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure Active Directory

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s Skydesk e-mailem, musíte následující položky:

- Předplatné Azure AD
- E-mailu Skydesk jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání e-mailu Skydesk z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-skydesk-email-from-the-gallery"></a>Přidání e-mailu Skydesk z Galerie
Pokud chcete nakonfigurovat integraci Skydesk e-mailu do Azure AD, potřebujete přidat Skydesk e-mailu z Galerie do seznamu spravované SaaS aplikace.

**Přidání e-mailu Skydesk z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do vyhledávacího pole zadejte **E-mailu Skydesk**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. V podokně výsledků vyberte **Skydesk e-mailu**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s Skydesk e-mailu založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je protějšek uživatelům v e-mailu Skydesk uživatele v Azure AD. Jinými slovy odkaz relace mezi Azure AD uživatel a související Skydesk e-mailem je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnotu **uživatelské_jméno** Skydesk e-mailem.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Skydesk e-mailem, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření e-mailu Skydesk testování uživatele](#creating-a-Skydesk-Email-test-user)** – mít protějšek Britta Simon v Skydesk E‑mailu, který je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cíl v této části je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Skydesk e-mailu.



**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Skydesk e-mailem, proveďte následující kroky:**

1. Na portálu na stránce **Skydesk e-mailové** aplikaci integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k e-mailu Skydesk** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Skydesk e-mailu pomocí následujícího vzorce: **"https://mail.skydesk.jp/portal/\<název společnosti\>"**.

    b. Klikněte na tlačítko **Další**.


4. Na stránce **konfigurovat jednotné přihlašování v e-mailu Skydesk** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Povolit jednotné přihlašování v **E-mailu Skydesk**, proveďte následující kroky:
 
    na. Přihlašování Skydesk e-mailového účtu jako správce.

    b. V nabídce na horní klikněte na nastavení a vyberte Org. 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c. V levém podokně klikněte na domény.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klepněte na Přidat doménu.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Zadejte název domény a ověření domény.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klikněte na **Ověření SAML** v levém panelu

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Na stránce dialogové okno **Ověření SAML** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] Pokud chcete použít ověřování na základě SAML, měli byste buď mít **ověření domény** nebo **portál URL** nastavení. Nastavení portálu URL jedinečný název.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    na. Klasický portálu Azure AD zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .

    b. Klasická portálu Azure AD zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole Adresa URL **Odhlásit** .

    c. Je nepovinný krok **Změnit heslo adresu URL** tak nechejte prázdné.

    d. Klikněte na **Získat klíč ze souboru** vyberte stažený Skydesk e-mailový certifikát a klikněte na tlačítko **Otevřít** nahrát certifikát.

    e. **Algoritmus**vyberte **RSA**.

    f. Klikněte na **Ok** uložte změny.


7. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

8. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-skydesk-email-test-user"></a>Vytvoření uživatele test Skydesk e-mailu

V této části vytvoříte uživatele s názvem Britta Simon Skydesk e-mailem.

na. Klikněte na **Přístup uživatelů** z panelu vlevo v Skydesk e-mailu a potom zadejte svoje uživatelské jméno. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Pokud potřebujete k vytvoření hromadné uživatelů, budete muset kontaktovat tým podpory Skydesk e-mailu.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k e-mailu Skydesk.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Skydesk e-mailu, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
 
    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Skydesk e-mailu**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Skydesk e-mailu v panelu aplikace Access můžete by měla získat automaticky přihlášení z Skydesk e-mailové aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
