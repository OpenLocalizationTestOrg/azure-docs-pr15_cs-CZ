<properties
    pageTitle="Kurz: Azure Active Directory integrace s UserEcho | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a UserEcho."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Kurz: Azure Active Directory integrace s UserEcho

Cílem tohoto kurzu je předvedení UserEcho integrace se službou Azure Active Directory (Azure AD).  
Integrace UserEcho s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k UserEcho 
- Povolení uživatelům, aby automaticky získat přihlášení na k UserEcho (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí UserEcho, musíte následující položky:

- Předplatné Azure AD
- UserEcho jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání UserEcho z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-userecho-from-the-gallery"></a>Přidání UserEcho z Galerie
Pokud chcete nakonfigurovat integraci UserEcho do Azure AD, potřebujete přidat UserEcho z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat UserEcho z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **UserEcho**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)

7. V podokně výsledků vyberte **UserEcho**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí UserEcho založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému UserEcho uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v UserEcho jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v UserEcho.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s UserEcho, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření UserEcho testování uživatele](#creating-a-userecho-test-user)** – a protějšek Britta Simon UserEcho, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci UserEcho. 




**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s UserEcho, proveďte následující kroky:**

1. Na portálu na stránce **UserEcho** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k UserEcho** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci UserEcho (například: *https://fabrikam.UserEcho.com/*).

    b. Klikněte na tlačítko **Další**.
 
 
4. Na stránce **konfigurovat jednotné přihlašování v UserEcho** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


1. V jiném okně prohlížeče Přihlaste se k vašemu webu společnosti UserEcho jako správce.

1. Na panelu nástrojů na horní klikněte na své uživatelské jméno rozbalte nabídku a pak klikněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Klikněte na tlačítko **Integrace**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 


1. Klikněte na **Web**a potom klikněte na **jednotné přihlašování (SAML2)**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 


1. Na stránce **jednotné přihlašování (SAML)** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png) 

    na. Jako, **u kterých je povolené SAML**vyberte **Ano**. 

    b. Na portálu v konfigurovat jednotné přihlašování v UserEcho dialogovou stránku Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **URL SAML jednotné přihlašování** .

    c. Na portálu v konfigurovat jednotné přihlašování v UserEcho dialogovou stránku Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL vzdálené logoout** . 

    d. Otevřete stažený certifikát v poznámkovém bloku, zkopírujte obsah a potom je vložte do textového pole **X.509 certifikát** .    

    e. Klikněte na **Uložit**.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png)  

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png)  

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-userecho-test-user"></a>Vytvoření uživatele UserEcho test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v UserEcho.

**Vytvoření uživatele s názvem Britta Simon v UserEcho, proveďte následující kroky:**

1. Přihlášení k vašemu webu společnosti UserEcho jako správce.

1. Na panelu nástrojů na horní klikněte na své uživatelské jméno rozbalte nabídku a pak klikněte na **Nastavení**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

1. Klikněte na **uživatele**, rozbalte oddíl **uživatelů** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png) 

1. Klikněte na **uživatele**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png) 

1. Klikněte na **pozvat nového uživatele**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)


1. V dialogu **pozvat nového uživatele** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png) 

    na. Do textového pole **název** zadejte **Britta Simon**.

    b. **E-mailu** do textového pole zadejte jeho Britta e-mailovou adresu v portálu Azure klasické.

    c. Klikněte na **pozvánku**.

Britta, což umožňuje ji začít používat UserEcho se pošle Pozvánka. 



### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k UserEcho.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon UserEcho, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **UserEcho**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici UserEcho na panelu aplikace Access můžete by měla získat automaticky přihlášení z UserEcho aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png






