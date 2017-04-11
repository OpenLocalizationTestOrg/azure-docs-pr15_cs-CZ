<properties
    pageTitle="Kurz: Azure Active Directory integrace s Pluralsight | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Pluralsight."
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


# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Kurz: Azure Active Directory integrace s Pluralsight

Cílem tohoto kurzu je předvedení Pluralsight integrace se službou Azure Active Directory (Azure AD).

Integrace Pluralsight s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Pluralsight
- Povolení uživatelům, aby automaticky získat přihlášení na k Pluralsight (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure


Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Pluralsight, musíte následující položky:

- Předplatné Azure
- Pluralsight jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Pluralsight z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-pluralsight-from-the-gallery"></a>Přidání Pluralsight z Galerie
Pokud chcete nakonfigurovat integraci Pluralsight do Azure AD, potřebujete přidat Pluralsight z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Pluralsight z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
 
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Pluralsight**.
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)

7. V podokně výsledků vyberte **Pluralsight**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Pluralsight založeného na uživateli testu s názvem "Britta Simon".

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Pluralsight, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Pluralsight testování uživatele](#creating-a-pluralsight-test-user)** – a protějšek Britta Simon Pluralsight, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Pluralsight.

Aplikace Pluralsight očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace SAML tokenu atributy. Následující obrázek ukazuje příklad pro tuto. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

Můžete také přidat atribut **"jedinečné ID"** příslušnou hodnotu jako číslo zaměstnance nebo něco jiného, které se hodí pro vaši organizaci. Navíc nezapomeňte, že se jedná není povinný atribut; však přidáte ho k identifikaci jedinečné uživatele. 

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Pluralsight, proveďte následující kroky:**

1. Na portálu na stránce integrace aplikace **Pluralsight** v nabídce v horní části Azure klasické klepněte na položku **atributy**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 



1. Odebrání nadbytečných **tokenu atributy SAML**, proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/2829.png) 


    na. Pro každý uživatel atribut v červeným ohraničením předchozí tabulce najeďte myší atribut a pak klikněte na odstranit. 




1. Chcete-li přidat požadované **SAML tokenu atributy**pro každý řádek v tabulce níže, proveďte následující kroky:

  	| Název atributu | Hodnota atributu |
  	| --- | --- |    
  	| Křestní jméno| User.givenName |
  	| Příjmení  | User.Surname |
  	| E-mailu | User.Mail |

    na. Klikněte na **Přidat uživatele atribut** otevřete dialogové okno **Přidat Attribure uživatele** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png) 


    b. Do textového pole **Attrubute název** napište název atribut zobrazí pro tento řádek.

    c. Ze seznamu **Hodnota atributu** selsect hodnota atributu zobrazí pro tento řádek.

    d. Klikněte na **dokončení**.  
    


1. Klikněte na **použít změny**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/3232.png)  



1. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  









1. Na portálu na stránce **Pluralsight** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Pluralsight** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno takto:.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 


    na. Přihlaste se na adresu URL do textového pole zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Pluralsight pomocí následujícího vzorce:`https://<instance name>.pluralsight.com/sso/<comapny name>`

    b. Klikněte na tlačítko **Další**.


4. Na stránce **konfigurovat jednotné přihlašování v Pluralsight** proveďte následující kroky:  ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png) 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci, kontaktujte tým Pluralsight [Odborné služby](mailTo:professionalservices@pluralsight.com) a poskytnout metadat stažený soubor.


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

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-pluralsight-test-user"></a>Vytvoření uživatele Pluralsight test

Cílem Tato část je vytvoření uživatele s názvem Britta Simon v Pluralsight. Prosím pracujte se na tým podpory Pluralsight přidáte uživatele v okně Pluralsight účet. 


> [AZURE.NOTE]Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory Pluralsight.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Pluralsight.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Pluralsight, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Pluralsight**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Pluralsight na panelu aplikace Access můžete by měla získat automaticky přihlášení z Pluralsight aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png
