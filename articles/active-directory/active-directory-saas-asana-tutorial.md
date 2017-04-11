<properties
    pageTitle="Kurz: Azure Active Directory integrace s Asana | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Asana."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Kurz: Azure Active Directory integrace s Asana

V tomto kurzu se naučíte, jak integrovat Asana s Azure Active Directory (Azure AD).

Integrace Asana s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Asana
- Povolení uživatelům, aby automaticky získat přihlášení na k Asana (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Asana, musíte následující položky:

- Předplatné Azure AD
- **Asana** jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Asana z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-asana-from-the-gallery"></a>Přidání Asana z Galerie
Pokud chcete nakonfigurovat integraci Asana do Azure AD, potřebujete přidat Asana z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Asana z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Asana**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. V podokně výsledků vyberte **Asana**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Asana založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Asana je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Asana jinými slovy, je nutné stanovit.
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Asana.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Asana, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření Asana otestovat uživatele](#creating-an-Asana-test-user)** – a protějšek Britta Simon Asana, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Asana.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Asana, proveďte následující kroky:**

1. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování][6]
2. Na portálu klasické na stránce integrace **Asana** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][7] 

3. Na stránce **jakým způsobem uživatelé přihlásit k Asana** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    na. Přihlaste se na adresu URL do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://app.asana.com`

    c. Klikněte na tlačítko **Další**.

5. Na stránce **konfigurovat jednotné přihlašování v Asana** klikněte na **Stáhnout certifikát**a pak uložit soubor v počítači. Také zkopírujte hodnotu pro jednotné přihlašování SAML adresu URL.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Klikněte pravým tlačítkem na certifikát a potom otevřete soubor certifikátu pomocí programu Poznámkový blok nebo upřednostňované textového editoru. Kopírování obsahu mezi zahájit a názvem certifikát ukončit. Jedná se o X.509 certifikát, že použijete v Asana Konfigurace jednotného přihlašování.

6. V jiném okně prohlížeče přihlašování do aplikace Asana. Abyste mohli nakonfigurovat jednotné přihlašování v Asana, přístup k nastavení pracovního prostoru kliknutím na název pracovního prostoru v pravém horním rohu obrazovky. Klepněte na ** \<název pracovního prostoru\> nastavení**. 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. V okně **Nastavení organizace** klikněte na **Správa**. Klepněte na **členy musíte se přihlásit pomocí SAML** povolit Konfigurace jednotného přihlašování. Provést následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    na. V texbox **Přihlašovací adresa URL** vložte znak SAML na adresu URL z Azure AD.

    b. Do textového pole **Certifikátu X.509** vložte certifikátu X.509 jste zkopírovali z Azure AD.

9. Klikněte na **Uložit**. Pokud potřebujete další pomoc, přejděte na [Asana Průvodce pro nastavení jednotného přihlašování](https://asana.com/guide/help/premium/authentication#gl-saml) .

7. Přejděte na **konfigurovat jednotné přihlašování v Asana** stránku v Azure AD, vyberte potvrzení jednotné přihlašování a pak klikněte na **Další**.
    
    ![Azure AD jednotné přihlášení][10]

8. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-asana-test-user"></a>Vytvoření uživatele test Asana

V této části vytvoříte uživatele s názvem Britta Simon v Asana.

1. Na **Asana**přejděte k části **týmy** v levém panelu. Klikněte na tlačítko plus. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Zadejte e-mailu britta.simon@contoso.com do textového pole a potom vyberte **Pozvat**.
3. Klikněte na **Odeslat pozvánku**. Nový uživatel, dostanou e-mailu do svého e-mailového účtu. Uživatel bude třeba vytvořit a ověření účtu.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Asana.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Asana, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Asana**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu všichni uživatelé vyberte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je otestujte vaše Azure AD jednotného přihlašování.

Přejděte na stránku pro přihlášení Asana. V e-mailu textové pole Adresa vložte e-mailovou adresu britta.simon@contoso.com. Ponechat textového pole heslo na prázdné místo a potom klikněte na **Log In**. Budete přesměrovaní na stránku pro přihlášení Azure AD. Dokončení Azure AD pověření. Teď jste přihlášeni Asana.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
