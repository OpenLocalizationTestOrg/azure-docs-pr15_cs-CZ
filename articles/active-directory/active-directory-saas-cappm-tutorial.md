<properties
    pageTitle="Kurz: Azure Active Directory integrace s CA PPM | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PPM certifikační Autority."
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


# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a>Kurz: Azure Active Directory integrace s PPM certifikační Autorita

V tomto kurzu se naučíte, jak integrovat CA PPM s Azure Active Directory (Azure AD).

Integrace CA PPM s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k CA PPM
- Povolení uživatelům, aby automaticky získat podepsané on PPM CA (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí CA PPM, musíte následující položky:

- Předplatné Azure AD
- CA PPM jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání CA PPM z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-ca-ppm-from-the-gallery"></a>Přidání CA PPM z Galerie
Pokud chcete nakonfigurovat integraci CA PPM do Azure AD, potřebujete přidat CA PPM z Galerie do seznamu spravované SaaS aplikace.

**Přidání CA PPM z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **PPM certifikační Autority**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_01.png)
7. V podokně výsledků vyberte **CA PPM**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s CA PPM založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD vědět, co uživatel protějšek v CA PPM uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v CA PPM jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v CA PPM.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s CA PPM, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření CA PPM testování uživatele](#creating-an-ca-ppm-test-user)** – mít protějšek Britta Simon v PPM CA, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci PPM certifikační Autority.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s CA PPM, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **CA PPM** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce, **jakým způsobem uživatelé přihlásit k CA PPM** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_04.png) 

    na. **Identifikátor URI** do textového pole, zadejte adresu URL používá uživatelů k přihlašování k aplikaci CA PPM pomocí následujícího vzorce: **https://ca.ondemand.saml.20.post.\< Název společnosti\>**.
    
    b. V textovém poli **Odpovědět URL** zadejte **https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer**

    c. Klikněte na tlačítko **Další**
 
4. Na stránce **konfigurovat jednotné přihlašování v CA PPM** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci, kontaktujte [Technickou podporu CA](mailto:catechnicalsupport@ca.com) a dejte jí takto:

    • Stažený certifikát

    • **ID Entity.**


6. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cappm-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-ca-ppm-test-user"></a>Vytvoření uživatele test CA PPM

V této části vytvoříte uživatele s názvem Britta Simon v CA PPM. Zkontrolujte pracujte se na tým podpory CA PPM přidáte uživatele v platformu PPM certifikační Autority.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení přístup PPM certifikační Autority.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon CA PPM, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **PPM certifikační Autority**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici PPM certifikační Autority na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci PPM certifikační Autority.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_205.png
