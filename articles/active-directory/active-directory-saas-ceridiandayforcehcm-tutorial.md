<properties
    pageTitle="Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Ceridian Dayforce HCM."
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


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM

Cílem tohoto kurzu je vidíte, jak integrovat Ceridian Dayforce HCM s Azure Active Directory (Azure AD).  
Integrace Ceridian Dayforce HCM s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Ceridian Dayforce HCM
- Povolení uživatelům, aby automaticky získat přihlášení z k Ceridian Dayforce HCM (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure


Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Ceridian Dayforce HCM, musíte následující položky:

- Předplatné Azure AD
- Ceridian Dayforce HCM jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Ceridian Dayforce HCM z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Přidání Ceridian Dayforce HCM z Galerie
Pokud chcete nakonfigurovat integraci Ceridian Dayforce HCM do Azure AD, potřebujete přidat Ceridian Dayforce HCM z Galerie do seznamu spravované SaaS aplikace.

**Přidání Ceridian Dayforce HCM z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Ceridian Dayforce HCM**.

    ![Aplikace](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. V podokně výsledků vyberte **Ceridian Dayforce HCM**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem Tato část je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí HCM Dayforce Ceridian založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Ceridian Dayforce HCM uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Ceridian Dayforce HCM jinými slovy, je nutné stanovit.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Ceridian Dayforce HCM, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Ceridian Dayforce HCM testování uživatele](#creating-a-ceridian-dayforce-hcm-test-user)** – a protějšek Britta Simon Ceridian Dayforce HCM, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Ceridian Dayforce HCM.

Aplikace Ceridian Dayforce HCM očekává výrazy SAML v určitém formátu. Prosím Spolupracujte s týmem Dayforce HCM nejdřív k identifikaci identifikátor správné uživatele. Microsoft doporučuje používat atribut **"název"** jako identifikátor uživatele. Hodnota atributu v dialogovém okně **"Atrribute"** můžete spravovat. Následující obrázek ukazuje příklad pro tuto. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Ceridian Dayforce HCM, proveďte následující kroky:**




1. Na portálu na stránce integrace aplikace **Ceridian Dayforce HCM** v nabídce v horní části Azure klasické klepněte na položku **atributy**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. V seznamu **tokenu atributy saml** atributy vyberte atribut název a potom klikněte na **Upravit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. V dialogovém okně **Upravit atribut uživatele** proveďte následující kroky:
 
    na. **Hodnota atributu** seznamu vyberte atribut uživatele, který chcete použít pro implementaci.  
    Například pokud chcete použít číslo zaměstnance jako jedinečný identifikátor uživatele a které máte uložené v ExtensionAttribute2 hodnota atributu, vyberte **user.extensionattribute2**. 

    b. Klikněte na **dokončení**.  
    



1. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Na portálu na stránce **Ceridian Dayforce HCM** aplikace integrace Azure klasické klikněte na **Konfigurovat jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Ceridian Dayforce HCM** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno takto:.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané uživatelům přihlašování do aplikace Ceridian Dayforce HCM. Provozním prostředí použijte následující formát adresy URL:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Pro přehlednost nahraďte DayforcehcmNamespace obor názvů prostředí nebo ID. vaší společnosti Příklad:`https://sso.dayforcehcm.com/contoso`
    
    Test prostředí použijte následující formát adresy URL:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. V textovém poli **Odpovědět URL** zadejte adresu URL používané Azure AD pro vystavení odpovědi.  
    U provozním prostředí můžete pomocí:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    V prostředí test pomocí:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Na stránce **konfigurovat jednotné přihlašování v Ceridian Dayforce HCM** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci, kontaktujte tým podpory Ceridian Dayforce HCM prostřednictvím e-mailu, a poskytněte mu takto:

    - Soubor stažený certifikátu
    - **Adresa URL vydavatel**
    - Adresu **URL SAML jednotné přihlašování** 
    - **Jeden odhlásit adresy URL služby** 


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Vytvoření uživatele Ceridian Dayforce HCM test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Ceridian Dayforce HCM. 

Prosím pracovat se na tým podpory Ceridian Dayforce HCM získat uživatelé přidají do vašeho v aplikaci Ceridian Dayforce HCM. 





### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup Ceridian Dayforce HCM.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Ceridian Dayforce HCM, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Ceridian Dayforce HCM**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Ceridian Dayforce HCM na panelu aplikace Access můžete by měla získat automaticky přihlášení z Ceridian Dayforce HCM aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
