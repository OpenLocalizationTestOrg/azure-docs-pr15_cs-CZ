<properties
    pageTitle="Kurz: Azure Active Directory integrace s HackerOne | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HackerOne."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Kurz: Azure Active Directory integrace s HackerOne

V tomto kurzu HackerOne integrace se službou Azure Active Directory (Azure AD).

Integrace HackerOne s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k HackerOne
- Povolení uživatelům, aby automaticky získat přihlášení na k HackerOne (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí HackerOne, musíte následující položky:

- Předplatné Azure
- HackerOne jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu konfigurace a otestujte Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání HackerOne z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-hackerone-from-the-gallery"></a>Přidání HackerOne z Galerie
Integrovat HackerOne do Azure AD, potřebujete přidat HackerOne z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat HackerOne z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

![Aplikace][4]

6. Do pole Hledat zadejte **HackerOne**.

![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. V podokně výsledků vyberte **HackerOne**a potom klikněte na **Dokončit** přidat aplikaci.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Potom konfigurace a otestujte Azure AD jednotné přihlašování pomocí HackerOne založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD ví, co je databáze odpovídajících funkcí pro uživatele systému HackerOne uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v HackerOne jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v HackerOne.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s HackerOne, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření HackerOne testování uživatele](#creating-a-hackerone-test-user)** – a protějšek Britta Simon osvědčení toho, že je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Pak povolíte Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci HackerOne.

V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s HackerOne, proveďte následující kroky:**

1. Na portálu na stránce **HackerOne** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k HackerOne** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace HackerOne pomocí následujícího vzorce: **"https://hackerone.com/\<název společnosti\>/authentication"**. 

    b. Obraťte se na tým podpory HackerOne prostřednictvím [support@hackerone.com](mailto:support@hackerone.com) získat adresu URL klienta, pokud si nejste jisti.

    c. **Identifikátor URI** do textového pole zadejte adresu URL klienta. 

    d. Klikněte na tlačítko **Další**.


4. Na stránce **konfigurovat jednotné přihlašování v HackerOne** proveďte následující kroky a potom klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


1. Přihlášení k vašemu tenantovi HackerOne jako správce.

1. V nabídce na horní klikněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Přejděte na "**ověřování**" a klikněte na "**nastavení přidat SAML**".

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. V dialogovém okně **Nastavení SAML** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    na. Do textového pole **E-mailovou doménu** zadejte registrované domény.

    b. Na portálu Azure klasické zkopírujte **Jednoho přihlašování adresy URL služby**a potom je vložte do textového pole jeden znak na adresu URL.

    c. Vytvoření souboru **kódování base-64** z stažený certifikát.  

       >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a potom je vložte do **X509 certifikát** textové pole.

    e. Klikněte na tlačítko **Uložit**


1. V dialogovém okně Nastavení ověřování proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    na. Klikněte na tlačítko **Spustit test**.

    b. Pokud hodnota **Stav** poli rovná se **Poslední stav testu: vytvořili**, kontaktujte tým podpory HackerOne prostřednictvím [support@hackerone.com](mailto:support@hackerone.com) požádat o seznámit s konfigurací.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test

Dále vytvořte zkušební uživatele na portálu klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření zkušební uživatele ZABEZPEČENÉ PŘEDVÁDĚNÍ v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   


### <a name="creating-a-hackerone-test-user"></a>Vytvoření uživatele HackerOne test

Dále vytvořte uživatele s názvem Britta Simon v HackerOne. HackerOne podporuje za běhu zřizování, která je standardně zapnutá.

Neexistuje žádná akce položku, kterou v této části. Při přístupu HackerOne vytvoří se vytvoří nový uživatel ho ještě neexistuje. [Konfigurace Azure AD jednotné přihlášení](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Pokud potřebujete k vytvoření uživatele ručně, budete muset kontaktovat tým podpory certifikovat.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Pak povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k HackerOne.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon HackerOne, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **HackerOne**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Nakonec můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici HackerOne na panelu aplikace Access můžete by měla získat automaticky přihlášení z HackerOne aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png